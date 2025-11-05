# Challenge 1 : RSA Oracle
>Can you abuse the oracle?
An attacker was able to intercept communications between a bank and a fintech company. They managed to get the message (ciphertext) and the password that was used to encrypt the message.
Additional details will be available after launching your challenge instance.

## Solution : 

I am able to encode and decode arbitrary RSA messages EXCEPT the one we want (password.enc, which I'll call m). So I encrypted the number 2, then decrypt the product of the encryption of 2 with the encryption of m. This decryption gave me 2m. So I divided by two to get the password, and then use openssl to get the message. I used this script to get the flag and do all the stuff mentioned above :
```python
from pwn import *

connection = remote('titan.picoctf.net', 55423)
response = connection.recvuntil('decrypt.')
print(response.decode())
payload = b'E' + b'\n'

connection.send(payload)

response = connection.recvuntil('keysize):')
print(response.decode())
payload = b'\x02' + b'\n'
connection.send(payload)
response = connection.recvuntil('ciphertext (m ^ e mod n)')
response = connection.recvline()
num=int(response.decode())*2336150584734702647514724021470643922433811330098144930425575029773908475892259185520495303353109615046654428965662643241365308392679139063000973730368839
response = connection.recvuntil('decrypt.')
print(response.decode())
payload = b'D' + b'\n'
connection.send(payload)
response = connection.recvuntil('decrypt:')
print(response.decode())
connection.send(str(num)+'\n')

response = connection.recvuntil('hex (c ^ d mod n):')
print(response.decode())
response = connection.recvline()
print(response.decode())
num=int(response,16)//2
print(hex(num))

Now we convert this to ASCII
hex_string=hex(num)[2:] # get rid of 0x
byte_array=bytes.fromhex(hex_string)
print(byte_array.decode('ascii'))
connection.close()
```
After that i got the decryption key which was `60f50` and used it with openssl and decode the secret.enc using that key which has aes-256 key encryption.
```
FudgeSupreme-picoctf@webshell:~$ openssl enc -aes-256-cbc -d -in secret.enc -k 60f50
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
picoCTF{su((3ss_(r@ck1ng_r3@_60f50766}FudgeSupreme-picoctf@webshell:~$
```
Flag : 
```
picoCTF{su((3ss_(r@ck1ng_r3@_60f50766}
```

# Challenge 2: Custom Encryption
>Can you get sense of this code file and write the function that will decode the given encrypted file content.
Find the encrypted file here flag_info and code file might be good to analyze and get the flag

## Solution :

In this challenge, we were handed 2 files, 1st was the cipher data and the 2nd was the encryption code which we needed to use to make a decrypt the cipher data and get our flag, to do this I carefully studied the code, it used the `Diffie-Hellman-like` key , also I noticed that the plaintext was reversed and XOR'd against a text key `trudeau`, so yeah, using these information and some other i was able to create a program in python by recreating the shared key and reversing the XOR operation. I hardcoded all my cipher data in the code itself and found the flag.

```python
from custom_encryption import is_prime, generator
 
 
def leak_shared_key(a, b):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
 
    return shared_key
 
 
def decrypt(ciphertext, key):
    semi_ciphertext = []
    for num in ciphertext:
        semi_ciphertext.append(chr(round(num / (key * 311))))
    return "".join(semi_ciphertext)
 
 
def dynamic_xor_decrypt(semi_ciphertext, text_key):
    plaintext = ""
    key_length = len(text_key)
    for i, char in enumerate(semi_ciphertext):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        plaintext += decrypted_char
    return plaintext[::-1]
 
 
if __name__ == "__main__":
    # 0. Take relevant values from `enc_flag` and `custom_encryption.py`
    a = 97
    b = 22
    ciphertext_arr = [151146, 1158786, 1276344, 1360314, 1427490, 1377108, 1074816, 1074816, 386262, 705348, 0, 1393902, 352674, 83970, 1141992, 0, 369468, 1444284, 16794, 1041228, 403056, 453438, 100764, 100764, 285498, 100764, 436644, 856494, 537408, 822906, 436644, 117558, 201528, 285498]

    text_key = "trudeau"
 
    # 1. Get the shared key used in `test`
    shared_key = leak_shared_key(a, b)
 
    # 2. Invert the `encrypt` operation
    semi_ciphertext = decrypt(ciphertext_arr, shared_key)
 
    # 3. Revert the XOR operation
    plaintext = dynamic_xor_decrypt(semi_ciphertext, text_key)
 
    # 4. Output the flag
    print(plaintext)
```
this code directly did all the things i mentioned above and gave us the flag.

Flag : 
```
picoCTF{custom_d2cr0pt6d_e4530597}
```

# Challenge 3 : miniRSA
>Let's decrypt this: ciphertext? Something seems a bit small.

## Solution : 

In this challenge, we had some Ciphertext which used RSA. According to the challenge, we were given a small `e` was used, which we can exploit to get the flag. We know `c = m^e % n` where m is the plaintext. e is small, so we could conceivably compute the cube root.
So I used this code, to do just that : 
```python
from Crypto.Util.number import long_to_bytes
from gmpy2 import iroot

N = 29331922499794985782735976045591164936683059380558950386560160105740343201513369939006307531165922708949619162698623675349030430859547825708994708321803705309459438099340427770580064400911431856656901982789948285309956111848686906152664473350940486507451771223435835260168971210087470894448460745593956840586530527915802541450092946574694809584880896601317519794442862977471129319781313161842056501715040555964011899589002863730868679527184420789010551475067862907739054966183120621407246398518098981106431219207697870293412176440482900183550467375190239898455201170831410460483829448603477361305838743852756938687673
e = 3

c = 2205316413931134031074603746928247799030155221252519872650073010782049179856976080512716237308882294226369300412719995904064931819531456392957957122459640736424089744772221933500860936331459280832211445548332429338572369823704784625368933


for i in range(10_000):
    c_try = c + i * N
    m = int(iroot(c_try, 3)[0])
    flag = long_to_bytes(m)

    if b'pico' in flag:
        print(flag)
```
which directly gave us the flag : 

<img width="426" height="69" alt="image" src="https://github.com/user-attachments/assets/1e590501-5130-4141-9c81-140b808edc09" />

## Flag : 
```
picoCTF{n33d_a_lArg3r_e_ccaa7776}
```
