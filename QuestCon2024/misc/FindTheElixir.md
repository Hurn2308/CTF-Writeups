#Find the Elixer -180 
To save the people from the Asuras, the gods send forth the Elixir of immortality. But the Asura, who calls himself iamkali3x3, has stolen the Elixir and infused it with his dark powers into it, corrupting its essence. Now, it is up to you to uncover the original Elixir and restore balance before chaos reigns. (OSINTxCrypto)

#Solution
Reading through the challenge description first thing to note was in the challenge description the "Asura" is named "iamkali3x3" so immediately I used sherlock to get all websites that have a user named iamkali3x3. A github user registered with the same name was one of the results of sherlock so I visited the page to see if there was any important information. Upon checking the user's profile there are two repositories, one named pandoras-box the other named hermes key.
![image](https://github.com/user-attachments/assets/e3a1a2ab-3dfd-4a5b-a335-a6fb902ac869)

Reading through the pandoras-box repository there are two files a read.md and a python file elysium-elixir.py, it's safe to assume this is the user we are looking for. Analyzing the code it seems to take an input from the user and encode it into binary, this is important for later. Checking the other repository hermes key there was a text file with some binary from this we can deduce the goal is to decode it.
![image](https://github.com/user-attachments/assets/0986c351-1e58-439f-bdb0-3ff8fd6a4546)

Looking at the binary I felt like reading it from left to right made no sense so under this assumption I assumed that the key is most probably meant to be read from up to down or down to up. After initially attempting to decode the key the result I got was a bunch of gibberish so I felt like I was missing something, I went back to read the challenge description and noticed that at one point in the description it tells you to uncover the ORIGINAL elixir. From this I deduced the one currently being shown was not the real elixir. Sure enough as I checked the commit history of the repository there is an original text file with the original elixir. 
![image](https://github.com/user-attachments/assets/9af52ab8-2bd1-4c89-96af-68981760ed06)

Now taking this new key I decoded it from binary to text and I received the flag: QUESTCON{WlgoQ2tg9ZBjH0ZZtlL1}
![image](https://github.com/user-attachments/assets/ae89b01c-481d-4df8-ae07-77781a283824)

