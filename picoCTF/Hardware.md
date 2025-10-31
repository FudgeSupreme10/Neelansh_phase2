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
