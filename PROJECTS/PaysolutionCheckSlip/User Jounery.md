[[PaysolutionCheckSlip Overview]]
1. **Line Group Registered**
	- **Requirement**
		1. Merchant owner or Sub-user can only registered the line group
		2. Merchant or Sub-user that had permission can generate token for registering line group

	- Usecase
		1. Merchant generate token for line group register
		2. Merchant create new Line group
		3. Merchant invite Payso Line Bot OA into the group
		4. Line Bot OA must check if sender had accessibility to perform this action (Merchant owner or Sub-user that had permission)
		5. Merchant send the token into group
		6. Line Bot OA Responded registered information

```merm
sequenceDiagram
    participant Merchant
    participant LineGroup
    participant PaysoLineBotOA as Payso Line Bot OA
    participant System as System

    Merchant->>System: Generate token for line group register
    System-->>Merchant: Return generated token

    Merchant->>LineGroup: Create new Line group
    Merchant->>LineGroup: Invite Payso Line Bot OA into the group
    Merchant->>LineGroup: Send the token into group

    LineGroup->>PaysoLineBotOA: Receive token
    PaysoLineBotOA->>PaysoLineBotOA: Check if sender had accessibility
    PaysoLineBotOA->>System: If had, then validate token and register line group
    System-->>PaysoLineBotOA: Return registered information

    PaysoLineBotOA-->>LineGroup: Respond with registered information
```

