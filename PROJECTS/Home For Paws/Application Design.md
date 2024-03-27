### #UML-Pattern-Design 

#Use-case-Diagram
	Core actors are `Adopter` the person who wants to adopt pets  and `Sender` the person who wants to send their dog to Adopter
- **Actors**
	- Adopter:  Looking to adopt from `Sender`
	- Sender: Sending their pets to `Adopter`
- Use-case
	- **Find and match adopters with dogs seeking homes :**** 
	- Sub-Use Case: **Define adoption terms and pricing**.
	- Sub-Use Case: **Initiate communication between Adopter and Sender**.
	- Sub-Use Case: **Facilitate the adoption transaction**.

#Sequence-Diagram
	
	1. Adopter sends a request to find a dog.
	2. System matches Adopter with available dogs and sends options.
	3. Adopter selects a dog and defines adoption terms.
	4. System notifies Sender about the interest.
	5. Sender responds, and communication is initiated.
	6. Adoption transaction details are confirmed.
	7. Adopter pays, and the system deducts the service fee.
	8. Transaction details are stored.
```mermaid
%%{init: { 'themeVariables': { 'fontSize': '10px', 'fontFamily': 'Inter'}}}%%

graph TD
  Start((Start)) -->|New User?| A[User Registration]
  Start -->|Existing User| B[User Login]
  A --> C{User Type?}
  B --> C{User Type?}
  C -->|Adopter| D[Create Dog Listing]
  C -->|Sender| E[Search and Filter Dog Listings by nearest location]
  D --> F[Communication System]
  E --> F
  F -->|Interested in Offer?| H{Business Chatting}
  H -->|Yes| I[Send Offer to Adopter]
  H -->|No| J[Adoption Process Failed]
  I -->|Accept Offer?| K{Interested in Offer?}
  K -->|Yes| L{On Transaction Processing}
  L -->|No| J
  K -->|No| J
  L --> |Success| O[Adoption Process Success ]
  O --> End((End))
  J --> End

```

#Activity-Diagram
### 1. **Adoption Process**
```mermaid
%%{init: { 'themeVariables': { 'fontSize': '10px', 'fontFamily': 'Inter'}}}%%
graph TD
  Start[Adopter initiates the adoption process.]
  Activity1[System matches Adopter with available dogs.]
  Decision1[Does the Adopter find a suitable dog?]
  Yes1[Define adoption terms.]
  No1[End process.]
  Activity2[System notifies Sender.]
  Activity3[Sender responds, and communication is initiated.]
  Activity4[Confirm adoption details and initiate the transaction.]
  Decision2[Is the payment successful?]
  Yes2[Deduct service fee and store transaction details.]
  No2[Notify Adopter and Sender about the payment failure.]
  End[Adoption process complete.]

  Start --> Activity1
  Activity1 --> Decision1
  Decision1 -- Yes --> Yes1
  Decision1 -- No --> No1
  Yes1 --> Activity2
  Activity2 --> Activity3
  Activity3 --> Activity4
  Activity4 --> Decision2
  Decision2 -- Yes --> Yes2
  Decision2 -- No --> No2
  Yes2 --> End
  No2 --> End
```

### 2. Matching Pet 
#### - **Algorithm features**

| #Geographical-Proximity | #User-Preference |  |
| ---- | ---- | ---- |
| ![[Pasted image 20240207164300.png]] | ![[Pasted image 20240207164252.png]] |  |
| #Budget-Matching | #User-Feedback-and-Ratings | #Machine-Learning (optinal)<br> |
| ![[Pasted image 20240207164311.png]] | ![[Pasted image 20240207164317.png]] | ![[Pasted image 20240207164323.png]] |

#### 2.1 Sequence diagram for Actors usage

| #Sender                              |
| ------------------------------------ |
| ![[Pasted image 20240207170900.png]] |
| #Adopter                             |
| ![[Pasted image 20240207171652.png]] |
### 3. Review Process

| #Review-and-Rating-Process |
| ---- |
| ![[Pasted image 20240217151926.png]] |
### 4.   Related with Proposal Transaction State Diagram
| #Payment-and-Proposal-Process |
| ---- |
| ![[Pasted image 20240220112809.png]] |

