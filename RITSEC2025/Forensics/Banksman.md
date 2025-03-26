# Description
Our professor received a report from an unfamiliar student. With his experience, the professor realized that this report was abnormal. He immediately used this file for our research assignment. Let's analyze whether there is anything mysterious embedded in it!

_(This challenge was written by MetaCTF. The flag format is `MetaCTF{}`)_

# Flag
`MetaCTF{I_4m_n0t_@_m1n3r_1_@m_a_b4nk5m4n} `


# Analysis
We were provided a pdf file usually when pdf files are provided I like to use pdftohtml to extract any extra information that we may be missing. For this challenge however I started by using strings on the file, after using strings I noticed there was JS tag for Javascript code. So I had to extract the javascript code to examine it if it was anything useful:
![image](https://github.com/user-attachments/assets/5e7360a3-09ec-4318-89ff-682668c75651)


After decoding the javascript from Hex to Text we can see that the javascript is taking base64 strings and decoding them, the script also likely contains an executable code that would inject a virus as I tried saving just the code to my computer but it was blocked by Windows Defender.
![image](https://github.com/user-attachments/assets/8bd5ba66-40b9-481c-bbda-18fce949aef9)


# Solution
To solve base64 decode all of the strings until you find the string with MetaCTF and you will find the flag.
