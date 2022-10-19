# DeadFace 2022 Forensics



# DeadFace 2022 Forensics 

![firststrike](/images/deadface.png)



## First strike


`POINTS : 10`


{{< admonition type=info title="Challenge Description" open=true >}}

There was a cyberattack on ESU's website on July 27th. ESU IT staff collected data from the attack and need your help sifting through it.

What IP Address did the attack originate from?

Submit the flag as: flag{ip_address}. Example: flag{192.168.1.1}.

{{< /admonition >}}




In this challenge we have to find IP of the attacker using files `access.log` and `error.log`.

This one is very easy. When we open the "access.log" file, the IP address is immediately visible in first line.

![firststrike](/images/deadface22forensics/first%20strike.png "First Strike")

Flag: `flag{165.227.73.138}`

---

##  ToolBox


`POINTS : 15`

{{< admonition type=info title="Challenge Description" open=true >}}

What tool was used when the attack started at 2022-07-27 14:13 UTC?

Submit the flag as flag{tool}. Example: flag{notepad}.

Use the files from First Strike.

{{< /admonition >}}




In line `3` in `access.log` we can see that attacker used NMAP to scan the website.

![nmap](/images/deadface22forensics/nmap.png "NMAP")

Flag: `flag{NMAP}`

---

## Grave Digger 1

`POINTS 15`

{{< admonition type=info title="Challenge Description" open=true >}}

Turbo Tactical has gained access to a machine owned by DEADFACE. It appears crypto_vamp, a new recruit at DEADFACE, used a weak password for his account on d34th's machine. See if you can find the flag associated with "Grave Digger 1"

`Host :   env.deadface.io
Password: 123456789q`

{{< /admonition >}}


I didn't solve that challenge in time , because I didn't think about looking at `env`. While looking for this flag I've found a flag for `Grave Digger 2` by accident.

We log in to the machine using credentials:

`Login : crypto_vamp`
`Password: 123456789q`

After logging in we just use command `env` and we get the flag.

![gravedigger1](/images/deadface22forensics/gravedigger1.png "Grave Digger1")

Flag: `flag{d34dF4C3_en1roN_v4r}`

---

## Agents of Chaos

`POINTS 25`

{{< admonition type=info title="Challenge Description" open=true >}}

What is the first user agent of the second scanning tool used by the attacker?

Submit the flag as flag{user agent string}.

Use the files from First Strike.

{{< /admonition >}}




In this challenge we need to find a user-agent strings from 2nd tool used by the attacker.

We can notice a new tool being used on line `70` of the same file we used for the previous challenges.

![toolbox](/images/deadface22forensics/agentofchaos.png "Agent of Chaos")

Flag: `flag{Mozilla/5.00 (Nikto/2.1.6) (Evasions:None) (Test:Port Check)}`

---

## Iterations

`POINTS 25`

{{< admonition type=info title="Challenge Description" open=true >}}

What is the name of the tool that likely resulted in DEADFACE acquiring login credentials to ESU's website?

Submit the flag as flag{tool}.

Use the files from First Strike.

{{< /admonition >}}



We can see that the attacker has used a new tool to brute-force the `login.php` page's credentials in line `5881`.

![hydra](/images/deadface22forensics/hydra.png "Hydra")

Flag: `flag{Hydra}`

---

## Submission

`POINTS 40`

{{< admonition type=info title="Challenge Description" open=true >}}

What artifact did DEADFACE place onto ESU's website to gain access to the filesystem?

Submit the flag as flag{filename}.

Use the files from First Strike.

{{< /admonition >}}



Near the end of the `access.log` in line `7442` we see that attacker uploaded the file and then accessed it.

![submission](/images/deadface22forensics/submission.png "Submission")

Flag: `flag{info.php}`




