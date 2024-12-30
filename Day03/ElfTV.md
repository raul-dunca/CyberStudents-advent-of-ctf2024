<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day3_description.png">

A Rust program is given, analyzing it seems that a valid license key needs to be given as input for the program to return the flag. The conditions for the license key are:

- the key starts with `XMAS`
- the length of the key is 12
- the sum of the characters with the index in the range [4-8] must have the sum 610

For the 3rd condition I looked at the ascii table where `z=122` and `122*5=610`, thus so far, the license key looks like: `XMASzzzzz`. Finally, a fibonnaci function is called and only the last 3 digits of the returned number are used as the last 3 characters of the license key. Well, I just ran the given function in an online Rust compiler and printed the output: `12200160415121876738`. So the license key is `XMASzzzzz738`.

`csd{Ru57y_L1c3N53_k3Y_CH3Ck3r}`
