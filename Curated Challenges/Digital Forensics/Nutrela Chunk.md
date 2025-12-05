# Nutrela Chunk
>One of my favorite foods is soya chunks. But as I was enjoying some Nutrela today, I noticed a few chunks weren’t quite right. Seems like something’s off with their structure. Could you help me fix these broken chunks so I can enjoy my meal again?

## Solution :
The first thing i tried was opening the file which did not open, so the first thing that crossed my mind was to check the hex code to see what is wrong, so I used hexedit to check the hex code. So a png's file structure has 4 parts :

1st : File Signature with hex : `89 50 4E 47 0D 0A 1A 0A`
2nd : IHDR with hex : `49 48 44 52`
3rd : IDAT with hex : `49 44 41 54`
4th : IEND with hex : `49 45 4E 44`

but what I found was that instead of `.PNG` it was `.png` and likewise instead of `IHDR` it was `ihdr` and so on. So after fixing the gile structure I finally got the correct image

<img width="1867" height="37" alt="image" src="https://github.com/user-attachments/assets/3e1789df-16ff-4e53-8cf9-de9ab95cf856" />

<img width="688" height="11" alt="image" src="https://github.com/user-attachments/assets/9aa59dc4-3c06-43cd-a1fe-7043499fcbfd" />


<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/3e08757d-6bfa-4b9b-a0d8-53c4b38defdb" />

## Flag :
```
nite{n0w_y0u_kn0w_ab0ut_PNG_chunk5}
```
