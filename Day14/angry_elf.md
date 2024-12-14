<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day14_description.png">

Opening the file in binary ninja, it can be seen that the length of the passcode must be 11 and that the `validate_passcode` function must return true. Looking at the validation function, each byte of the passcode is XOR-ed with `0x7f` and then the result must be equal to a variable called `obfuscated_key` which is equal to `\x0f\r\x16\x11\x18\x13\x1a\x0cOF\\`. Thus, I wrote a python script to XOR each byte of the variable with `0x7f` and get the passcode:

```python

obfuscated_key = b"\x0f\r\x16\x11\x18\x13\x1a\x0cOF\\"
result = bytes([byte ^ 0x7f for byte in obfuscated_key])
print(result) #b'pringles09#'
```
Finally, I just sent the passcode to the server:
```bash
echo "pringles09#" | nc ctf.csd.lol 1147
```
`csd{4N9ry_3lf5_5h0uLdNT_83_M3553D_w1tH}`
