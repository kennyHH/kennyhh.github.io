# DeadFace 2022 Forensics



# DeadFace 2022 Forensics 

## First strike


`POINTS : 10`

>There was a cyberattack on ESU's website on July 27th. ESU IT staff collected data from the attack and need your help sifting through it.
>
>What IP Address did the attack originate from?
>
>Submit the flag as: flag{ip_address}. Example: flag{192.168.1.1}.
>
>Download access.log
>SHA1: b71d59350a6802b9fc1b772a76388ca250ba6d89
>
>Download error.log
>SHA1: 8fc2c50a21e7133c319c6bb4df597cc707f21c91

In this challenge we have to find IP of the attacker using files `access.log` and `error.log`.

This one is very easy. When we open the "access.log" file, the IP address is immediately visible in first line.



Flag: `flag{165.227.73.138}`

---

##  ToolBox


`POINTS : 15`

>What tool was used when the attack started at 2022-07-27 14:13 UTC?
>
>Submit the flag as flag{tool}. Example: flag{notepad}.
>
>Use the files from First Strike.

On third line in `access.log` we can see that attacker used NMAP to scan the website.


Flag: `flag{NMAP}`

---

## Grave Digger 1

`POINTS 15`

>Turbo Tactical has gained access to a machine owned by DEADFACE. It appears crypto_vamp, a new recruit at DEADFACE, used a weak password for his account on d34th's machine. See if you can find the flag associated with "Grave Digger 1"
>
>env.deadface.io
>Password: 123456789q

I didn't solve that challenge in time , because I didn't think about looking at `env`. While looking for this flag I've found a flag for `Grave Digger 2` by accident.

We log in to the machine using credentials:

`Login : crypto_vamp`
`Password: 123456789q`

After logging in we just use command `env` and we get the flag.


Flag: `flag{d34dF4C3_en1roN_v4r}`

---

## Agents of Chaos

>What is the first user agent of the second scanning tool used by the attacker?
>
>Submit the flag as flag{user agent string}.
>
>Use the files from First Strike.

In this challenge we need to find a user-agent strings from 2nd tool used by the attacker.

We can notice a new tool being used on line 70 of the same file we used for the previous challenges.



Flag: `flag{Mozilla/5.00 (Nikto/2.1.6) (Evasions:None) (Test:Port Check)}`

---

## Iterations

>What is the name of the tool that likely resulted in DEADFACE acquiring login credentials to ESU's website?
>
>Submit the flag as flag{tool}.
>
>Use the files from First Strike.

We can see that the attacker has used a new tool to brute-force the `login.php` page's credentials in line `5881`.


Flag: `flag{Hydra}`

---

## Submission

>What artifact did DEADFACE place onto ESU's website to gain access to the filesystem?
>
>Submit the flag as flag{filename}.
>
>Use the files from First Strike.

Near the end of the `access.log` in line `7442` we see that attacker uploaded the file and then accessed it.

Flag: `flag{info.php}`


