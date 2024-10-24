#California Dreamin'
The Professor has uncovered a critical piece of evidence: a dashcam photo retrieved from a hostile subject’s car, suspected of planning an infiltration at a nearby military installation. Your task is to utilize OSINT skills to stop this threat before it's too late.

Analyze the dashcam image to determine the exact coordinates where it was taken. Identify the nearest military installation to this location . Find the distance by road (in miles) from the exact coordinates of the dashcam image to the military installation. Round off the mile count to the nearest whole number.

Image:
![image](https://github.com/user-attachments/assets/d17225a5-7903-42c5-b996-3d5498e2456a)


Flag format: QUESTCON{<Name of airforce base>-<rounded off mile count>} The name should be one word and lowercase

#Solution
I first tried to use google lens to identify where this signboard in the image is but it ultimately resulted in no useful information so I moved on to google searching "rosamond 6 lancaster 17 los angeles 88" which led me to this website https://www.aaroads.com/california/ca-014sb.html , in this website I was able to find the route and direction this highway was which can be seen in the image below. 

![image](https://github.com/user-attachments/assets/dee4b0f3-4698-46a0-9e79-cc19d36c1341)

I then used google maps to find the exact location of the signboard and googled "what is the nearest military installation to the California 14 Antelope Freeway" and got the answer Edwards AFB. Using google maps i checked for the directions to Edwards AFB and the distance in miles which was 23.3 and rounded it to 23. So I got the flag: QUESTCON{edwards-23}

