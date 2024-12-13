<img src="https://github.com/raul-dunca/assets/blob/main/.images/ccccc.png?raw=true">

A password protected file is given. After different attempts of trying to find the password I realised that the content of the zip looks like a github repository. I found [this](https://github.com/bitmakerlabs/North-Pole-Writing-Machine), which has the exact same strcture as the content of the zip (without the .env, so I assumed the flag is there). I also discovered the bkcrack which can find the key of the zip, knowing some of the plaintext. I runned `bkcrack.exe -C North-Pole-Writing-Machine.zip -c North-Pole-Writing-Machine/README.md -p README.md` here after the `-p` flag is the plaintext readme file obtained from the github repository. The command yeld `Keys: 9310e3e7 c19b7b37 2e82342d`. Now I just used bkcrac again to get the .env file `bkcrack -C North-Pole-Writing-Machine.zip -c North-Pole-Writing-Machine/.env -k 9310e3e7 c19b7b37 2e82342d -d .env`.

`c​sd{Uns3cu4e_Encrypti0n}`
