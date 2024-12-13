<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day12_description.png">

A password-protected zip file is given. After different attempts of trying to find the password, I realized that the content of the zip looks like a github repository. I found  [this](https://github.com/bitmakerlabs/North-Pole-Writing-Machine), which has the exact same structure as the content of the zip (without the .env, so I assumed the flag is there). I also discovered bkcrack, a tool that can find the key of the zip, knowing some of the plaintext. I ran:
```bash
bkcrack.exe -C North-Pole-Writing-Machine.zip -c North-Pole-Writing-Machine/README.md -p README.md
```
Here, the `-p` flag specifies the plaintext readme file obtained from the github repository. The command yelled `Keys: 9310e3e7 c19b7b37 2e82342d`. Now, I just used bkcrack again to get the .env file:

```bash
bkcrack -C North-Pole-Writing-Machine.zip -c North-Pole-Writing-Machine/.env -k 9310e3e7 c19b7b37 2e82342d -d .env
```

`c​sd{Uns3cu4e_Encrypti0n}`
