#Knowlaged-Category 

- **Plain Text**: The simplest form of data, represented as plain text. Content-Type: `text/plain`.
    
- **HTML**: Hypertext Markup Language, often used for web pages. Content-Type: `text/html`.
    
- **XML**: XML, or Extensible Markup Language, is a versatile and widely-used format for structuring, storing, and transporting data. It is both human-readable and machine-readable `application/xml`.
	- **Configuration Data** 
	- **Message** 
	- **Document** 
	- **Metadata** set of information that describes and gives information about other data
- **JSON**: JavaScript Object Notation, a lightweight data interchange format that is easy for humans to read and write and easy for machines to parse and generate. Content-Type: `application/json`.
    
- **Form Data**: Data formatted as `application/x-www-form-urlencoded`, which is the default content type for submitting form data in an HTTP POST request.
    
- **Multipart/Form-Data**: Used for submitting files and form data together. This content type is used for file uploads. Content-Type: `multipart/form-data`.
    
- **Binary Data**: Raw binary data, which can include any binary file (images, videos, audio, etc.). Content-Type: `application/octet-stream` or a specific binary type like `image/png`, `audio/mpeg`, etc.
    
- **CSV**: Comma-Separated Values, often used for exporting and importing tabular data. Content-Type: `text/csv`.
    
- **YAML**: Yet Another Markup Language, a human-readable data serialization standard often used for configuration files. Content-Type: `application/x-yaml`.
    
- **Protocol Buffers**: A method developed by Google for serializing structured data, useful in developing programs to communicate with each other over a network. Content-Type: `application/x-protobuf`.
    
- **PDF**: Portable Document Format, used for documents. Content-Type: `application/pdf`.


## what is QR TAG 30 and 29 and what is. the different between them?
# **CHATGPT**
- QR Tag 30 และ QR Tag 29 เป็นส่วนหนึ่งของข้อบังคับของสหภาพยุโรป (EU) ที่เกี่ยวข้องกับใบแจ้งหนี้อิเล็กทรอนิกส์ (Electronic invoices) และเอกสารทางธุรกิจดิจิทัลอื่นๆ โดยเฉพาะในบริบทของการปฏิบัติตาม VAT (ภาษีมูลค่าเพิ่ม) แท็ก QR เหล่านี้ใช้ในการเข้ารหัสข้อมูลเฉพาะในรูปแบบที่เครื่องอ่านได้ ซึ่งโดยปกติจะอยู่ภายในโค้ด QR เพื่อปรับปรุงการตรวจสอบและการประมวลผลเอกสารดังกล่าว
### QR Tag 30

- **วัตถุประสงค์**:
	- QR Tag 30 ใช้สำหรับการส่งข้อมูลใบแจ้งหนี้แบบง่าย ๆ โดยการเข้ารหัสข้อมูลที่จำเป็นเพื่อความสอดคล้องกับภาษีมูลค่าเพิ่ม (VAT) ในรูปแบบที่สามารถอ่านและประมวลผลได้อย่างรวดเร็วโดยทั้งผู้รับและหน่วยงานด้านภาษี

- **เนื้อหา** รายละเอียดใบแจ้งหนี้พื้นฐาน:
    - หมายเลขใบแจ้งหนี้
    - วันที่ออกใบแจ้งหนี้
    - หมายเลข VAT ของผู้ขายและผู้ซื้อ
    - ยอดรวม
    - จำนวนภาษีมูลค่าเพิ่ม

### QR Tag 29

**วัตถุประสงค์**:
- QR Tag 29 ใช้สำหรับการส่งข้อมูลใบแจ้งหนี้ที่มีความครอบคลุมมากขึ้น โดยรวมถึงข้อมูลทั้งหมดใน QR Tag 30 และรายละเอียดเพิ่มเติมที่จำเป็นสำหรับใบแจ้งหนี้ที่ซับซ้อนมากขึ้น

**เนื้อหา**:
- ข้อมูลทั้งหมดที่รวมอยู่ใน QR Tag 30 และ:
    - รายละเอียดรายการ (คำอธิบาย, ปริมาณ, ราคาต่อหน่วย, และอัตราภาษีมูลค่าเพิ่มสำหรับแต่ละรายการ)
    - เงื่อนไขการชำระเงิน
    - เงื่อนไขการส่งมอบ
    - ข้อมูลอ้างอิงเพิ่มเติม (เช่น หมายเลขใบสั่งซื้อ)

### ความแตกต่างหลัก
1. **ระดับของรายละเอียด**:
    - **QR Tag 30**: เข้ารหัสข้อมูลใบแจ้งหนี้พื้นฐานที่เหมาะสมสำหรับใบแจ้งหนี้ที่ง่าย
    - **QR Tag 29**: เข้ารหัสรายละเอียดใบแจ้งหนี้ที่ครอบคลุม ซึ่งเหมาะสมสำหรับใบแจ้งหนี้ที่ซับซ้อนมากขึ้น
      
2. **กรณีการใช้งาน**:
    - **QR Tag 30**: Invoice ที่ต้องการข้อมูลที่จำเป็นเท่านั้นเพื่อความสอดคล้องกับ VAT
    - **QR Tag 29**: Invoice ที่มีรายละเอียดที่ต้องการข้อมูลการทำธุรกรรมที่สมบูรณ์ รวมถึงการเรียกเก็บเงินแบบรายการ
      
3. **การปฏิบัติตามข้อกำหนด**:
    - **QR Tag 30**: เพียงพอสำหรับการรายงานภาษีมูลค่าเพิ่มส่วนใหญ่
    - **QR Tag 29**: เพื่อให้แน่ใจว่าปฏิบัติตามข้อกำหนดอย่างเต็มที่สำหรับการทำธุรกรรมที่มีรายละเอียด ลดความเสี่ยงของความไม่ตรงกันระหว่างการตรวจสอบหรือเมื่อจำเป็นต้องใช้ข้อมูลที่ละเอียดขึ้นโดยหน่วยงานด้านภาษี
      
4. **ขนาดของข้อมูล**:
    - **QR Tag 30**: โค้ด QR ขนาดเล็กกว่าเนื่องจากมีข้อมูลที่เข้ารหัสน้อยกว่า
    - **QR Tag 29**: โค้ด QR ขนาดใหญ่กว่าเนื่องจากมีข้อมูลเพิ่มเติมที่เข้ารหัส

# Research
### การชำระเงิน/โอนเงินด้วย Thai QR Code ผ่านบริการพร้อมเพย์
TQRC หรือ Thai QR Code เป็นมาตรฐาน QR Code สำหรับการชำระเงินในประเทศไทย โดยพัฒนาขึ้นตามมาตรฐาน EMVCo เพื่อให้สอดคล้องกับบริบทและความต้องการของผู้ใช้งานในประเทศไทย

โดยมีรายละเอียดการแบ่ง Tag ID ดังนี้ - 
- ล าดับที่ 00-25 : เครือข่ายบัตรระหว่างประเทศ (International Card Schemes) 
- ล าดับที่ 26-28 : เครือข่ายบัตรในประเทศ (Local Card Schemes) 
- ล าดับที่ 29-31 : การชำระเงิน/ โอนเงินผ่านพร้อมเพย์ และ Payment Innovation 
- ล าดับที่ 32-51 : สำรองสำหรับ Additional Payment Networks และกรณีอื่น ๆ (ถ้ามี) 
- ล าดับที่ 52-64 : ข้อมูลเพิ่มเติมอื่นๆ เพื่อใช้ประกอบการชำระเงิน/ โอนเงิน ซึ่งใช้ร่วมกัน ระหว่าง International / Local Card Schemes และพร้อมเพย์ เช่น มูลค่า ประเภทร้านค้า รหัสอ้างอิง
### ความแตกต่างหลัก:
1. **Tag 29**: ใช้สำหรับการโอนเงินของบุคคลทั่วไป
2. **Tag 30**: ใช้สำหรับการรับชำระเงินของผู้ประกอบการที่ลงทะเบียนด้วย Biller ID / Merchant ID

- **QR Tag 29**:
    - ใช้สำหรับการโอนเงินผ่านบริการพร้อมเพย์ โดยรองรับการลงทะเบียนด้วยหมายเลขโทรศัพท์มือถือ, เลขประจำตัวประชาชน, เลขทะเบียนนิติบุคคล, เลขประจำตัวผู้เสียภาษี, หมายเลข e-Wallet และเลขที่บัญชีเงินฝากธนาคาร
    - เหมาะสำหรับบุคคลทั่วไปและองค์กรที่ต้องการโอนเงินหรือรับเงินผ่านช่องทางที่หลากหลาย
		- หมายเลขโทรศัพท์มือถือ
	    - เลขประจำตัวประชาชน
	    - เลขทะเบียนนิติบุคคล
	    - เลขประจำตัวผู้เสียภาษี
	    - หมายเลข e-Wallet
	    - เลขที่บัญชีเงินฝากธนาคาร
- **QR Tag 30**:
    - ใช้สำหรับผู้ประกอบการที่ลงทะเบียนด้วย Biller ID / Merchant ID ของร้านค้า ซึ่งใช้โครงสร้างพื้นฐานการชำระเงินของบริการพร้อมเพย์ประเภท Bill Payment
    - การลงทะเบียนด้วย Biller ID มีพื้นฐานจากการใช้เลขทะเบียนนิติบุคคลหรือเลขประจำตัวผู้เสียภาษีของร้านค้า หรือเลขของผู้ให้บริการที่เป็นตัวแทนในการรับชำระเงิน (Bill Aggregator)
    - เหมาะสำหรับผู้ประกอบการที่ต้องการรับชำระเงินค่าสินค้าและบริการผ่านช่องทางที่ปลอดภัยและมีการตรวจสอบจากธนาคารแห่งประเทศไทย
		- เลขทะเบียนนิติบุคคล
	    - เลขประจำตัวผู้เสียภาษีของร้านค้า
	    - หรือเลขของผู้ให้บริการที่เป็นตัวแทนในการรับชำระเงิน (Bill Aggregator) ที่ออกให้ร้านค้า

    ทั้งนี้ ผู้ที่ให้บริการรับชำระเงินด้วย Thai QR Code แก่ร้านค้าต้องได้รับใบอนุญาตเป็นผู้ให้บริการรับชำระเงินแทน ภายใต้การกำกับของธนาคารแห่งประเทศไทย และการดำเนินธุรกิจต้องเป็นไปตามประกาศธนาคารแห่งประเทศไทยเรื่องหลักเกณฑ์ทั่วไปในการกำกับดูแลการประกอบธุรกิจบริการการชำระเงินภายใต้การกำกับ โดยก่อนเริ่มให้บริการผู้ให้บริการมีหน้าที่ลงทะเบียน Biller ID / Merchant ID ให้กับร้านค้า และต้องมีกระบวนการตรวจสอบให้มั่นใจว่าร้านค้าที่ลงทะเบียนเป็นผู้ประกอบการที่ใช้ Thai QR Code เพื่อการรับชำระเงินค่าสินค้าและบริการที่ถูกต้องตามหลักเกณฑ์และกฎหมายที่เกี่ยวข้อง
    
![[Pasted image 20240626015444.png]]
    

    
