# NineTails
>Looks like I got a little too clever and hid the flag as a password in Firefox, tucked away like one of NineTails’ many tails. Recover the "logins" and the "key4" and let it guide you to the flag.
>
>Hint : I named my Ninetails "j4gjesg4", quite a peculiar name isn't it?

## Solution :

In this challenge we were given a rar file although at the start the description didn't make sense but when I extracted the rar file I found a file called `ninetails.ab1`, I had no clue what this filetype did so I researched online and found that they are logical images similar to a container. They are used to hold file-level acquisitions, basically it is a file which stores the crucial data of a drive or partition which can be helpful during digital forensics. So now that I knew what they were I needed to get FTK imager to extract the data from the .ab1 file. So I downloaded `FTK imageer` and added the `.ab1` file as Image Type Evidence and then exported all the data to my hard disk. Now that That was done the description started to make a bit sense, so according to the description the flag was stored as a password in a user profile called `j4gjesg4`. So I found out that passwords are stored in :

<img width="264" height="142" alt="image" src="https://github.com/user-attachments/assets/9d6ac9e2-0b2a-445f-9eb0-a9017ccabae3" />

These 2 files at `appData\Roaming\Mozilla\Firefox\Profiles\<ProfileName>`, So I go over to find it, then the next step for me was to download Firefox on my laptop and then replace my `key4.db` and `logins.json` with the ones I copied from the user, after doing that I directly went over to Firefox checked the passwords and found the flag divided into 2 parts :

<img width="946" height="519" alt="image" src="https://github.com/user-attachments/assets/f67a5209-0592-49bd-8384-9cad0998983e" />

<img width="916" height="508" alt="image" src="https://github.com/user-attachments/assets/34052371-983d-47b0-ac76-2f62805c7ddb" />

## Flag : 
```
GCTF{m0zarellap4ssw0rd}
```
