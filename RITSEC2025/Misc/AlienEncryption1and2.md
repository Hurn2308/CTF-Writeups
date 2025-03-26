# Description
We found some hard drives on an alien ship that we think contains important data, but they seem to be encrypted. Can you crack the passwords?

1. `68834fdcdff2a84a2844bd794aa9bcdf`
2. `5b9727bb9358137b79f6f8ccfd6fd5a4`
3. `ec77ff156c178c0c1537eba873ddbcdd`
4. `0ecfc8ea245300b44a4d0ede3c049d43`
5. `1d3404bcf57b2e7e6cba61a3318736ad`
6. `78ea73e4f2fc8765f5b520ec602ef948`

`nc alien-encryption.ctf.ritsec.club 32190`

# Flag
`RS{a_c0smic_adv3ntur3}`
`RS{l3ts_m!x_17_up`

# Analysis
For the first part of the challenge all of the hashes can be cracked at md5decrypt or crackstation.
The second half of the challenge requires you to build a custom wordlist with additional planets rather than the ones already mentioned and use wordlists to crack. You can use hashcat for this.



# Solution
![image](https://github.com/user-attachments/assets/ebd9a6d2-4d02-45cf-877b-748eeb523434)

`RS{a_c0smic_adv3ntur3}`
`RS{l3ts_m!x_17_up`
