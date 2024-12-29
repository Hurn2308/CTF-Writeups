# Description
Uh.. Oh.. Help me, I just browsing funny cats memes, when I click download cute cat picture, the file that been download seems little bit wierd. I accidently run the file making my files shredded. Ughh now I hate cat meowing at me.

# Flag
wgmy{4a4be40c96ac6314e91d93f38043a634}
![image](https://github.com/user-attachments/assets/6b609a94-858b-414b-80d0-e42008ab7436)



# Analysis
First step was to analyze the hex of the file:

![image](https://github.com/user-attachments/assets/df36af4d-11fc-4d0d-b2d3-efb17e2a3a2b)


Looking at it I noticed within the file there a lot of meows? This is certainly not normal for the hex of a JFIF file, then I deduced that based on the challenge title the goal was to remove the meows to fix the "corruption" of the JFIF file. 

# Solution
To fix the file you can use cyberchef.
![image](https://github.com/user-attachments/assets/eb17a1fb-1c84-472a-a258-551607a799d8)


CTRL + F in cyberchef to find all of the meow words and replace them with nothing to remove them completely. After removing make sure to check if there any left that you may not have removed. Once it's completely removed export the file and you will get this image with the flag.
![image](https://github.com/user-attachments/assets/d66c646e-8c9e-4f9a-ac70-9b9de9ba0acc)

