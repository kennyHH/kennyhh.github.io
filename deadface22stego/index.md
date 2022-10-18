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

![doggoflag](/images/deadface22stego/doggosteghide.png "Doggo Flag")

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

![glitchframebrowser](/images/deadface22stego/stegsolvegif.png "Frame Browser")

We receive an HIT in frame 23/80! Although that flag is visible, it is difficult to read.


![glitchflag](/images/deadface22stego/glitchflag.png "Glitch flag")



Flag: `flag{c0rrupt3d}`

---

## Holiday Nesting Doll


`POINTS : 50`

>Holidays are a headache and this one is no different. The flag is fractured, can you find it? TIME is almost up! Santa is coming!

We get a `zip` file with 3 images inside it . 

![files](/images/deadface22stego/nestingfiles.png "Files")

Let's get to work !

I've run strings on all 3 files , but no results . So let's try `stegseek` with `rockyou.txt` passlist !

```bash
stegseek -sf Scary1.jpg -wl /opt/rockyou.txt
stegseek -sf Scary2.jpg -wl /opt/rockyou.txt
stegseek -sf Scary3.jpg -wl /opt/rockyou.txt
```

![stegseek](/images/deadface22stego/scary1seek.png "StegSeek")

No luck on 1st image, but we've found passwords for 2nd and 3rd one ! YEAAAAAAAAAH

Extraction of the hidden data from the pictures provided us with 2 new images. I wonder what might be in them.

![scarynewfiles](/images/deadface22stego/scarynewfiles.png "New files")

I've moved old pictures to `/old` folder , and changed the extensions from `.jpg.out` to `.jpg`

Run StegSeek once more, but this time it failed to provide the passwords.

Let's try good old `strings`.

![scarystrings2](/images/deadface22stego/scary2strings.png "Scary 2 strings")
![scarystrings3](/images/deadface22stego/scary3strings.png "Scary 2 strings")

I've uncovered two flags, however they appear to be encoded using some cipher. It's CyberChef time!

![scarychef](/images/deadface22stego/scarychef1.png "CyberChef")


`flag{veF9pbmFib3hfaW5hYm94X2luYWJve}` > `ox_inabox_inabox_inabo`
`flag{ZmxhZ3tpbmFib3hfaW5hYm94X2luYWJ}` > `flag{inabox_inabox_inab`

1st flag was encoded using Base64 and rotated right twice. 
2nd flag was encoded using Base64.

After trying for five minutes to find the remaining piece of the flag, I gave up. I started to wonder if it was the same word repeated a few times.

I submitted the flag: `flag{inabox_inabox_inabox_inabox_inabox_inabox_inabox_inabox}` and it worked !

![pikachu](https://media.tenor.com/UZJd1pjj4NMAAAAC/surprised-pikachu.gif)

I've missed the message on Discord server that the 3rd flag was missing from the picture. No big deal, it happens.

![scarydiscord](/images/deadface22stego/scarydiscord.png)


Flag: `flag{inabox_inabox_inabox_inabox_inabox_inabox_inabox_inabox}`

---

## Missing home


`POINTS : 100`

>We intercepted this email being passed by a DEADFACE member. We tried some steghide, but that didn't work. Help us out here!

We get this image in the challenge.

![scarydiscord](/images/deadface22stego/missinghome.jpg "Missing Home")

The description claims that they attempted to run `steghide` on the file but that it failed; however, just to be sure, I will try `stegseek` with usual `rockyou.txt` passlist.

![Steghide](/images/deadface22stego/missinghSteghide.png "Steghide")

Well, that was expected.

Let's try using `binwalk` to see if that image contains any hidden files.

![binwalk](/images/deadface22stego/missinghBinwalk.png "Binwalk")

**BINGO !**

Now we need to extract that data.

```bash
binwalk --dd='.*' missinghome.jpg 
```

![binwalk](/images/deadface22stego/missinghBinwalkExtract.png "Binwalk Extract")

We obtained a file named `2C573` from the `binwalk` extraction that resembles a flag, but when we view it in `Ristretto Image Viewer` a different image is displayed.

![ristretto](/images/deadface22stego/missinghRistretto.png "Ristretto") 

The flag is most likely hidden in the thumbnail.

There are many ways and tools to extract it , but we can just simply zoom in the folder.

{{< admonition type=tip title="TIP" open=true >}}
You can use `exiftool` to extract the thumbnail.
```bash
exiftool -b -ThumbnailImage 2C573 > thumbnail.jpg
```
{{< /admonition >}}


![missingHflag](/images/deadface22stego/missinghflag.png "Missing Home Flag")

**Voilà !**

Flag: `flag{s3cr3ts_don'T_stAy_bur13d_L0nG}`

---

## The Farm


`POINTS : 100`

>We found this video on one of the laptops recovered from Lytton Lab's secret lab area in the boiler room of the old Lytton, IN school building. It is very strange, cryptic, and unsettling.
>
>This video starts as a simple visit to a farm, but it quickly takes a dark turn when the videographer visits an old abandoned house deep in the woods at night with a mysterious companion.
>
>There is a strange figure appearing to convey a secret message. Recover the message and enter it as the flag.
>
>Enter the flag in all caps, with no spaces or punctuation, like this: flag{ALLCAPSNOSPACES}.
>
>Pick one of the following video files based on your bandwidth and processing power. They are the same video with different resolutions. The videos are MP4 H.264 format.


This challenge is different from previous challenges since we receive `.mp4` file instead of usual image.

Given that Halloween is quickly approaching, the video's mysterious and eerie nature is fantastic.

I've scrolled through the video and found some weird noise around `12:12` minute.

At `13:59` minutes, I also heard some beeps that resembled morse code.

Let's get to work and export the audio portion of the video using `ffmpeg`.

```bash
ffmpeg -i farm.mp4 farm.mp3
```

Let's put that `mp3` file into `SonicVisualiser` and turn on `Spectogram`.

![farm](/images/deadface22stego/farmMorse.png "Farm morse Code")

It's a morse code, as I had suspected. 

All that's left to do is enter the information into an online decoder to decipher it.

![farm](/images/deadface22stego/farmMorseDecoded.png "Farm morse Decoded") 

We get a flag and sweet points. 


Flag: `flag{IWILLBEAVENGEDSEVENFOLD}`
