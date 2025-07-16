# Description
Oh my god, I just can't with these concert ticket queues. It's gotten out of control.

# Flag
flag{b1bd4795215a7b81699487cc7e32d936}


# Analysis
Submit a JWT token to attempt to use the none alg attack, it doesn't work but it exposes the jwt secret
<img width="965" height="476" alt="image" src="https://github.com/user-attachments/assets/0f6cf892-0694-4e83-848c-96451b61fa19" />

This challenge is not vulnerable to client side so it has to be jwt token manipulation as far as I know.

# Solution
Use the found secret to generate a valid token:
![[Pasted image 20250525153324.png]]

it will skip your turn and you can visit the complete purchase screen to download the pdf that has the flags:
![[Pasted image 20250525153359.png]]

The flag:
![[Pasted image 20250525153420.png]]


