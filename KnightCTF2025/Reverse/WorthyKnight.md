
# Flag
KCTF{NjkSfTYaIi}

# Analysis
Inputting the key we can view the check between the all pairs of the string which are validated, the only one that isn't valid is the md5 hash check between characters 4:5, this should not be the case as we acquired the correct two letters from decrypting the md5 hash which was "Tf".
```
Welcome, traveler. A mighty dragon blocks the gate.
Speak the secret incantation (10 runic letters) to continue.

)         = 575
printf("Enter your incantation: ")                  = 24
fgets(Enter your incantation: NjkSTfYaIi
"NjkSTfYaIi\n", 128, 0x7fb340df18e0)          = 0x7ffc69735cb0
strcspn("NjkSTfYaIi\n", "\n")                       = 10
strlen("NjkSTfYaIi")                                = 10
__ctype_b_loc()                                     = 0x7fb3413eae60
strlen("fT")                                        = 2
MD5(0x7ffc69735c6c, 2, 0x7ffc69735c70, 8)           = 0x7ffc69735c70
sprintf("4c", "%02x", 0x4c)                         = 2
sprintf("6d", "%02x", 0x6d)                         = 2
sprintf("55", "%02x", 0x55)                         = 2
sprintf("8e", "%02x", 0x8e)                         = 2
sprintf("64", "%02x", 0x64)                         = 2
sprintf("2c", "%02x", 0x2c)                         = 2
sprintf("fe", "%02x", 0xfe)                         = 2
sprintf("26", "%02x", 0x26)                         = 2
sprintf("b5", "%02x", 0xb5)                         = 2
sprintf("5d", "%02x", 0x5d)                         = 2
sprintf("79", "%02x", 0x79)                         = 2
sprintf("1e", "%02x", 0x1e)                         = 2
sprintf("d6", "%02x", 0xd6)                         = 2
sprintf("d4", "%02x", 0xd4)                         = 2
sprintf("18", "%02x", 0x18)                         = 2
sprintf("0e", "%02x", 0xe)                          = 2
strcmp("4c6d558e642cfe26b55d791ed6d4180e"..., "33a3192ba92b5a4803c9a9ed70ea5a9c"...) = 1
puts("\nThe dragon's eyes glow red... T"...
The dragon's eyes glow red... The final seal remains locked.
)        = 62
puts("   The ancient dragon roars: "Be"...   The ancient dragon roars: "Begone, unworthy!"
```

Inspecting the comparison through ltrace we can see that the program flips "Tf" to "fT" during this check which is what is resulting in the incorrect md5 hash comparison. To fix this we can change the inputted key to `NjkSfTYaIi` which will give the valid md5 hash check comparison.

# Solution
Taking the information from the binary we can XOR the given values of known indexes within the string to get the unknown indexes. We lastly only needed the characters 4:5 which can be acquired from decrypting the md5 hash found in the binary.

solve.py:
```
# Given known characters
known_chars = {
    1: 106,  # 'j'
    3: 83,   # 'S'
    7: 97,   # 'a'
    9: 105   # 'i'
}

# XOR relationships from the code
def find_missing_characters():
    # Calculate characters[0] from characters[1] and the XOR relationship
    characters_0 = known_chars[1] ^ 36
    print(f"characters[0]: {chr(characters_0)} (ASCII: {characters_0})")
    
    # Calculate characters[2] from characters[3] and the XOR relationship
    characters_2 = known_chars[3] ^ 56
    print(f"characters[2]: {chr(characters_2)} (ASCII: {characters_2})")
    
    # Calculate characters[6] from characters[7] and the XOR relationship
    characters_6 = known_chars[7] ^ 56
    print(f"characters[6]: {chr(characters_6)} (ASCII: {characters_6})")
    
    # Calculate characters[8] from characters[9] and the XOR relationship
    characters_8 = known_chars[9] ^ 32
    print(f"characters[8]: {chr(characters_8)} (ASCII: {characters_8})")
    
    # Placeholder for characters[4] and characters[5] (MD5 part with ROL2)
    # MD5 decryption check here. You can try combinations that would produce
    # the desired MD5 hash after the rotation operation (ROL2).
    print("\nCharacters[4] and [5] need to be determined through MD5 hash check.")

# Run the function to find missing characters
find_missing_characters()

```

Final ASCII value of characters:
```
- `characters[0]`: 'N' (ASCII: 78)
- `characters[1]`: 'j' (ASCII: 106)
- `characters[2]`: 'k' (ASCII: 107)
- `characters[3]`: 'S' (ASCII: 83)
- `characters[4]`: 'T' (ASCII: 84) (from MD5)
- `characters[5]`: 'f' (ASCII: 102) (from MD5)
- `characters[6]`: 'Y' (ASCII: 89)
- `characters[7]`: 'a' (ASCII: 97)
- `characters[8]`: 'I' (ASCII: 73)
- `characters[9]`: 'i' (ASCII: 105)
```
