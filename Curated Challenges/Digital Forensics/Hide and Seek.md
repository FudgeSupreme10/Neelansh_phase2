# Hide and Seek
>Sakamoto’s at it again with a game of hide and seek, but this time, it’s not with Shin or his daughter. An old friend hid some secret data in this image. Can you find it before the others do?
>
>Hint: Even in retirement, Sakamoto never loses at hide and seek. Maybe stegseek can help you keep up.

## Solution :
This was a pretty easy challenge, the solution was given in the hint itself, I just had to use stegseek to get the password from the image file and extract the hidden file in the image to get the flag

```shell
┌──(kali㉿kali)-[~/Desktop/VM_Files/curated/hideandseek]
└─$ stegseek sakamoto.jpg ../../rockyou.txt
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "iloveyou1"
[i] Original filename: "flag.txt".
[i] Extracting to "sakamoto.jpg.out".
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/Desktop/VM_Files/curated/hideandseek]
└─$ cat sakamoto.jpg.out 
nite{h1d3_4nd_s33k_but_w1th_st3g_sdfu9s8}
```

# Flag : 
```
nite{h1d3_4nd_s33k_but_w1th_st3g_sdfu9s8
```
