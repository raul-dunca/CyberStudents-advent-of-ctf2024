<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day7_description1.png">
<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day7_description2.png">

The index page has a script included, located at `/my-secret-vault-of-scripts-n-files/ai-script.js`. Looking at it seems like is not a complete program:

<img src="https://github.com/raul-dunca/CyberStudents-advent-of-ctf2024/blob/main/.assets/day_7_info.png">

It requires that the user agent contains the word Mac, and that an input element on the page should have a specific string in it. If these conditions are met, a GET request will be made but the link to whom the request will be made is missing, so the solution must be in another direction. On the site, an interesting note was that: `You will never see me use a Windows or Linux system (ew). Especially not for creating this site :^)`. So, I thought maybe it has something to do with mac, like a mac specific file for web servers. Tried: https://apple-fanatic.csd.lol/my-secret-vault-of-scripts-n-files/.DS_Store and indeed a file was downloaded. Then just installed a ds_store parser which revealed the file `the-birth-date-of-my-beloved-apple-tree.txt` so going to: `https://apple-fanatic.csd.lol/my-secret-vault-of-scripts-n-files/my-beloved-apple-tree.txt`  reveals the flag.

`csd{5H3_w45_80RN_0N_7H3_d4y_0f_Chr157M4Z}`
