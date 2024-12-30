<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day4_description.png">

For this challenge, I used `FTK Imager` to analyze the disk image. Browsing the files and knowing that some got deleted, I found in `osbozed/.local/share/Trash/files` a file `flag_game.zip` inside there is a `flaggame.txt` which I assume has the flag, but the zip is password protected. I tried to browse the system and hoped to find a file with the password. Then I decided to bruteforce the password using `fcrackzip` and password length of 1-6, but that did not work. So I decided to do a dictionary attack using the `rockyou` file. The command I used is:
```bash
fcrackzip -v -D -u -p rockyou.txt flag_game.zip
```
The password: `ilovexmas2` was found and I could extract the txt file and get the flag.

`csd{5An7a5_N3W_Gam3_5uCk2}`
