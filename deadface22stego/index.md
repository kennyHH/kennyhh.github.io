# DeadFace 2022 Stegonography


# DeadFace 2022 Forensics 



---

## The Goodest Boy


`POINTS : 20`

>We found this image on Ghost Town. We think bumpyhassan hid some information here. Can you see what information he hid?

In this challenge we get a picture of a dog with some data hidden in it.


![doggo](/images/deadface22stego/doggo.jpg)


First thing we do is check if it's a JPG file.


![doggofilecheck](/images/deadface22stego/filedoggo.png "Doggo File Check")


Looks like it's a proper `jpg` file . 

Next we use strings to look for any clues.


```bash
strings doggo.jpg
``` 

![doggostrings](/images/deadface22stego/doggostrings.png "")


IT'S A HIT ! We found the password which we can use to extract the hidden data.

`Password: borkbork`


So now we need to run steghide to extract the data !


```bash
steghide --extract -sf doggo.jpg
```


![doggosteghide](/images/deadface22stego/doggosteghide.png "Doggo Steghide")



File `itsasecret.pdf` was hidden inside the picture !

We open it and find the flag !

![doggoflag](/images/deadface22stego/doggoflag.png "Doggo Flag")

Flag: `flag{whos_A_g00d_boi_bork_bork}`

---

## Eye Know ,Do You ?


`POINTS : 20`


Flag: `flag{xxx}`

---

## Life's a Glitch


`POINTS : 50`


Flag: `flag{xxx}`

---

## Holiday Nesting Doll


`POINTS : 50`


Flag: `flag{xxx}`

---

## Missing home


`POINTS : 100`


Flag: `flag{xxx}`

---

## The Farm


`POINTS : 100`


Flag: `flag{xxx}`
