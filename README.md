# Nginx_JWT_logging

Goal:
	Configure Nginx to Log all request and responce headers that come from AWS ALB to backend.
	ALB listener on port 80 configured to redirect to port 443.
	ALB listener on port 443 configured to use AWS Cognito auth provider.
	App backend - Nginx server configured to listen on port *:80.
	Nginx backend logs all requests and responces to
    ```
        /var/log/nginx/server.log
    ```
     
##On Ubuntu:
	1. Install required packages:
	```
		apt update; apt install python-pip; 
		pip install ansible; 
		curl --silent --location  https://raw.githubusercontent.com/viasite-ansible/ansible-role-zsh/master/install.sh | sudo bash -
		apt install nginx-extras
	```

	2. Configure nginx ligging (requres mod_lua)
	```
		cd /etc/nginx
		cp nginx.conf nginx.conf.bcu
		vim nginx.conf
	```

# Results:
* Use https://jwt.io/ for decodding

## Logged Headers:
  x-amzn-oidc-identity: 21f782e8-6bd2-4363-9134-0ce990b8c873
  host: debugger.example.com
  x-amzn-oidc-data: eyJ0eXAiOiJKV1QiLCJraWQiOiI3ZTVlODNlOS02ZjRiLTQwNzAtYjY0Yi00NDNlYmFlNDliNzMiLCJhbGciOiJdddd1NiIsImlzcyI6Imh0dHBzOi8vY29nbml0by1pZHAudXMtd2VzdC0yLmFtYXpvbmF3cy5jb20vdXMtd2Vzwe0yX1RJRjk0aFliZSIsImNsaWVudCI6IjN1NXQzZjJsYmVpZXIwdmVibGN1NnRkNWYzIiwic2lddddddjoiYXJuOmF3dzplbGFzdGljbG9hZGJhbGFuY2luZzp1cy13ZXN0LTI6NDgxMjc3NTIyMDMzOmxvYWRiYWxhbmNlci9hcHAvcHJveHloZWFkZXJzLzE2MDMyN2UzMjAyZjM1NmYiLCJleHAiOjE1NjY0ODAxNzZ9.eyJzdWIiOiIyMWY3ODJlOC02YmQyLTQzNjMtOTEzNC0wY2U5OTBiOGM4NzMiLCJlbWFpbF92ZXJpZmllZCI6ImZhbHNlIiwiZ2l2ZW5fbmFtZSI6IlZpdGFsaWkiLCJmYW1pbHlfbmFtZSI6IlNhbW90YWldddddImVtYWlsIjoidml0YWxpaS5zYW1vdGFpZXZAY3JlYXRvci5yZXN0IiwiY3VzdG9tOmdyb3VwcyI6IltEZWZhdWx0LCByb2JvdGljcy1zb2Z0d2FyZS1hZG1pbnMsIHJvYm90aWNzLXNvZnR3YXJlLXVzZXJzXSIsInVzZXJuYW1lIjoiQ3JlYXRvci1PbmVMb2dpbl92aXRhbGlpLnNhbW90YWlldkBjcmVhdG9yLnJlc3QiLCJleHAiOjE1NjY0ODAxNzYsImlzcyI6Imh0dHBzOi8vY29nbml0by1pZHAudXMtd2VzdC0yLmFtYXpvbmF3cy5jb20vdXMtd2VzdC0yX1RJRjk0aFliZSJ9.hTQG4heB8GwMfbbpNFF886L2Y9hn8s-GS5454426tCncrw6_m2sQ9xHOnwRM7D2BNODWizTWdXnEWaNjSHkhAA==
  x-forwarded-proto: https
  if-modified-since: Tue, 17 Apr 2018 15:22:36 GMT
  if-none-match: W/\x225ad6113c-264\x22
  upgrade-insecure-requests: 1
  x-forwarded-for: 156.67.50.7
  cookie: 
  accept-language: en-US,en;q=0.9,ru;q=0.8
  accept-encoding: gzip, deflate, br
  cache-control: max-age=0
  sec-fetch-site: none
  accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
  sec-fetch-user: 1
  sec-fetch-mode: navigate
  x-forwarded-port: 443
  x-amzn-trace-id: Root=1-5d5e96b8-256d5e1c0b0e2c6a92e91a37
  x-amzn-oidc-accesstoken: eyJraWQiOiJUNdddddV1MXBcL0ZyS08yTCtGKytyNndXdnk1dddddHFXS1lMffffffpsYjg9IiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiIyMWY3ODJlOC02YmQyLTQzNjMtOTEzNC0wY2U5OTBgfgfgfgfiLCJjb2duaXRvOmdyb3VwcyI6WyJNeVRlc3RHcm91cCJdLCJ0b2tlbl91c2UiOiJhY2Nlc3MiLCJzY29wZSI6Im9wZW5pZCIsImF1dGhfdGltZSI6MTU2NjQwMDkyNiwiaXNzIjoiaHR0cHM6XC9cL2NvZ25pdG8taWRwLnVzLXdlc3QtMi5hbWF6b25hd3MuY29tXC91cy13ZXN0LTJfVElGOTRoWWJlIiwiZXhwIjoxNTY2NDgzNDUzLCJpYXQiOjE1NjY0Nzk4NTMsInZlcnNpb24iOjIsImp0aSI6IjZlODFmMDhjLTI4M2EtNDg5NC04ZjRmLWIzYmExODYwZGVmMSIsImNsaWVudF9pZCI6IjN1NXQzZjJsYmVpZXIwdmVibGN1NnRkNWYzIiwidXNlcm5hbWUiOiJDcmVhdG9yLU9uZUxvZ2luX3ZpdGFsaWkuc2Ftb3RhaWV2QGNyZWF0b3IucmVzdCJ9.BuTI8qRFeI1G2cwLDscIYNwaLNXIlEib1mlPKezl_0YvveDpNOtl9RTYRtCAebvo9JsqHm0s7ChSMoc3JrFRFnrO0D8GOvQUI74YyjBdnyAaKKqpcBBdbER1v9TMUFfIR2VVY3uLVcvadpaJLnMNrDb3A1gu0bup6P-AqQ7vlXctAhRrvD1DdHTb4LgL08qMswRemmQEU9mI67hOfh1YiyIP5HZSOVVzL0KSnfnUugUkGyh2Cen2NRhyV_Gb5_3sndFtAvj1uJco0AUbfrnzoy4o7fGOzcbOO4Cr2gi_nsTkLdk_XeuyTYWrEzomfffffBrjbjsRh0mytLkfennVtg
  user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.87 Safari/537.36


# Decoded x-amzn-oidc-data:
## HEADER:
```
{
  "typ": "JWT",
  "kid": "7e5e83e9-6f4b-4070-b64b-443ebae49b73",
  "alg": "ES256",
  "iss": "https://cognito-idp.us-west-2.amazonaws.com/<cognito_userpool_id>",
  "client": "<cognito_app-client_id>",
  "signer": "arn:aws:elasticloadbalancing:us-west-2:<aws_account_id>:loadbalancer/app/proxyheaders/160327e3202f356f",
  "exp": 1566480176
}
```

## PAYLOAD:
```
{
  "sub": "21f782e8-6bd2-4363-9134-0ce990b8c873",
  "email_verified": "false",
  "given_name": "<name>",
  "family_name": "<family_name>",
  "email": "<email>",
  "custom:groups": "[Default, <group_name_1>, <group_name_2>]",
  "username": "<cognito_username>",
  "exp": 1566480176,
  "iss": "https://cognito-idp.us-west-2.amazonaws.com/<cognito_userpool_id>"
}
```


# Decoded x-amzn-oidc-accesstoken:
## HEADER:
```
{
  "kid": "T5QnyUu1p/FrKO2L+F++r6wWvy5IrYlqWKYLQ/mzlb8=",
  "alg": "RS256"
}
```

## PAYLOAD:
```
{
  "sub": "21f782e8-6bd2-4363-9134-0ce990b8c873",
  "cognito:groups": [
    "<groupname>"
  ],
  "token_use": "access",
  "scope": "openid",
  "auth_time": 1566400926,
  "iss": "https://cognito-idp.us-west-2.amazonaws.com/<cognito_userpool_id>",
  "exp": 1566483453,
  "iat": 1566479853,
  "version": 2,
  "jti": "6e81f08c-283a-4894-8f4f-b3ba1860def1",
  "client_id": "<cognito_app-client_id>",
  "username": "<cognito_username>"
}
```

# Python App that echo Headers and decode JWT:
## Installation:
  ```
    install python-pip3
    pip3 uninstall JWT
    pip3 uninstall python-jwt
    pip3 install pyjwt
  ```
