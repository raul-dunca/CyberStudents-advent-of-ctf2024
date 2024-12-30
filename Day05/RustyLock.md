<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day5_description.png">

I started by putting the binary and the pdb file in the same folder and opened the exe file in binary ninja. Found the `CrackMe::validate_password` function and relabeled symbols to get an understanding of what is happening. There are 5 checks done:

1) len ==12
2) first digit must be even
3) sum of digits should be 69
4) must contains a substring from variable `pat`, which is a substring of: `YellowsGreenBlueRedOrangePurplesrc\\main.rs`
5) last digit is odd

Here is the relabeled function in binary ninja:

<img src="https://github.com/raul-dunca/CyberStudents-advent-of-ctf2024/blob/main/.assets/day_5_info1.png">

<img src="https://github.com/raul-dunca/CyberStudents-advent-of-ctf2024/blob/main/.assets/day_5_info2.png">

For the 4th condition, it is possible to find exactly which substring from `pat` must be contained, but I didn’t check the function in detail because it seems like a lot of work. I assumed there is some kind of slicing being done. Knowing this information, I just burteforced the 4th condition. Ignoring that condition for now, I crafted this string: `8XXXX7999999`, which respects the other conditions. Now I just replaced `XXXX` with a different substring of `YellowsGreenBlueRedOrangePurplesrc\\main.rs` until `8eRed7999999` gave the flag.

`csd{V41id_L1ceNs3_K3y}`
