# Haunted Data Trail: The Last Tab 2
![image](https://github.com/user-attachments/assets/6c8dde13-bede-4b4f-806c-ba6978b11a61)

# Solution
The website is a credit card database where when you first reach it you will be presented with a login screen and there is an option to register as well.
![image](https://github.com/user-attachments/assets/4e06bdca-2e80-49fc-b590-624ffaffa739)

After registering and logging in I inspected the requests to see if there was any clue in which I found the first part of the flag:
![image](https://github.com/user-attachments/assets/8a4f2452-f92c-48e0-8d41-d249e4e1db84)

Each part of the flag is base64 encrypted so you need to decode all parts together to form the flag. The next part of the flag I found in the log path which was one of the only other features after logging in:
![image](https://github.com/user-attachments/assets/c47b823b-82d1-4bea-8c26-cf72c7ae0869)

The last 2 parts of the flag I obtained through XML file upload injection as this was the only other feature on the website where users could upload the xml structure of their credit card data to upload it into the database. Using my first payload and checking the etc/passwd file I found part 3 of the flag:
![image](https://github.com/user-attachments/assets/04690dfa-52c5-4a2a-9df2-25bece3f2763)

And searching through the app/app.py file I found the last part of the flag:
![image](https://github.com/user-attachments/assets/be81c34c-705a-4b7a-8599-eeb9109e6260)

Then you combine all parts to get the flag:
![image](https://github.com/user-attachments/assets/0fa7ba35-0a0e-4388-a548-6961dc926f90)
