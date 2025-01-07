# Description
Welcome to the Phish Market Order Management System! This system will allow you to view important information about the Phish Market's products and customers.

`nc phishmarket.ctf.intigriti.io 1336`

# Flag 
INTIGRITI{w3b_ch4ll3n63_1n_d156u153}

# Analysis
Initially running the program it will expect a password, so the first step of this challenge is to find out what is the password.

![image](https://github.com/user-attachments/assets/6403fd0d-6a25-49fd-8138-2cc0996e0e9a)

To do this we can inspect the source code as it is provided within the challenge. In the source code, we can see there is a check for the password in sub_3593

![image](https://github.com/user-attachments/assets/98f2bdb7-de7b-4b89-9668-9554d7dbd4fc)

This function is more of the password comparison function where it just checks if the inputted password is the same length as the expected password and etc. The logic in which we can use to decipher the password is in function sub_34C9.
```
_QWORD *__fastcall sub_34C9(_QWORD *a1)
{
  char *v1; // r12
  _BYTE *v2; // r14
  unsigned __int64 v3; // rax
  char v4; // r13
  __int64 v5; // rbp
  unsigned __int64 v6; // r15

  *a1 = a1 + 2;
  a1[1] = 0LL;
  *((_BYTE *)a1 + 16) = 0;
  v1 = (char *)&unk_6C20;
  v2 = &unk_6C10;
  do
  {
    v4 = *v2 ^ *v1;
    v5 = a1[1];
    v6 = v5 + 1;
    if ( a1 + 2 == (_QWORD *)*a1 )
      v3 = 15LL;
    else
      v3 = a1[2];
    if ( v3 < v6 )
      std::string::_M_mutate(a1, a1[1], 0LL, 0LL, 1LL);
    *(_BYTE *)(*a1 + v5) = v4;
    a1[1] = v6;
    *(_BYTE *)(*a1 + v5 + 1) = 0;
    ++v1;
    ++v2;
  }
  while ( v1 != (char *)&unk_6C30 );
  return a1;
}
```

This function XOR's bytes from two buffers `unk_6C10` and `unk_6C20` within these two buffers is the data that can be used to decipher the password. To dump the data of these buffers you can use IDA. Search for the buffer.
![image](https://github.com/user-attachments/assets/526350a3-c1b2-4193-8c40-6bd20164c834)

As we can see unk_6C10 is readable ascii but 6C20 is not but this doesn't matter as we can produce a script that uses the hex values like so:

```
# Data from .rodata
unk_6C10 = b"INTIGRITI1337up#"
unk_6C20 = bytes([0x07, 0x7D, 0x22, 0x7A, 0x15, 0x15, 0x79, 0x3A, 0x27, 0x71, 0x05, 0x46, 0x04, 0x51, 0x54, 0x02])

# Recover the password
password = ''.join(chr(unk_6C10[i] ^ unk_6C20[i]) for i in range(len(unk_6C10)))

print(f"The admin password is: {password}")

```

Running this script gives you the password `N3v3RG0nn@6u3$$!`. Now, we can login into the server.
![image](https://github.com/user-attachments/assets/f64fbb91-0c2e-409a-8095-e1f579a2cafb)

In the server, we are given a 4 options 2 of which are irrelevant which you can tell by just reading the source code.
![image](https://github.com/user-attachments/assets/53b86299-c12c-400c-9a3e-82a91243b319)

Looking at the source code and the init-db.sql file we can tell that the rest of this challenge is an SQL injection to get the content of the admin table which houses the flag. 

![image](https://github.com/user-attachments/assets/dd6ef5a5-ca5e-472c-a9cf-897737ccac8a)

At first I tested on the Orders function but no matter what injection I gave it returned this error which I figured meant that it was validating and checking for any ' symbol which meant this was not the injection point so I tried the Products function instead:

![image](https://github.com/user-attachments/assets/e2214c07-2e89-4dd2-bc4c-6ef3ba4cede3)

To test my theory I injected a simple injection to see if this is the injection point. The payload `admin' AND (SELECT 1 FROM admin LIMIT 1) = 1; --` gave an error that said "--" is not allowed. So I tested an injection that used # to comment out instead. 

![image](https://github.com/user-attachments/assets/d0bfbe22-86e4-4f3a-aef6-19771ed24272)

There was no query results but the injection was valid and went through the server so from here I knew the structure of the payload which needed #. Next I tried to use UNION to get the data from the admin table with this payload `admin' UNION SELECT 1, username, flag FROM admin; # `

![image](https://github.com/user-attachments/assets/6db65deb-6eee-4287-978b-efac9684762c)

I only realized this a bit later but seeing that the flag column value can't be printed I realized later it was due to the fact the product table price column is using the decimal format so the string of the flag can't be printed on the price column it needs to be on the name column. Thus we can now create our payload for the solution.

![image](https://github.com/user-attachments/assets/5c5996bf-14c1-4026-881c-830c7cf84f1e)


# Solution
So the solution is to inject a payload that will replace the name value of the product table with the value of the flag. To do this I just asked GPT to generate the payload for me that will follow the criteria of the SQL database which was `' OR 1=1 UNION SELECT flag, 0 FROM admin; #`

![image](https://github.com/user-attachments/assets/1edf0773-6ec6-4edf-9424-5c7a874061ea)

#L337upCTF2024 
