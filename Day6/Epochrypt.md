<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day6_description.png">

For this challenge I can see the encoded flag, done by this function: 

```python
def epochrypt(enc):
    bits = bytes([(b + 3) % 256 for b in enc])
    based = b64.b64encode(bits)
    epc = str(int(time.time())).encode()
    final = xor(based, epc)
    print(final.hex())
```
Additionally, there is the possibility to encrypt a given text and check if the flag is correct. The `epochrypt` function uses `time.time()` which  return the current time, so the program will return different encoding of the flag based on the time when the program is ran. First, I decided to reverse the logic of the epochrypt function:

```python
import time

from pwn import xor
import base64 as b64

print(str(int(time.time())))

enc_hex="6b59695b535a510a6a6c7f5e667151597c55700f57707a0660607b0a5b0a5c767764080a"
encrypted_flag=bytes.fromhex(enc_hex)
around_time=1735572898
epc=str(int(around_time)).encode()
final=xor(encrypted_flag,epc)
print(final)

based=b64.b64decode(final)
dec=bytes([(b-3)%256 for b in based])
print(dec)
```

This code just reverses the logic, and to get an estimated of `time.time()`, I printed it before pressing `View Encrypted Flag` on the remote server. Then just used my reverse function and increased the `around_time` variable by 1 (the delay was just 1 second so I had to increase it only once) and checked the flag with the remote server. 

`csd{d3F0_M4d3_8y_4N_3lf}`
