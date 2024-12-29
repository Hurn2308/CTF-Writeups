# Description


"[25, 10, 0, 3, 17, 19, 23, 27, 4, 13, 20, 8, 24, 21, 31, 15, 7, 29, 6, 1, 9, 30, 22, 5, 28, 18, 26, 11, 2, 14, 16, 12]"

Author: Yes

The element is the number in the list, combine for the flag. Wrap in wgmy{}

# Flag
wgmy{51fadeb6cc77504db336850d53623177}


# Analysis
This was a misc challenge, the files that was provided was a file labelled challenge.dcm with a .dcm extension. For me personally whenever I encounter unknown file extensions I google them and always xxd to check the hex of the file. Upon inspecting it you get this:
![image](https://github.com/user-attachments/assets/8048e9de-383f-4c21-8dc3-06d6b68e52c0)


Upon putting extracting the characters, you get WGMYf63acd3b78127c1d7d3e700b55665354 which obviously looks like the flag if you put it in the wrapper as such wgmy{f63acd3b78127c1d7d3e700b55665354}. However, the challenge cannot be that simple as an array was provided. After counting the number of elements within the array I noticed that there are 32 which is identical as the number of characters in the string 'f63acd3b78127c1d7d3e700b55665354'.
# Solution
From here I could deduce that the array was the the position mapping of the characters so we can write a script to arrange it for us:
```
def decode_message(order, values):
    result = ''
    for idx in order:
        result += values[idx]
    return result

order = [25, 10, 0, 3, 17, 19, 23, 27, 4, 13, 20, 8, 24, 21, 31, 15, 7, 29, 6, 1, 9, 30, 22, 5, 28, 18, 26, 11, 2, 14, 16, 12]
values = "f63acd3b78127c1d7d3e700b55665354"

decoded = decode_message(order, values)
print("wgmy{" + decoded + "}")
```

![image](https://github.com/user-attachments/assets/ece041a9-0492-49a0-bd47-d68f0430e757)

and you will get the flag.
