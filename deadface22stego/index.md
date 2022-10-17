# DeadFace 2022 Stegonography


# DeadFace 2022 Forensics 



---

## The Goodest Boy


`POINTS : 20`

>We found this image on Ghost Town. We think bumpyhassan hid some information here. Can you see what information he hid?

In this challenge we get a picture of a dog with some data hidden in it.

![doggo](/images/deadface22stego/doggo.jpg "Doggo")

First thing we do is check if it's a JPG file.

![doggofilecheck](/images/deadface22stego/filedoggo.png "Doggo File Check")

Looks like it's a proper `jpg` file . 

Next we use strings to look for any clues.
```bash
strings doggo.jpg
``` 

![doggostrings](/images/deadface22stego/doggostrings.png "Doggo String Check")

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

>lamia415 sent a warning email to other De Monne employees with an image attached. The employees, however, can't figure out what the intended message>is. Can you reveal the message and find the flag?

This is the image we receive in this challenge. Let's get to work.

![eye](/images/deadface22stego/All_Seeing_Eye.jpg "Stego Image")

At first I've used strings and binwalk on it , but didn't get any meaningful information from it. Soooo let's use great tool called StegSolve to find some clues!

![eyestegsolve](/images/deadface22stego/eyestegsolve.png "Steg Solve")

Analyse options such as File Format, Data Extract and the rest didn't reveal anything useful so I've started flipping through different options using arrows underneath. 

![eyeflag](/images/deadface22stego/eyeflag.png "Flag")

We've got a hit! The flag was most visible on Random Colour map 2 !



Flag: `flag{Deadface_Knows_All_Sees_All}`

---

## Life's a Glitch

`POINTS : 50`

>Another one of De Monne's employees was compromised. DEADFACE left a GIF image of what looks like a glitched face. They claim there is a flag in the
>GIF. See if you can use your repertoire of tools to find the flag hidden in the GIF.

In this challenge we get a gif file instead of usual jpg. It's safe to assume that flag is hidden in one of the frames.


![glitch](/images/deadface22stego/glitchedout.gif "Glitch gif")


Once again , we use StegSolve to find some useful information.


We go to Analyse > Frame Browser and start flipping throgh the frames.


![glitchframebrowser](/images/deadface22stego/stegsolvedif.png "Frame Browser")


We receive an HIT in frame 23/80! Although that flag is visible, it is difficult to read.


![glitchflag](/images/deadface22stego/glitchflag.png "Glitch flag")



Flag: `flag{c0rrupt3d}`

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
