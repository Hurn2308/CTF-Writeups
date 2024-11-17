# Description
Something weird going on at this pizza store!!

[https://pizzaparadise.ctf.intigriti.io](https://pizzaparadise.ctf.intigriti.io/)

No source code provided.
# Flag
INTIGRITI{70p_53cr37_m15510n_c0mpl373}


# Analysis
As always we visit the site first and see what are the available functions/features to us that could be injection points
![image](https://github.com/user-attachments/assets/f47fc33c-f5fa-4458-b429-edbb31668111)


Looking at all the pages there didn't seem to be any injection points that are worthwhile on testing, whenever I'm stuck what I like to do is check the /robots.txt, /secrets.txt and finally /sitemap.xml. These are usual hidden directories that could house useful information during ctfs so I always give them a check. For this particular challenge, the robots.txt directory does exist and have information:
![image](https://github.com/user-attachments/assets/e5b12887-3bd2-4906-bbe7-ef5143ebeff6)

From this we can tell there is a secret directory that isn't on the home page. Visiting this page prompts the user with a top secret government access page:
![image](https://github.com/user-attachments/assets/74f54ff0-f422-4d61-8c66-ef769559a44f)

If you inspect the code you can see that the parameters taken in are the usual username and password but the password is compared to a password hash. Hmmm, if we can find this hash we could reverse it and get the password. Looking through the source code of the page I noticed there's an asset linked that says auth.js . This must be the authentication code, I figured this was also the way to go because the robots.txt document hints that you are able to access the assets of the website.
![image](https://github.com/user-attachments/assets/a8941590-a3b7-4fc3-801d-cae241963982)

Upon checking the auth.js file we can see there is a username and password hash:
![image](https://github.com/user-attachments/assets/ea4d70b8-bd41-4f72-b8fe-94ef669a79ad)

Now we have the password, so next we need to get the password from the hash. For this part I think there's multiple ways you can go about it such as bruteforcing the hash with hashcat and a wordlist to get the password but usually for challenges like these I like to use https://md5decrypt.net/en/Sha256/ . This website has a ton of stored hashes that have been decrypted with their actual string counterpart such as the hash that's being used for this challenge:
![image](https://github.com/user-attachments/assets/f4679146-e78c-44e3-888f-2f498fcd2e68)

Okay now that we have the username and password we can login to the website, once logged in we are given the option to view and download 4 different images. 
![image](https://github.com/user-attachments/assets/1874cbf0-ad2d-4a90-98c5-280b6490587e)


# Solution (Didn't solve during CTF)
As I expected this was a path traversal exploit but I couldn't get the correct traversal to get the flag as I was looking through files that were irrelevant. Reading the write-up from the CC it is apparently common knowledge that if there's a path traversal exploit you can utilize it to read the source code of the page you want at the directory `var/www/<pagename>` in this case it was `var/www/topsecret_a9aedc6c39f654e55275ad8e65e316b3.php`. Using normal path traversal didn't work either such as:
```
https://pizzaparadise.ctf.intigriti.io/topsecret_a9aedc6c39f654e55275ad8e65e316b3.php?download=/etc/passwd
```
This was because the start with `/assets/images/` was needed. So the solve for this is to download the source code of the page with this url:
```
https://pizzaparadise.ctf.intigriti.io/topsecret_a9aedc6c39f654e55275ad8e65e316b3.php?download=%2Fassets%2Fimages%2F../../topsecret_a9aedc6c39f654e55275ad8e65e316b3.php
```
![image](https://github.com/user-attachments/assets/461611b9-80fc-4d4d-845b-794ecf46448e)

in which you can see the flag. 

#L337upCTF2024 
