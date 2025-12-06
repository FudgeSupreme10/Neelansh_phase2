# Re:Draw
>Her screen went black and a strange command window flickered to life, lines of text flashed across before everything went silent. Moments later, the system crashed. By sheer luck, we recovered a memory dump. 
>
>Note: There are three stages to this challenge and you will find three flags.
>
>What we know: just before the crash, a black command window flickered across the screen, something in its output might still be visible if you dig through memory. She was drawing when it happened, and remnants of a painting program linger, which could reveal more if inspected in the right way. Finally, a mysterious archive hides deeper in memory, likely holding the last piece of her work.
>
>Hint: Learn up on volatility 2 and its various plugins and what they are used for.

## Solution :
Ok this is something I have never done before so kind of a fun challenge for me Memory Forensics, so According to hint given I need to use `Volatility 2` to get the flags, here there are 3 parts so 3 flags, so For the 1st part

### 1st Part : CMD

The first part clearly says `black command window` which is the cmd so the first thing we do is directly use volatility 2 to find the profile used, which here is `Win7SP1x64`, we can find this by doing the following :
```cmd
┌──(kali㉿kali)-[/mnt/…/VM_Files/curated/Redraw/volatility]
└─$ python2 vol.py -f ../MemoryDump_Lab1.raw imageinfo
Volatility Foundation Volatility Framework 2.6.1
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_24000, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_24000, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/mnt/hgfs/VM_Files/curated/Redraw/MemoryDump_Lab1.raw)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf800028100a0L
          Number of Processors : 1
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff80002811d00L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2019-12-11 14:38:00 UTC+0000
     Image local date and time : 2019-12-11 20:08:00 +0530
```

Which gives use the Profile : `Win7SP1x64`, now we need to use that to see what the user did in the cmd, to do that we use the consoles plugin, which does the following :
```
consoles                   - Extract command history by scanning for _CONSOLE_INFORMATION
```
so now we use the consoles plugin on the above mentioned profile to see the user's command history :
```cmd
┌──(kali㉿kali)-[/mnt/…/VM_Files/curated/Redraw/volatility]
└─$ python2 vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 consoles
Volatility Foundation Volatility Framework 2.6.1
**************************************************
ConsoleProcess: conhost.exe Pid: 2692
Console: 0xff756200 CommandHistorySize: 50
HistoryBufferCount: 1 HistoryBufferMax: 4
OriginalTitle: %SystemRoot%\system32\cmd.exe
Title: C:\Windows\system32\cmd.exe - St4G3$1
AttachedProcess: cmd.exe Pid: 1984 Handle: 0x60
----
CommandHistory: 0x1fe9c0 Application: cmd.exe Flags: Allocated, Reset
CommandCount: 1 LastAdded: 0 LastDisplayed: 0
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
Cmd #0 at 0x1de3c0: St4G3$1
----
Screen 0x1e0f70 X:80 Y:300
Dump:
Microsoft Windows [Version 6.1.7601]                                            
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.                 
                                                                                
C:\Users\SmartNet>St4G3$1                                                       
ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=                                        
Press any key to continue . . .                                                 
**************************************************
ConsoleProcess: conhost.exe Pid: 2260
Console: 0xff756200 CommandHistorySize: 50
HistoryBufferCount: 1 HistoryBufferMax: 4
OriginalTitle: C:\Users\SmartNet\Downloads\DumpIt\DumpIt.exe
Title: C:\Users\SmartNet\Downloads\DumpIt\DumpIt.exe
AttachedProcess: DumpIt.exe Pid: 796 Handle: 0x60
----
CommandHistory: 0x38ea90 Application: DumpIt.exe Flags: Allocated
CommandCount: 0 LastAdded: -1 LastDisplayed: -1
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
----
Screen 0x371050 X:80 Y:300
Dump:
  DumpIt - v1.3.2.20110401 - One click memory memory dumper                     
  Copyright (c) 2007 - 2011, Matthieu Suiche <http://www.msuiche.net>           
  Copyright (c) 2010 - 2011, MoonSols <http://www.moonsols.com>                 
                                                                                
                                                                                
    Address space size:        1073676288 bytes (   1023 Mb)                    
    Free space size:          24185389056 bytes (  23064 Mb)                    
                                                                                
    * Destination = \??\C:\Users\SmartNet\Downloads\DumpIt\SMARTNET-PC-20191211-
143755.raw                                                                      
                                                                                
    --> Are you sure you want to continue? [y/n] y                              
    + Processing...
```
So here if we anaylze a command which gave him something in base64 :
```cmd
C:\Users\SmartNet>St4G3$1                                                       
ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=
```
On decoding it we get our first flag : 

### Flag 1 :
```
flag{th1s_1s_th3_1st_st4g3!!}
```
## Part 2 : Ms Paint

The next part stated that she was using some `painting program`, so I used the `pslist` plugin to list the processes running.
```cmd
┌──(kali㉿kali)-[/mnt/…/VM_Files/curated/Redraw/volatility]
└─$ python2 vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 pslist                                                
Volatility Foundation Volatility Framework 2.6.1
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                          
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------
0xfffffa8000ca0040 System                    4      0     80      570 ------      0 2019-12-11 13:41:25 UTC+0000                                 
0xfffffa800148f040 smss.exe                248      4      3       37 ------      0 2019-12-11 13:41:25 UTC+0000                                 
0xfffffa800154f740 csrss.exe               320    312      9      457      0      0 2019-12-11 13:41:32 UTC+0000                                 
0xfffffa8000ca81e0 csrss.exe               368    360      7      199      1      0 2019-12-11 13:41:33 UTC+0000                                 
0xfffffa8001c45060 psxss.exe               376    248     18      786      0      0 2019-12-11 13:41:33 UTC+0000                                 
0xfffffa8001c5f060 winlogon.exe            416    360      4      118      1      0 2019-12-11 13:41:34 UTC+0000                                 
0xfffffa8001c5f630 wininit.exe             424    312      3       75      0      0 2019-12-11 13:41:34 UTC+0000                                 
0xfffffa8001c98530 services.exe            484    424     13      219      0      0 2019-12-11 13:41:35 UTC+0000                                 
0xfffffa8001ca0580 lsass.exe               492    424      9      764      0      0 2019-12-11 13:41:35 UTC+0000                                 
0xfffffa8001ca4b30 lsm.exe                 500    424     11      185      0      0 2019-12-11 13:41:35 UTC+0000                                 
0xfffffa8001cf4b30 svchost.exe             588    484     11      358      0      0 2019-12-11 13:41:39 UTC+0000                                 
0xfffffa8001d327c0 VBoxService.ex          652    484     13      137      0      0 2019-12-11 13:41:40 UTC+0000                                 
0xfffffa8001d49b30 svchost.exe             720    484      8      279      0      0 2019-12-11 13:41:41 UTC+0000                                 
0xfffffa8001d8c420 svchost.exe             816    484     23      569      0      0 2019-12-11 13:41:42 UTC+0000                                 
0xfffffa8001da5b30 svchost.exe             852    484     28      542      0      0 2019-12-11 13:41:43 UTC+0000                                 
0xfffffa8001da96c0 svchost.exe             876    484     32      941      0      0 2019-12-11 13:41:43 UTC+0000                                 
0xfffffa8001e1bb30 svchost.exe             472    484     19      476      0      0 2019-12-11 13:41:47 UTC+0000                                 
0xfffffa8001e50b30 svchost.exe            1044    484     14      366      0      0 2019-12-11 13:41:48 UTC+0000                                 
0xfffffa8001eba230 spoolsv.exe            1208    484     13      282      0      0 2019-12-11 13:41:51 UTC+0000                                 
0xfffffa8001eda060 svchost.exe            1248    484     19      313      0      0 2019-12-11 13:41:52 UTC+0000                                 
0xfffffa8001f58890 svchost.exe            1372    484     22      295      0      0 2019-12-11 13:41:54 UTC+0000                                 
0xfffffa8001f91b30 TCPSVCS.EXE            1416    484      4       97      0      0 2019-12-11 13:41:55 UTC+0000                                 
0xfffffa8000d3c400 sppsvc.exe             1508    484      4      141      0      0 2019-12-11 14:16:06 UTC+0000                                 
0xfffffa8001c38580 svchost.exe             948    484     13      322      0      0 2019-12-11 14:16:07 UTC+0000                                 
0xfffffa8002170630 wmpnetwk.exe           1856    484     16      451      0      0 2019-12-11 14:16:08 UTC+0000                                 
0xfffffa8001d376f0 SearchIndexer.          480    484     14      701      0      0 2019-12-11 14:16:09 UTC+0000                                 
0xfffffa8001eb47f0 taskhost.exe            296    484      8      151      1      0 2019-12-11 14:32:24 UTC+0000                                 
0xfffffa8001dfa910 dwm.exe                1988    852      5       72      1      0 2019-12-11 14:32:25 UTC+0000                                 
0xfffffa8002046960 explorer.exe            604   2016     33      927      1      0 2019-12-11 14:32:25 UTC+0000                                 
0xfffffa80021c75d0 VBoxTray.exe           1844    604     11      140      1      0 2019-12-11 14:32:35 UTC+0000                                 
0xfffffa80021da060 audiodg.exe            2064    816      6      131      0      0 2019-12-11 14:32:37 UTC+0000                                 
0xfffffa80022199e0 svchost.exe            2368    484      9      365      0      0 2019-12-11 14:32:51 UTC+0000                                 
0xfffffa8002222780 cmd.exe                1984    604      1       21      1      0 2019-12-11 14:34:54 UTC+0000                                 
0xfffffa8002227140 conhost.exe            2692    368      2       50      1      0 2019-12-11 14:34:54 UTC+0000                                 
0xfffffa80022bab30 mspaint.exe            2424    604      6      128      1      0 2019-12-11 14:35:14 UTC+0000                                 
0xfffffa8000eac770 svchost.exe            2660    484      6      100      0      0 2019-12-11 14:35:14 UTC+0000                                 
0xfffffa8001e68060 csrss.exe              2760   2680      7      172      2      0 2019-12-11 14:37:05 UTC+0000                                 
0xfffffa8000ecbb30 winlogon.exe           2808   2680      4      119      2      0 2019-12-11 14:37:05 UTC+0000                                 
0xfffffa8000f3aab0 taskhost.exe           2908    484      9      158      2      0 2019-12-11 14:37:13 UTC+0000                                 
0xfffffa8000f4db30 dwm.exe                3004    852      5       72      2      0 2019-12-11 14:37:14 UTC+0000                                 
0xfffffa8000f4c670 explorer.exe           2504   3000     34      825      2      0 2019-12-11 14:37:14 UTC+0000                                 
0xfffffa8000f9a4e0 VBoxTray.exe           2304   2504     14      144      2      0 2019-12-11 14:37:14 UTC+0000                                 
0xfffffa8000fff630 SearchProtocol         2524    480      7      226      2      0 2019-12-11 14:37:21 UTC+0000                                 
0xfffffa8000ecea60 SearchFilterHo         1720    480      5       90      0      0 2019-12-11 14:37:21 UTC+0000                                 
0xfffffa8001010b30 WinRAR.exe             1512   2504      6      207      2      0 2019-12-11 14:37:23 UTC+0000                                 
0xfffffa8001020b30 SearchProtocol         2868    480      8      279      0      0 2019-12-11 14:37:23 UTC+0000                                 
0xfffffa8001048060 DumpIt.exe              796    604      2       45      1      1 2019-12-11 14:37:54 UTC+0000                                 
0xfffffa800104a780 conhost.exe            2260    368      2       50      1      0 2019-12-11 14:37:54 UTC+0000
```
In this I notice `mspaint.exe` is also running with PID : `2424`, so we can dump whatever was open to a folder of our choice using `memdump` 
```cmd
──(kali㉿kali)-[/mnt/…/VM_Files/curated/Redraw/volatility]
└─$ python2 vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 memdump -p 2424 -D dumps
Volatility Foundation Volatility Framework 2.6.1
************************************************************************
Writing mspaint.exe [  2424] to 2424.dmp
```
this gave me a .dmp file which I converted to .data to edit using GIMP, after opening the file with GIMP and tweaking it for like 15 mins I finally got the clear flag in the image.

<img width="1918" height="979" alt="image" src="https://github.com/user-attachments/assets/07c2ec24-7185-4418-a1e3-b005ebed6aaf" />

### Flag 2 : 
```
flag{Good_BoY_good_girl}
```

### Part 3 : Zip Archive file

The third part tells us about a archive file hidden deep within the memory to find it I used filescan plugin to first find the user who was `Alissa Simpson` then I grepped it to find the files made by her :
```cmd
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[/mnt/…/VM_Files/curated/Redraw/volatility]
└─$ python2 vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 filescan | grep "Alissa Simpson"
Volatility Foundation Volatility Framework 2.6.1
0x000000003e8105e0      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Links\desktop.ini
0x000000003e811220      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Favorites\desktop.ini
0x000000003e811370      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Downloads\desktop.ini
0x000000003e817070      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Favorites\Links for United States\desktop.ini
0x000000003e83b630      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Pictures\desktop.ini
0x000000003e852070      2      1 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\NTUSER.DAT{016888bd-6c6f-11de-8d1d-001e0bcde3ec}.TM.blf
0x000000003e85bdd0      1      1 RW---- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\UsrClass.dat
0x000000003e865a20      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Music\desktop.ini
0x000000003e8a2f20      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Saved Games\desktop.ini
0x000000003e8aa070      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Videos\desktop.ini
0x000000003e8abf20      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Searches\desktop.ini
0x000000003ea7b830      1      1 RW-rwd \Device\clfs\Device\HarddiskVolume2\Users\Alissa Simpson\NTUSER.DAT{016888bd-6c6f-11de-8d1d-001e0bcde3ec}.TM
0x000000003eaf0740      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_idx.db
0x000000003eb5e520      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_idx.db
0x000000003eb77a70     17      1 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_sr.db
0x000000003ebd4d10      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Desktop\desktop.ini
0x000000003ec24450      2      1 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\NTUSER.DAT{016888bd-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000001.regtrans-ms
0x000000003ec803a0      1      1 RW---- \Device\HarddiskVolume2\Users\Alissa Simpson\ntuser.dat.LOG1
0x000000003eca0f20      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Contacts\desktop.ini
0x000000003ed12f20      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\desktop.ini
0x000000003ee3d8b0      1      1 RW---- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\UsrClass.dat.LOG2
0x000000003ee7aa90     17      1 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_1024.db
0x000000003ee7e8c0      1      1 RW---- \Device\HarddiskVolume2\Users\Alissa Simpson\NTUSER.DAT
0x000000003ee82c10      2      1 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\NTUSER.DAT{016888bd-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000002.regtrans-ms
0x000000003eee3720      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_96.db
0x000000003ef3af20      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Favorites\Links\desktop.ini
0x000000003fa04640     15      0 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\46f433176bc0b3d2.automaticDestinations-ms
0x000000003fa04790      1      1 R--rw- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents
0x000000003fa17b70      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_32.db
0x000000003fa183b0      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_256.db
0x000000003fa19500      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_sr.db
0x000000003fa1b3d0      1      0 R--rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\WinRAR\version.dat
0x000000003fa1c500      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations\5afe4de1b92fc382.customDestinations-ms
0x000000003fa1cf20      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_1024.db
0x000000003fa2aa50     16      0 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\1b4dd67f29cb1962.automaticDestinations-ms
0x000000003fa2b8e0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Temporary Internet Files\Content.IE5\index.dat
0x000000003fa2baf0      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\desktop.ini
0x000000003fa2c5e0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Cookies\index.dat
0x000000003fa2e660      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\History\desktop.ini
0x000000003fa2ebd0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\History\History.IE5\index.dat
0x000000003fa2fda0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\History\History.IE5\MSHist012019121120191212\index.dat
0x000000003fa3ebc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
0x000000003fa89640     15      0 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\46f433176bc0b3d2.automaticDestinations-ms
0x000000003fa89790      1      1 R--rw- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents
0x000000003fa9cb70      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_32.db
0x000000003fa9d3b0      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_256.db
0x000000003fa9e500      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_sr.db
0x000000003faa03d0      1      0 R--rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\WinRAR\version.dat
0x000000003faa1500      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations\5afe4de1b92fc382.customDestinations-ms
0x000000003faa1f20      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_1024.db
0x000000003faafa50     16      0 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\1b4dd67f29cb1962.automaticDestinations-ms
0x000000003fab08e0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Temporary Internet Files\Content.IE5\index.dat
0x000000003fab0af0      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\desktop.ini
0x000000003fab15e0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Cookies\index.dat
0x000000003fab3660      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\History\desktop.ini
0x000000003fab3bd0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\History\History.IE5\index.dat
0x000000003fab4da0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\History\History.IE5\MSHist012019121120191212\index.dat
0x000000003fac3bc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
0x000000003fb0e640     15      0 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\46f433176bc0b3d2.automaticDestinations-ms
0x000000003fb0e790      1      1 R--rw- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents
0x000000003fb21b70      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_32.db
0x000000003fb223b0      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_256.db
0x000000003fb23500      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_sr.db
0x000000003fb253d0      1      0 R--rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\WinRAR\version.dat
0x000000003fb26500      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations\5afe4de1b92fc382.customDestinations-ms
0x000000003fb26f20      2      2 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_1024.db
0x000000003fb34a50     16      0 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\1b4dd67f29cb1962.automaticDestinations-ms
0x000000003fb358e0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Temporary Internet Files\Content.IE5\index.dat
0x000000003fb35af0      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Recent\desktop.ini
0x000000003fb365e0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Cookies\index.dat
0x000000003fb38660      1      0 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\History\desktop.ini
0x000000003fb38bd0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\History\History.IE5\index.dat
0x000000003fb39da0     17      1 RW-rw- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\History\History.IE5\MSHist012019121120191212\index.dat
0x000000003fb48bc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
0x000000003fc6e2c0      1      1 RW---- \Device\HarddiskVolume2\Users\Alissa Simpson\ntuser.dat.LOG2
0x000000003fc70650      2      1 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\UsrClass.dat{6f347af8-1bf1-11ea-bd40-080027c6589a}.TMContainer00000000000000000002.regtrans-ms
0x000000003fc709c0      2      1 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\UsrClass.dat{6f347af8-1bf1-11ea-bd40-080027c6589a}.TMContainer00000000000000000001.regtrans-ms
0x000000003fc7edd0      1      1 RW-rwd \Device\clfs\Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\UsrClass.dat{6f347af8-1bf1-11ea-bd40-080027c6589a}.TM
0x000000003fca7970      2      1 RW-r-- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\UsrClass.dat{6f347af8-1bf1-11ea-bd40-080027c6589a}.TM.blf
0x000000003fcc3990      1      1 RW-rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\Explorer\thumbcache_32.db
0x000000003fcf3840      2      1 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Credentials
0x000000003fcf6860      2      1 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Credentials
0x000000003fcf6d80      2      1 RW-rw- \Device\clfs\Device\HarddiskVolume2\Users\Alissa Simpson\NTUSER.DAT{016888bd-6c6f-11de-8d1d-001e0bcde3ec}.TM
0x000000003fcf7e50      1      1 RW---- \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\UsrClass.dat.LOG1
0x000000003fcf9f20      2      1 RW-rw- \Device\clfs\Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Local\Microsoft\Windows\UsrClass.dat{6f347af8-1bf1-11ea-bd40-080027c6589a}.TM
0x000000003fd021a0      2      1 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\AppData\Roaming\Microsoft\Windows\Printer Shortcuts
0x000000003fd04d50      2      1 R--rwd \Device\HarddiskVolume2\Users\Alissa Simpson\Links
```

Here after analyzing the output I saw the `important.rar` file and now using the memory address, I used dumpfiles to dump it to the `dumps` folder.
```cmd
0x000000003fac3bc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
```

```cmd
┌──(kali㉿kali)-[/mnt/…/VM_Files/curated/Redraw/volatility]
└─$ python2 vol.py -f ../MemoryDump_Lab1.raw --profile=Win7SP1x64 dumpfiles -Q 0x000000003fac3bc0 --name 3rd.rar -D dumps
Volatility Foundation Volatility Framework 2.6.1
DataSectionObject 0x3fac3bc0   None   \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
```
now I need to change the .dmp to .rar so it converts into a rar inside the rar I find a image but it is password protected
