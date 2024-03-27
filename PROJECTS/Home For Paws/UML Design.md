### #UML-Pattern-Design   

**Use-case Diagram**
	Core actors are Adopter the person who wants to adopt pets and Sender the person who wants to send their dog to Adopter
- Actors
	- Adopter: Looking to adopt from Sender
	- Sender: Sending their pets to Adopter
	- Use-case
	- Find and match adopters with dogs seeking homes :
	- Sub-Use Case: Define adoption terms and pricing.
	- Sub-Use Case: Initiate communication between Adopter and Sender.
	- Sub-Use Case: Facilitate the adoption transaction.
==Sequence Diagram== 
	1. Adopter sends a request to find a dog.
	2. System matches Adopter with available dogs and sends options.
	3. Adopter selects a dog and defines adoption terms.
	4. System notifies Sender about the interest.
	5. Sender responds, and communication is initiated.
	6. Adoption transaction details are confirmed.
	7. Adopter pays, and the system deducts the service fee.
	8. Transaction details are stored.

```mermaid
%%{init: { 'themeVariables': { 'fontSize': '10px', 'fontFamily': 'Inter'} }}%%

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
<mark style="background: #BBFABBA6;">Activity Diagram</mark>

1. **Adoption Process**
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

2. Matching Algorithm
	- #User-Preference
```mermaid
%%{init: { 'themeVariables': { 'fontSize': '10px', 'fontFamily': 'Inter'}}}%%
graph TD
  PreferenceMatchingStart[Start Preference Matching]
  PreferenceMatchingActivity1[1. Evaluate Adopter preferences]
  PreferenceMatchingActivity2[2. Match based on Sender listings]
  PreferenceMatchingEnd[Preference Matching Complete]

  PreferenceMatchingStart --> PreferenceMatchingActivity1
  PreferenceMatchingActivity1 --> PreferenceMatchingActivity2
  PreferenceMatchingActivity2 --> PreferenceMatchingEnd
```

- #Geographical-Proximity:
```mermaid
%%{init: { 'themeVariables': { 'fontSize': '10px', 'fontFamily': 'Inter'}}}%%
graph TD
  ProximityMatchingStart[Start Geographical Proximity Matching]
  ProximityMatchingActivity1[1. Evaluate Adopter and Sender locations]
  ProximityMatchingActivity2[2. Consider practicality in transportation]
  ProximityMatchingEnd[Geographical Proximity Matching Complete]

  ProximityMatchingStart --> ProximityMatchingActivity1
  ProximityMatchingActivity1 --> ProximityMatchingActivity2
  ProximityMatchingActivity2 --> ProximityMatchingEnd
```

- #Budget-Matching

```mermaid
graph TD
  BudgetMatchingStart[Start Budget Matching]
  BudgetMatchingActivity1[1. Evaluate Adopter budget preferences]
  BudgetMatchingActivity2[2. Consider Sender adoption fees]
  BudgetMatchingEnd[Budget Matching Complete]

  BudgetMatchingStart --> BudgetMatchingActivity1
  BudgetMatchingActivity1 --> BudgetMatchingActivity2
  BudgetMatchingActivity2 --> BudgetMatchingEnd
```

- #User-Feedback-and-Ratings
```mermaid
graph TD
  FeedbackMatchingStart[Start User Feedback and Ratings Matching]
  FeedbackMatchingActivity1[1. Consider Adopter and Sender ratings]
  FeedbackMatchingActivity2[2. Incorporate user feedback]
  FeedbackMatchingEnd[User Feedback and Ratings Matching Complete]

  FeedbackMatchingStart --> FeedbackMatchingActivity1
  FeedbackMatchingActivity1 --> FeedbackMatchingActivity2
  FeedbackMatchingActivity2 --> FeedbackMatchingEnd
```

- #Machine-Learning
```mermaid
graph TD
  MachineLearningStart[Start Machine Learning Matching]
  MachineLearningActivity1[1. Implement dynamic matching algorithms]
  MachineLearningActivity2[2. Adjust matches based on user behaviors]
  MachineLearningEnd[Machine Learning Matching Complete]

  MachineLearningStart --> MachineLearningActivity1
  MachineLearningActivity1 --> MachineLearningActivity2
  MachineLearningActivity2 --> MachineLearningEnd
```

- Sequence diagram for Actors usage
	- #Adopter
```mermaid
%%{init: { 'themeVariables': { 'fontSize': '6px', 'fontFamily': 'Inter'}}}%%
sequenceDiagram
  A->>Sys: List available dogs
  Sys->>Sys: Evaluate potential matches by considering factors
  Sys->>Sys: [Geographical, User Perference, Budget,Feedback, ML (optinal)]
  alt Matches found
    Sys->>A: Return suggested matches
    A->>A: Review suggested matches and dog listing
    A->>Sys: Select a potential match
    Sys->>S: Notification about potential match
    S->>Sys: Review potential match details
    S->>Sys: Initiate communication
  else No matches
    Sys->>A: Notify no matches found
  end
```

- #Sender
```mermaid
%%{init: { 'themeVariables': { 'fontSize': '10px', 'fontFamily': 'Inter'}}}%%
sequenceDiagram
  A->>Sys: Initiate adoption process
  A->>Sys: Listing Avaible Adopters 
  Sys->>Sys: Evaluate potential matches by considering factors
  Sys->>Sys: [Geographical, User Perference, Budget,Feedback, ML (optinal)]
  alt Matches found
    Sys->>A: Return suggested matches
    A->>A: Review suggested matches and preferences
    A->>Sys: Select a potential match
    Sys->>S: Notification about potential match
    S->>Sys: Review potential match details
    S->>Sys: Initiate communication
  else No matches
    Sys->>A: Notify no matches found
  end
```

#Review-and-Rating-Process 
```mermaid
%%{init: { 'themeVariables': { 'fontSize': '12px', 'fontFamily': 'Inter'}}}%%

sequenceDiagram
  participant User
  participant BusinessPartner
  participant Transaction
  participant Rating
  participant Comment

  User->>User: See own preferences
  User->>BusinessPartner: See partner's rating
  User->>BusinessPartner: See partner's comments
  User->>Transaction: Complete transaction
  BusinessPartner->>Transaction: Complete transaction
  User->>Rating: Give rating and comment
  User->>Comment: Give comment
  BusinessPartner->>Rating: Give rating and comment
  BusinessPartner->>Comment: Give comment
  Rating-->>User: Update user's preferences
  Rating-->>BusinessPartner: Update partner's preferences
  Comment-->>User: View comment from partner
  Comment-->>BusinessPartner: View comment from user

```

#Payment-and-Proposal-Process 
```mermaid
stateDiagram-v2
[*] --> Proposal


state Proposal {
  state  if_make_new_request <<choice>>
  state  on_request_pending <<choice>>
  [*] --> OfferingRequest
  OfferingRequest --> PENDING
  PENDING --> OFFERING_COMPLETED
  PENDING --> on_request_pending 
  on_request_pending --> OFFERING_CANCELLED : user cancelled
  on_request_pending --> OFFERING_COMPLETED : user accepted
  OFFERING_CANCELLED --> if_make_new_request 
  if_make_new_request --> OfferingRequest: reattempt
  if_make_new_request --> [*] : no reattempt
  OFFERING_COMPLETED --> PROCESSING : sent request
}

state Payment {
  state on_process_pending <<choice>>
  state if_problem_can_be_fixed <<choice>>
  [*] --> PROCESSING
  PROCESSING --> on_process_pending
  on_process_pending --> COMPLETED : transaction success
  on_process_pending --> FAILED : transaction failed
  FAILED --> ProblemAnalysis
  ProblemAnalysis --> if_problem_can_be_fixed 
  if_problem_can_be_fixed --> PROCESSING : problem can be fix
  if_problem_can_be_fixed --> [*] : problem can't be fix
  COMPLETED --> [*]
}
```
