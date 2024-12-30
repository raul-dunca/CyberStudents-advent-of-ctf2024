<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day15_description.png">

The app allows you to authenticate with any username and password, and you get a JWT token. There is also a call to `script.js` which basically checks if the subject of the token is `KUn4L` and also gives the 256 bit secret used to sign the JWT token, both were encoded in base64. Putting the given token in [jwt.io](https://jwt.io/) I could see:

<img src="https://github.com/raul-dunca/CyberStudents-advent-of-ctf2024/blob/main/.assets/day_15_info.png">

Now I just modified the subject and signed the token, then just updated my cookie and refresh the page. The flag was in one of the pictures.

`csd{Wh47_D1D_KUN4I_do_7h1S_71M3}`
