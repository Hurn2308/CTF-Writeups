# Web Dev N The Bugs
![image](https://github.com/user-attachments/assets/d476132b-4c92-4bab-9829-f15a805d9834)

# Solution
The website has multiple versions and if you idle on it, it will switch between the version at some point you will reach to version 1.12 which tells you on version 1.13 you can view files to debug. Using this we can assume that this version of 1.13 since it doesn't appear on its own is the version we need to get to. After inspecting each request I found one with a cookie that looked strange:
![image](https://github.com/user-attachments/assets/dce60d25-7230-4704-9720-b2ed8bd267a9)

The force_page cookie by assumption would be able to swap between the versions forcefully so to test I used burp and sure enough it did allow to change between versions by changing the numbers. Eventually, you will get to version 1.13 where you can see multiple debug logs.
![image](https://github.com/user-attachments/assets/fc0c4056-d8b1-4cf8-ae6f-c8ba99bc2304)

If you click on it you can inspect the file and if you pay attention to the url https://hackersnhops-noscrapingoritwillgodownagain.chals.io/viewfile/etc/passwd you can change what is after the viewfile to reach the file that you want. 
![image](https://github.com/user-attachments/assets/966805e1-5f22-4542-b426-bb30e67e3787)

So I viewed the files that I suspected would have the flag based on experience till I tried https://hackersnhops-noscrapingoritwillgodownagain.chals.io/viewfile/var/www/site.log which housed the flag:
![image](https://github.com/user-attachments/assets/6524a156-dd06-4234-84ff-38ec9515e28a)
