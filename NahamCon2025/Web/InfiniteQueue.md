# Description
Oh my god, I just can't with these concert ticket queues. It's gotten out of control.

# Flag
flag{b1bd4795215a7b81699487cc7e32d936}


# Analysis
Submit a JWT token to attempt to use the none alg attack, it doesn't work but it exposes the jwt secret
<img width="957" height="487" alt="image" src="https://github.com/user-attachments/assets/f850d7ee-0249-4545-a1c7-d7fa4aff38a1" />

This challenge is not vulnerable to client side so it has to be jwt token manipulation as far as I know.

# Solution
Use the found secret to generate a valid token:
<img width="702" height="280" alt="image" src="https://github.com/user-attachments/assets/e5206011-6a84-4f3b-bdd1-7307c30d830e" />

it will skip your turn and you can visit the complete purchase screen to download the pdf that has the flags:
<img width="695" height="528" alt="image" src="https://github.com/user-attachments/assets/93829735-09fb-4abc-bdba-0c1dbb970e2a" />

The flag:
<img width="701" height="366" alt="image" src="https://github.com/user-attachments/assets/65710430-4cef-490f-ac9b-4e6961819e1a" />



