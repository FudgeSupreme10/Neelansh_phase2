# RAR of the Abyss
>Two philosophers peer into the networked abyss and swap a secret. Use the secret to decrypt the Abyss’ RAwR and pull your flag from the void.

## Solution :

This was a pretty straight forward challenge, I saw that the given file had a .pcap extension, but the description showed that we need to get a rar file and uncompress it with a secret, so I went over to the pcap file and opened it with wireshark, and analyzed the ascii until I found the password :
```
Frame 28: 84 bytes on wire (672 bits), 84 bytes captured (672 bits)
Ethernet II, Src: Microsoft_d9:a7:81 (00:15:5d:d9:a7:81), Dst: Broadcast (ff:ff:ff:ff:ff:ff)
Internet Protocol Version 4, Src: 10.0.0.20, Dst: 10.0.0.10
Transmission Control Protocol, Src Port: 53005, Dst Port: 80, Seq: 1, Ack: 1, Len: 30
Hypertext Transfer Protocol
    Nietzsche: b3y0ndG00dand3vil\r\n
```
After finding the password the next step was pretty straight forward as the pcap file had a rar inside it so I directly just opened the pcap file using WinRAR, it asked for the password which we already had so I directly just entered it and got flag.txt which gave me the flag

<img width="801" height="147" alt="image" src="https://github.com/user-attachments/assets/952489e0-4c75-4332-834b-bc423609b1d7" />

Flag : 
```
nite{thus_sp0k3_th3_n3tw0rk_f0r3ns1cs_4n4lyst}
```
