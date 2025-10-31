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

# Challenge 2 : 
