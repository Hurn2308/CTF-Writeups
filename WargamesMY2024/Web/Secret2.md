# Description
![image](https://github.com/user-attachments/assets/680e66fb-e325-4cde-88bf-f901b8a89d2f)


# Flag
wgmy{1bc665d324c5bd5e7707909d03217681}


# Analysis
My advice is to focus solely on the files related to the nginx configurations. To start you need to analyze the code that is provided with the challenge. After thorough inspection you will notice that the `vault/v1/kv/data/flag` endpoint is exposed and can be accessed externally which you can see in this file:
![image](https://github.com/user-attachments/assets/e6511451-e6a8-4a7e-a8bb-89e4430bf79a)


This is where the flag is stored, now the only question remains how do we get access to the endpoint and get the flag?

If you try to curl the endpoint normally you will receive 403 forbidden access, so that means there's something we need to pass to gain access to the endpoint.
![image](https://github.com/user-attachments/assets/415402a0-0f82-4e66-b359-1fb691e13038)


If you read through this [link](https://developer.hashicorp.com/vault/api-docs) you will find that to access a vault endpoint you need the header X-Vault-Token and since reading through the source code we can find that it is in development mode with a bit of searching we can find the default token is root.


# Solution
So now the solution is to try to access the endpoint with the header X-Vault-Token with value root.

`curl -H "X-Vault-Token: root" http://nginx.wgmy/vault/v1/kv/data/flag`
![image](https://github.com/user-attachments/assets/bce78e78-3dd4-44fd-9094-3aa26af9595e)


