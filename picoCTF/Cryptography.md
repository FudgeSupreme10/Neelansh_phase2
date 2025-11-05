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
