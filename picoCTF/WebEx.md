# Challenge 1 : Web Gauntlet

> Can you beat the filters?

## Solution

When I saw the challenge,I realized it was a classic SQL injection challenge, so as usual my first thought was to use `'OR 1+1=2` to solve it, but seeing as it didn't work i checked out filter.php, and realized that it is blacklisted, so my next thought was to use comments, so using `--` which will comment out anything else in the line. So in the username and password field i put down the following info
```
Username : admin'--
Password : anything
```
Which when done correctly would like this : 
<img width="1919" height="885" alt="image" src="https://github.com/user-attachments/assets/4cc804b7-94ac-4694-b974-510a00506956" />

So now for Round 2, `filter.php` had been updated and so now i couldn't use `--`, but there is another way to comment out things in sqlite i.e. `/**/`, so using the same methodology i put down the following info :  
```
Username : admin'/*
Password : anything
```
<img width="1916" height="886" alt="image" src="https://github.com/user-attachments/assets/89919ce5-e51c-4a2f-ab87-ac4622d1f0cb" />

Which worked so i proceeded to round 3, I noticed something in round 3 that the filter.php, did not multiline comments in it again, so i did the exact same thing again to proceed to the next round.

Now here in round four the filter has yet again been updated and this time the word `admin` has also been blacklisted, so i searched around ways to go around it, and found that u can concatenate words in sqllite by using `||`,so by using this info I again went on to put down the following info in the username and password box to test out my theory

```
Username : ad'||'min'/*
Password : anything
```

<img width="1911" height="876" alt="image" src="https://github.com/user-attachments/assets/2c640fde-beec-47e8-b42c-b79b113e3318" />

Which to my delight worked exactly as i had expected, now as I proceeded to the fifth round admin was still blacklisted but `||` was still not, so i used the exact same thing again to complete round 5.

<img width="582" height="466" alt="image" src="https://github.com/user-attachments/assets/6f4a3c20-4912-4dcf-9404-3672e25cdc2b" />

Now the challenge has been completed and i found the flag in `filter.php`

## Flag : 

```
picoCTF{y0u_m4d3_1t_79a0ddc6}
```

## Resources 

```
For Sql Injections : https://swisskyrepo.github.io/PayloadsAllTheThings/SQL%20Injection/SQLite%20Injection/
To find out how to concatenate a sentence in sqlite : https://www.sqlitetutorial.net/sqlite-string-functions/sqlite-concat/
```
