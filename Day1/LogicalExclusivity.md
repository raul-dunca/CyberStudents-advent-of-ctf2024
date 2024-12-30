<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day1_description.png">

For this challenge I used CyberChef, after trying different things and given the name of the challenge I tried the following recipe on the hex strings: 

<img src="https://github.com/raul-dunca/CyberStudents-advent-of-ctf2024/blob/main/.assets/day_1_info1.png">

In the output I observed something that looked like a flag: `kal{pCu8t3_8391Vv1V95_858L48}` when `key = 0f`. But the flag did not start with `csd{` as it is supposed to, so I tried `ROT13 Brute Force` from CyberChef. I also checked the `Rotate numbers` setting but that didn’t yield the correct flag, so use it without that setting:

<img src="https://github.com/raul-dunca/CyberStudents-advent-of-ctf2024/blob/main/.assets/day_1_info2.png">

`csd{hUm8l3_8391Nn1N95_858D48}`