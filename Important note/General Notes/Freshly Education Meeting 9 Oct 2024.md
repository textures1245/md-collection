
# Topic 
## Primary
1. Git flow
	2. Git Clone
	3. GIt Checkout
	4. GIt Branch 
	5. Git Status & Log
	6. GIt Pull & Merge & Fetch
	7. Git Reset & Revert
2. Basic App API Service (Node Express)
	1. HTTP Message
	2. CORS
	3. JSON
	4. Authentication & Authorization
4. Building And Deploy apps with Docker
	1. Dockerfile
		1. เกริ่น
		2. วัตถุประสงค์
		3. อะไรคือ Dockerfile
		4. การทำงาน
		5. วิธีใช้งาน
	2. SSL
		1. วัตถุประสงค์
		2. วิธีใช้งาน
5. Basic Frontend connected with Node Express
	1. Asynchronous and synchronous 
## Secondary
1. Unit Test & Integrated Test 


## Infographic
1. Git Flow

 **Git Diagram** ![[Pasted image 20241007120551.png]]

2. Basic App API Service (Node Express)
**HTTP Message Paradigm** https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages
![[Pasted image 20241007144004.png]]![2400](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/httpmsgstructure2.png)

```curl
   curl --cert payso_kbank.crt --key payso_kbank.key --location --request POST 
    'https://openapi-test.kasikornbank.com/v1/verslip/kbank/verify' 
    --header 'Content-Type: application/json' 
    --header 'Authorization: Bearer 5hKgIkEhQHAAWnE7mYLGzpRAsnSW' 
    --header 'Content-Type: application/json' 
    --data-raw '{
        "rqUID": "a1c6a7fd-b080-41f8-88d9-61ba818b4d12",
        "rqDt": "2024-10-08T14:22:54+07:00",
        "data": {
            "sendingBank": "004",
            "transRef": "014281155210CTF08333"
        }
      }'
```
