#PaysolutionCheckSlip [[PaysolutionCheckSlip Overview]]
1. Bugs
	-  Can't check received account number if Slip is type of **Promptpay Topup**
		- **Cause :** Promptpay-Topup id is EWALLETID, so it difference from the original account id that tied up with. 
		- **Fix**: 
		1. User retrieved ref Id from **Promptpay Topup** QR Receiver 


- Feature Implemented
	- Room Token
		- Format: PAYSO:MID:CREATE_AT -> PAYSO_ROOM_TOKEN:13:2024-20-27
