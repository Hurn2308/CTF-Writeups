# Flag
KCTF{d38787fb0741bd0efdad8ed01f037740}

# Analysis
When decrypting the initial base64 endpoint you get `yaml: something`. From this we can deduce there is a yaml vulnerability, upon inspecting the response of the server we can see that the server is running on a python server. Based on this we can make the assumption this is a pyyaml or yaml deserialization vulnerability.

I did some research on this [link](https://book.hacktricks.wiki/en/pentesting-web/deserialization/python-yaml-deserialization.html?highlight=yaml#yaml--deserialization) where you can find common yaml deserialization exploits. Within this I tested to see the directory that we are currently at to view what other directories are present. 
![image](https://github.com/user-attachments/assets/f068c0b8-dba6-40e4-beb0-355aa89741e8)

From the output of the previous command we can see there is an app directory, based on experience if you view the app/app.py of a python server you will be able to see any hardcoded values which may include the flag so I decided to inspect it.
![image](https://github.com/user-attachments/assets/c321858c-97bf-4b7f-93c5-bb7f06da56b8)


# Solution
And there we go we got the flag.

Used exploit link:http://172.105.121.246:5990/eWFtbDogISFweXRob24vb2JqZWN0L2FwcGx5OmV2YWwgWyJfX2ltcG9ydF9fKCdvcycpLmxpc3RkaXIoJy9hcHAnKSJd
![image](https://github.com/user-attachments/assets/41838016-c954-43cd-b3cb-16e6c7bd44a9)


Exploit: `yaml: !!python/object/apply:eval ["__import__('os').listdir('/app')"]`
