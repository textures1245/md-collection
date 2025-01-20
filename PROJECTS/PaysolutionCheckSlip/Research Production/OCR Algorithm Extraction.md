## Core Algorithm
- Image Normaliazation
	- Image DPI
- Image Pre Processing -> Custom + Leptonica
	- Custom
		- Grayscale
		- Sharpen
		- Gaussian Blur
		- Otsu Threshold
		- Brightness 
		- Contrast
	- Leptonica
		- **Binarization**:
		    - Otsu's method for automatic thresholding
		- **Scale and Resolution**:
		    - Area mapping for scaling
		    - Linear interpolation
		- **Image Enhancement**:
		    - Contrast stretching
		    - Histogram equalization
		    - Background normalization
		    - Unsharp masking for edge enhancement
		- **Noise Reduction**:
		    - Rank order filters (median filtering)
		    - Gaussian smoothing
		- **Document-specific Processing**:
		    - Skew detection and correction using projection profiles
		    - Page segmentation
		    - Border removal
		    - Deskewing algorithms
	- `Input Image → Scale/Resolution Adjustment → Noise Reduction → Binarization -> Contrast Enhancement →  Final Output`

- OCR Processing -> อธิบายหลักการทำ OCR 
- Data Pattern Extraction -> Regex
- Data Post Processing -> fuzzy matching, data normalisation  
- Data Validation

## Key Takeaways
- Transaction Status -> 
	- PENDING
	- VALID
	- INVALID
	- BACK_RECIVER_ACC_NOT_MATCH
	- BANK_REQEUEST_FAILED -> ข้อมูลที่ส่งให้แบงค์ไม่ถูกต้อง ->(ข้อมูลหมดอายุ, เลข ref ไม่ตรง)
	- BANK_RESPOND_FAILED -> ข้อมูลที่แบงค์ตอบกลับไม่ใช้ข้อมูล slip (Generic Error Respond, Internal Error etc.)
- Error/Unexpected Handler -> แนวทางในการจัดการ error ไม่ให้ service ล่มเมื่อเกิดปัญหา
- Model Accuracy -> Testing with difference image ppc and parameters tunings   
- User Role
	- DASHBOARD,PACKAGE,USER_MANAGEMENT,SLIP_CHECKER
- Quota Calculating

## Pro
- **Efficiency**: The system automates the process of verifying transaction slips, reducing manual effort and increasing efficiency.
- **Accuracy**: By integrating with external APIs for validation, the system ensures high accuracy in transaction verification.
- **Scalability**: The architecture is designed to handle a large volume of transactions, making it scalable for businesses of different sizes.
- **User-Friendly**: The integration with LINE messaging API provides a user-friendly way to notify users about their transaction status.
- **Security**: The system ensures secure handling of transaction data and integrates with secure external APIs for validation.

## 3. Use Case

**Use Case: Automated Transaction Slip Verification for E-commerce Platform**

**Actors**:

- User (Customer)
- Merchant
- System (OCR Slip Verification System)
- External Bank API
- LINE Messaging API

**Preconditions**:

- The user has made a purchase and received a transaction slip.
- The merchant is registered on the e-commerce platform.

**Main Flow**:

1. **Upload Slip**: The user uploads the transaction slip image to the e-commerce platform.
2. **OCR Processing**: The system uses OCR technology to extract text from the slip image.
3. **Data Parsing**: The system parses the extracted text to identify key transaction details.
4. **Validation**: The system sends the parsed transaction details to the external bank API for validation.
5. **Status Update**: Based on the validation results, the system updates the transaction status in the database.
6. **Notification**: The system sends a notification to the user via LINE messaging API about the transaction status (e.g., "Transaction Verified", "Transaction Failed").
7. **Merchant Notification**: The system also notifies the merchant about the transaction status.

**Postconditions**:

- The user and merchant are informed about the transaction status.
- The transaction status is updated in the database.

## Usecases
- OnLineMerchantTokenRedeem
- OnAuthorize
- Image Preprocessing
- OCRScanner
- OnUpdateQuotaUsage
- CheckReceiverBankAccounts

