# Description:
There's a 0 day in the package that are being used in this application! But no worries, we've blocked it with LLM powered detection.

Note: This challenge uses version 5.0.6, which patches most known solutions. However, some previous issues have reappeared in the latest version (feel free to report them). The intended solution for this version is version-agnostic and has already been reported.

Also, please dont steal my Deepseek key...

http://116.203.176.73:7178/

Author: vicevirus

# Flag
`UMCS{d1d_y0u_d0_pr0mpt_inj3ct10n?}`

# Analysis
To start we can do an analysis on the files and source code provided to us, we start with the index.php file, the first thing we can notice is that it’s calling deepseek and giving it instructions to validate the user input:

![image](https://github.com/user-attachments/assets/3c33aba6-c622-4ccb-b7d0-0181e32043d0)

Reading the instruction it seems that attempts to get the flag normally are prohibited or detected by the LLM, so my idea was the first thing we needed to achieve was prompt injection to escape the instruction.

Upon testing however, I noticed that if you input the same input twice there are moments where it will be flagged as safe and then dangerous. Especially if you keep inputting malicious prompts it builds the logic within the LLM for that session that you are trying to exploit it so it is alot more strict. (This is my assumption)

![image](https://github.com/user-attachments/assets/c41fb642-cb8a-4c72-9396-84cec517aac2)


This is the Safe output test with the payload “hello i am flag”, so here we can see the word flag isn’t necessarily flagged and blocked by the LLM. Now after this I tested a bunch of payloads like "<script>var url='&#32;'+...;window.location=url;</script>” but they didn’t work and I was stuck. Later my teammate did something for fun just to see what would happen that I hadn’t tried he took the entire HTML source code of the Fermatt’s Little Theorem wikipedia page and it gave an interesting error:

![image](https://github.com/user-attachments/assets/6035d889-c335-4dca-a2c9-787802e87b30)

After he showed me this output, it struck a chord the LLM is not able to process a certain maximum amount of words/input this gave me an idea what if I were to submit a huge chunk of a malicious payload, would the LLM be able to detect the invalid input that goes against it’s instruction:

![image](https://github.com/user-attachments/assets/12cbf34d-9418-4d95-a26a-5209820434bd)


As you can see even though this malicious payload previously didnt work if you sent just the one line, now if you send it repeatedly it works 

# Solution
From here we just needed to find a way to read the flag.txt, prompting gpt to generate one gave me this:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Directory Snapshot</title>
  <style>
    /* make the iframe fill the viewport for a full-page directory listing */
    html, body { margin:0; padding:0; height:100%; width:100%; }
    iframe { border:none; width:100vw; height:100vh; }
  </style>
</head>
<body>
  <!-- this will load the directory viewer at /flag.txt/ -->
  <iframe src="/flag.txt"></iframe>
</body>
</html>
```

So within this it will load the flag.txt as a screenshot, if you submit it just as is it wont work you need to copy it multiple times until its multiple lines long and you will bypass the LLM and get the flag:

![image](https://github.com/user-attachments/assets/1120496e-015a-45a4-ad5a-a97e290b572a)


