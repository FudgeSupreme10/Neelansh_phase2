# Challenge 1 : I Like Logic

> i like logic and i like files, apparently, they have something in common, what should my next step be.

## Solution : 

Tbh I have never done any hardware challenges before so this was a pretty new one for me, the first thing i did was find out what a sal file was and on googling i found out i had to use `Saleae Logic 2` to anaylze it. So on downloading it, i loaded the file on the software, where it showed that there was some data on channel 3, next I went to analyzers tab because that is what looked most pbvious to me, I played around a bit and then googled on how it worked, so apparently `I2C`, `Async Serial` and `SPI` were the 3 most common ones from the list of options I had been provided with so one by one i tried using them, but only `Async Serial` seemed to be working, so i played around with, found a lot of hex data, and saw and option to convert it into `ASCII`, which gave me lots of text and the flag.

```

on't think he makes any claims of
that kind.  But I do believe he has got something new."

"Then for Heaven's sake, man, write it up!"

"I'm longing to, but all I know he gave me in confidence and on
condition that I didn't."  I condensed into a few sentences the
Professor's narrative.  "That's how it stands."

McArdle looked deeply incredulous.

"Well, Mr. Malone," he said at last, "about this scientific meeting
to-night; there can be no privacy about that, anyhow.  I don't suppose
any paper will want to report it, for Waldron has been reported already
a dozen times, and no one is aware that Challenger will speak.  We may
get a scoop, if we are lucky.  You'll be there in any case, so you'll
just give us a pretty full report.  I'll keep space up to midnight."

My day was a busy one, and I had an early dinner at the Savage Club
with Tarp Henry, to whom I gave some account of my adventures.  He
listened with a sceptical smile on his gaunt face, and roared with
laughter on hearing that the Professor had convinced me.

"My dear chap, things don't happen like that in real life.  People
don't stumble upon enormous discoveries and then lose their evidence.
Leave that to the novelists.  The fellow is as full of tricks as the
monkey-house at the Zoo.  It's all bosh."

"But the American poet?"

"He never existed."

"I saw his sketch-book."

"Challenger's sketch-book."

"You think he drew that animal?"

"Of course he did.  Who else?"

"Well, then, the photographs?"

"There was nothing in the photographs.  By your own admission you only
saw a bird."

FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}

"A pterodactyl."

"That's what HE says.  He put the pterodactyl into your head."

"Well, then, the bones?"

"First one out of an Irish stew.  Second one vamped up for the
occasion.  If you are clever and know your business you can fake a bone
as easily as you can a photograph."

I began to feel uneasy.  Perhaps, after all, I had been premature in my
acquiescence.  Then I had a sudden happy thought.

"Will you come to the meeting?" I asked.

Tarp Henry looked thoughtful.
.....
```

# Flag : 
```
FCSC{b1dee4eeadf6c4e60aeb142b0b486344}
```

# Challenge 2 : IQ Test
>let your input x = 30478191278.
wrap your answer with nite{ } for the flag.
As an example, entering x = 34359738368 gives (y0, ..., y11), so the flag would be nite{010000000011}.

## Solution : 

The Challenge took me a while to solve as it had to be done completely manually cuz i couldn't figure out any other way to do it so what I did was first I converted the decimal number to binary and then added an extra 0 to as the no was 35 bits and we needed 36, then i manually decoded it using `y0....y11` gates. to finally get the flag.

<img width="1225" height="2176" alt="iqtest (1)" src="https://github.com/user-attachments/assets/c892c2fe-11dc-425e-bb22-69646f077032" />

## Flag : 
```
nite{100010011000}
```

# Challenge 3 : Bare Metal Alchemist
>my friend recommended me this anime but i think i've heard a wrong name.

## Solution : 

So the file given to us was a ELF file, so I used `file firmware.elf` to find the exact filetype and then searched it online and found out that I can use `Ghidra` to decompile and checkout the main function and see what the file was doing, so then using that I found the main function of the file :
```c

void main(void)

{
  char cVar1;
  byte bVar2;
  undefined1 *unaff_SP;
  undefined1 *puVar3;
  byte bVar4;
  byte bVar5;
  char cVar6;
  char *pcVar7;
  byte bVar8;
  byte extraout_R17;
  char cVar9;
  undefined1 uVar11;
  undefined1 *puVar10;
  short sVar12;
  
  uVar11 = (undefined1)((ushort)&stack0x0000 >> 8);
  bVar4 = 0;
  *unaff_SP = 0;
  unaff_SP[-1] = uVar11;
  *(undefined2 *)(unaff_SP + -3) = 0xbe;
  puVar3 = unaff_SP + -4;
  bVar5 = DAT_mem_0044;
  DAT_mem_0044 = bVar5 | 2;
  bVar5 = DAT_mem_0044;
  DAT_mem_0044 = bVar5 | 1;
  bVar5 = DAT_mem_0045;
  DAT_mem_0045 = bVar5 | 2;
  bVar5 = DAT_mem_0045;
  DAT_mem_0045 = bVar5 | 1;
  bVar5 = DAT_mem_006e;
  DAT_mem_006e = bVar5 | 1;
  DAT_mem_0081 = bVar4;
  bVar5 = DAT_mem_0081;
  DAT_mem_0081 = bVar5 | 2;
  bVar5 = DAT_mem_0081;
  DAT_mem_0081 = bVar5 | 1;
  bVar5 = DAT_mem_0080;
  DAT_mem_0080 = bVar5 | 1;
  bVar5 = DAT_mem_00b1;
  DAT_mem_00b1 = bVar5 | 4;
  bVar5 = DAT_mem_00b0;
  DAT_mem_00b0 = bVar5 | 1;
  bVar5 = DAT_mem_007a;
  DAT_mem_007a = bVar5 | 4;
  bVar5 = DAT_mem_007a;
  DAT_mem_007a = bVar5 | 2;
  bVar5 = DAT_mem_007a;
  DAT_mem_007a = bVar5 | 1;
  bVar5 = DAT_mem_007a;
  DAT_mem_007a = bVar5 | 0x80;
  DAT_mem_00c1 = bVar4;
  bVar5 = DAT_mem_002a;
  DAT_mem_002a = bVar5 | 0xf8;
  bVar5 = DAT_mem_0024;
  DAT_mem_0024 = bVar5 | 3;
  bVar5 = DAT_mem_002a;
  DAT_mem_002a = bVar5 & 0xfb;
  bVar5 = DAT_mem_002b;
  DAT_mem_002b = bVar5 & 7;
  bVar5 = DAT_mem_0025;
  DAT_mem_0025 = bVar5 & 0xfc;
  bVar5 = 0;
  cVar6 = '\0';
  puVar10 = puVar3;
  do {
    bVar8 = DAT_mem_0029;
    if (((bVar8 ^ bVar8 * '\x02') & 4) == 0) {
      *(undefined2 *)(puVar3 + -1) = 0x141;
      puVar3 = puVar3 + -2;
      z1();
    }
    else {
      pcVar7 = (char *)0x68;
      while( true ) {
        if ((*pcVar7 == '\0') || (*pcVar7 == -0x5b)) goto LAB_code_0131;
        bVar8 = DAT_mem_0029;
        if (((bVar8 ^ bVar8 * '\x02') & 4) == 0) break;
        *(undefined **)(puVar3 + -1) = &DAT_mem_0151;
        puVar3 = puVar3 + -2;
        bVar8 = z1();
        if ((extraout_R17 & 1) != 0) {
          bVar2 = DAT_mem_002b;
          DAT_mem_002b = bVar2 | 8;
        }
        if ((extraout_R17 & 2) != 0) {
          bVar2 = DAT_mem_002b;
          DAT_mem_002b = bVar2 | 0x10;
        }
        if ((extraout_R17 & 4) != 0) {
          bVar2 = DAT_mem_002b;
          DAT_mem_002b = bVar2 | 0x20;
        }
        if ((extraout_R17 & 8) != 0) {
          bVar2 = DAT_mem_002b;
          DAT_mem_002b = bVar2 | 0x40;
        }
        if ((extraout_R17 & 0x10) != 0) {
          bVar2 = DAT_mem_002b;
          DAT_mem_002b = bVar2 | 0x80;
        }
        if ((extraout_R17 & 0x20) != 0) {
          bVar2 = DAT_mem_0025;
          DAT_mem_0025 = bVar2 | 1;
        }
        if ((extraout_R17 & 0x40) != 0) {
          bVar2 = DAT_mem_0025;
          DAT_mem_0025 = bVar2 | 2;
        }
        cVar9 = (bVar8 & 0x1f) + 0x2d;
        while (cVar1 = cVar9 + -1, cVar9 != '\0') {
          sVar12 = 3999;
          do {
            sVar12 = sVar12 + -1;
            cVar9 = cVar1;
          } while (sVar12 != 0);
        }
        pcVar7 = (char *)CONCAT11((char)((ushort)pcVar7 >> 8) - (((char)pcVar7 != -1) + -1),
                                  (char)pcVar7 + '\x01');
      }
      *(undefined2 *)(puVar3 + -1) = 0x131;
      puVar3 = puVar3 + -2;
      z1();
LAB_code_0131:
      puVar10[2] = bVar4;
      puVar10[1] = bVar4;
      while (puVar10[2] == '\0' || (byte)(puVar10[2] - 1) < ((byte)puVar10[1] < 0x2c)) {
        sVar12 = *(short *)(puVar10 + 1) + 1;
        puVar10[2] = (char)((ushort)sVar12 >> 8);
        puVar10[1] = (char)sVar12;
      }
    }
    if (bVar5 != bVar4 || cVar6 != (byte)(bVar4 + (bVar5 < bVar4))) {
      *(undefined2 *)(puVar3 + -1) = 0x146;
      puVar3 = puVar3 + -2;
      __vectors();
    }
  } while( true );
}
```
In this I noticed that XOR was being used and I knew the flag would probably be hardcoded in XOR in the executable, so to find it out I made a script which tried brute-forced it to get the flag, which I used on the firmware file and got the flag.

## Flag : 
```
TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}
```
