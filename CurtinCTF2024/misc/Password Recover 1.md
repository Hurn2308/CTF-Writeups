Misc - Password Recovery 1 300 Points

This challenge had a description that detailed the conditions that are set when Alex was creating his password you are tasked to figure out what is his password given through the shadow file. 
This is the given content of the shadow file.
![image](https://github.com/user-attachments/assets/b6ed0ef3-e75e-4e05-af4e-11dd0a1b985c)

The first step I took was to extract alex's password and save it into a txt file. Then based on the hint given by the challenge creator I crafted a script to create a custom wordlist to crack the password using John The Ripper. Below is the script:
![image](https://github.com/user-attachments/assets/708a73a4-ac11-4e1a-800f-e23a36d16c86)

Using the wordlist and John The Ripper I cracked the password which was Autumn2024! and the flag format is CURTIN_CTF{Alex's Password}.
