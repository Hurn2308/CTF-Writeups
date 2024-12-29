# Description
We found a leak of a blackmarket website's login credentials. Can you find the password of the user osman and successfully decrypt it?

Author: Alhfs


# Flag
WGMY{b6d180d9c302d8a8daad1f2174a0b212}


# Analysis
Looking at both the txt files the number of lines basically matched up so I just searched for the same line position in the password as osman which was 337 which had the obfuscated flag.


# Solution
To solve just throw it into a rot13 decipher and choose rot23:
![image](https://github.com/user-attachments/assets/e70050c3-3a5b-4ef5-bd20-9abe1d162f74)
