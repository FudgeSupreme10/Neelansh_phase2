# Challenge 1 : Trivial Flag Transfer Protocol

> Figure out how they moved the flag.

## Solution

As the extension of the file was `.pcapng` I knew i had to open it up i wireshark to anaylze it, but another hint given in the file name was `TFTP`,so without analyzing the file i tried exporting files hidden inside that file by clicking on `File` and then `Export Objects` and then `TFTP`.
After extraction I got 6 Files, 2 were text files which seemed to be encoded, and three were images and one `.deb` file, First I looked at the `Instructions` File which had the following text : 
```
GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA
```
After trying out different ciphers i founf out that it was ROT13, so I went online and used a decoder to decode the text to find this written : 
```
TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN
```
Hmm, doesn't seem to be of much help i did the same with `plan` file to get this : 
```
VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF
```
which read the following,
```
IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS
```
According to the plan something was there in the images my first thought as usual was to check for hidden files in the image : 
```shell
┌──(kali㉿kali)-[~/Desktop/VM_Files/tftp]
└─$ steghide extract -sf picture1.bmp 
Enter passphrase: 
steghide: could not extract any data with that passphrase!
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/Desktop/VM_Files/tftp]
└─$ steghide extract -sf picture2.bmp  
Enter passphrase: 
steghide: could not extract any data with that passphrase!
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/Desktop/VM_Files/tftp]
└─$ steghide extract -sf picture3.bmp  
Enter passphrase: 
steghide: could not extract any data with that passphrase!
```
but it probably has some passphrase, i checked out some common passwords which didn't work, went back to the plan file and what looked odd to me was `DUEDILIGENCE` written in it. So I tried using it as the password and voila : 
```shell
┌──(kali㉿kali)-[~/Desktop/VM_Files/tftp]
└─$ steghide extract -sf picture1.bmp 
Enter passphrase: 
steghide: could not extract any data with that passphrase!
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/Desktop/VM_Files/tftp]
└─$ steghide extract -sf picture2.bmp 
Enter passphrase: 
steghide: could not extract any data with that passphrase!
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/Desktop/VM_Files/tftp]
└─$ steghide extract -sf picture3.bmp 
Enter passphrase: 
wrote extracted data to "flag.txt".
```
so I catted out `flag.txt` to get the flag and solved the challenge.

## Flag :
```
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```

## Resources Used : 
```
To Decoded ROT13 encoded text : https://cryptii.com/pipes/rot13-decoder
```

# Challenge 2 : tunn3l v1s10n

> We found this file. Recover the flag.

## Solution : 

I had no idea on what to do when i got the file, i used the file command to figure out what kind of file it was but it was of no help, so I then went on trying different things like opening or executing the file, but that didn't work, finally i checked the hex of the file, which gave me some insight.
```shell
┌──(kali㉿kali)-[~/Desktop/VM_Files/PicoCTF]
└─$ file tunn3l_v1s10n.bmp 
tunn3l_v1s10n.bmp: data
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/Desktop/VM_Files/PicoCTF]
└─$ xxd tunn3l_v1s10n | more
00000000: 424d 8e26 2c00 0000 0000 bad0 0000 bad0  BM.&,...........
00000010: 0000 6e04 0000 3201 0000 0100 1800 0000  ..n...2.........
00000020: 0000 5826 2c00 2516 0000 2516 0000 0000  ..X&,.%...%.....
00000030: 0000 0000 0000 231a 1727 1e1b 2920 1d2a  ......#..'..) .*
00000040: 211e 261d 1a31 2825 352c 2933 2a27 382f  !.&..1(%5,)3*'8/
```
so as known every file extension has a unique file header same for all it's file types, so I searched online what kind of file `424D` was, which then turned out to be a `.bmp` image file, but while checking if the header was correct for the file as it seemed corrupted, i found some irregularities in the header,following our the rules for a .bmp file header :
```
Byte 1,2 should be [0x4D 0x42]: 424D
Bytes 3-6 (Images Size) 00003032
Bytes 7,8 (Must be zero) 0000
Bytes 9,10 (Must be zero) 0000
Bytes 11-14 (Image offset) 00000036
Bytes 15-18 (size of BITMAPINFOHEADER structure, must be 40 [0x28]) 00000028
```
but our header didn't see to follow the rules, so using a [hex editor online](https://hexed.it/), I tried fixing the file's header, to see if that worked. so i changed the first line of the header from : 
```
00000000: 424d 8e26 2c00 0000 0000 bad0 0000 bad0  BM.&,...........
```
to this
```
00000000: 424d 8e26 2c00 0000 0000 3600 0000 2800  BM.&,.....6...(.
```
which made the image non corrupt, but on opening it I just saw `nottheflag`, so then again I tried the usual steps on a image, checkout if it's hiding any files inside it like the previous challenge or check it's metadata, and etc. I couldn't find anything, then i googled what all i things i can try and one option stuck out to me which was, changing the width and height of an image by changing it's hex values.
so i changed the hex value width and height of the image from `3201` to `5203`which seemed to work, so now the height of the image had increased significantly.
and the image now looked like this
<img width="818" height="614" alt="image" src="https://github.com/user-attachments/assets/2c439ca9-ba09-4f50-82f8-8a759e2b08c0" />

Flag : 
```
picoCTF{qu1t3_a_v13w_2020}
```
Resources Used : 
```
To edit hex data : https://hexed.it/
To find the bmp header and required rules for it : https://asecuritysite.com/forensics/bmp?file=router.bmp
```

# Challenge 3 : m00nwalk

> Decode this message from the moon.

## Solution : 

I used the first hint of the challenge to understand what I had to do but the only thing written in the hint was `How did pictures from the moon landing get sent back to Earth?`, so I just searched that on Google and apparently, Some thing called Slow-scan television (SSTV), was used so on searching if I could use it to decode the audio, apparently yes so I used [this](https://sstv-decoder.mathieurenaud.fr/) site to decode SSTV audio, on decoding it I got this image : 
<img width="320" height="256" alt="image" src="https://github.com/user-attachments/assets/10fc07e8-8ea4-48dd-806a-24a3d52f8544" />
which had the flag written, so pretty easy challenge.

## Flag : 
```
picoCTF{beep_boop_im_in_space}
```
