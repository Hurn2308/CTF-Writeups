# Anti Spirit F Captcha
![image](https://github.com/user-attachments/assets/34bcce91-008c-473d-b095-fa6eb61761e2)

# Solution
For this challenge I unfortunately do not have screenshots of the actual website so I will have to explain it, okay so to start when you reach the website there is a captcha checkbox like most other sites but with the twist that you can't check it because you'll be agreeing to not being human. So to avoid this if you read the source code you can check the captcha by using your keyboard instead of mouse. Once you fulfill this check theres another check in the source code you need to hold Ctrl + (another key I forgot) then once you do this it'll redirect you to a page that's supposed to be an image but you can't see, all you will see is the base64 data of the image. You can copy paste the base64 and put it into this website to get the image: https://base64.guru/converter/decode/image

Which is:
![image](https://github.com/user-attachments/assets/85d6ee35-dbf0-4300-9388-e6dc2f4193c2)

