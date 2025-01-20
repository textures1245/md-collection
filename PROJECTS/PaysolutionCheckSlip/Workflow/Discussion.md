[[Workflow Overview]]

## Date: 27/11/2024
	Discussion about user journey that would need to be changed. main topic would be Group Chat Based and Private OA Chat

1. Group Based Service (One-to-Many)
	   This journey, Merchants would have to create the room for service their customers. Which in the room everybody can send e-slip for verification.

2. OA Based Service (One-to-One)
	   Merchant customers can send e-slip for verification through our OA Chat for private conversation

### Workflow
1. Group Based Service
	```uml
	@startuml
	left to right direction
	actor Guest as g
	:Check Slip Service: as CS << Service >>
	:Token: as TK <<Data>> 
	json MerchantInfo {
	   "Qouta":"..",
	   "MerchantId":"..",
	   "LineUserId": ".."
	}
	package Admin {
	  actor SuperUser as SPU
	  actor SubUser as SU
	}
	package GroupChat {
	  usecase "Slip Verification" as UC1
	  usecase "Management" as UC4
	}
	
	package Action {
	 usecase "Verified Merchant Received Account" as UC5
	 usecase "Deduct Qouta" as UC6
	}
	
	Admin --> UC4
	Admin --> UC1
	g --> UC1
	UC1 --> CS : Request
	CS --> TK : Finding Merchant Info
	CS --> UC5
	CS --> UC6
	SPU .> MerchantInfo : <<extends>>
	MerchantInfo .> TK : <<includes>>
	TK .> Action : <<includes>>
	@enduml
	```
![[Pasted image 20241126232532.png]]

2. Private OA Chat
	```uml
	@startuml
	left to right direction
	actor Guest as g
	:Check Slip Service: as CS << Service >>
	json MerchantInfo {
	   "Qouta":"..",
	   "MerchantId":"..",
	   "LineUserId": ".."
	}
	package Admin {
	  actor SuperUser as SPU
	  actor SubUser as SU
	}
	package LineOA {
	  usecase "Slip Verification" as UC1
	}
	
	package Action {
	 usecase "Slip Verification" as UC7
	 usecase "Verified Merchant Received Account" as UC5
	 usecase "Deduct Qouta" as UC6
	}
	
	Admin --> UC1
	g --> UC1
	UC1 --> CS : Request
	CS --> UC7
	SPU .> MerchantInfo : <<extends>>
	@enduml
	```
![[Pasted image 20241127110527.png]]
### **Key Comparisons**

| **Aspect**               | **Group-Based (One-to-Many)**                                                                           | **OA-Based (One-to-One)**                                                                   |
| ------------------------ | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Privacy**              | Low – Group members share the same space.                                                               | High – Private session per customer.                                                        |
| **Concurrency Resource** | Using single Agent data (Token). Since it shared group state and each room can have only one agent data | May using more resource to find out who agent data is. Since there is no shared group state |
| **Scalability**          | Scales by group, using fewer resources.                                                                 | Scales by user, using more resources.                                                       |
| **Integration**          | Requires group context-awareness to integrated. but clustered                                           | Simpler to integrate. but non-clustered                                                     |
| **Infrastructure Cost**  | Lower per user (track by room).                                                                         | Higher per user (track by user, may need to track user data into database)                  |


Discussion

UX แย่
1. การใช้งานหน้าบ้าน