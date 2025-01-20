- หนึ่ง
	- # **Login**
	-  ติดปัญหา การ Login ผ่าน Google, Facebook, Line (Login ผ่านแต่ไม่ได้มีการ Set Account credential ใน cookies ทำให้เมื่อดึงข้อมูลออกใช้ในระบบ เกิด Error) ![[Pasted image 20240612140337.png]]
	- # **Customer Register**
		- เมื่อทำการ Register ด้วย Email ที่มีอยู่แล้วในระบบ ทำให้เกิดการ Error Duplicated ของ Email ในหลังบ้าน แต่ไม่มีการ Handling ในส่วนนี้ในหน้าบ้าน
		- ![[Pasted image 20240612140858.png]]
	- # Store Register
		- ติดปัญหา การ Register Store account  (เมื่อ Register ผ่านแต่ไม่ได้มีการ Set Account credential ใน cookies ทำให้เมื่อดึงข้อมูลออกใช้ในระบบ เกิด Error) 
```shell
{
  message: "Error 1062 (23000): Duplicate entry 'sirprak1245@gmail.com' for key 'Account_email_key'",
  result: null,
  status: 500,
  status_code: 500
}
```

# Reset Password
- เมื่อทำการกรอก Reset รหัสผ่านใหม่ ไม่เหมือนกัน จะทำให้เกิด Error แล้ว เด้งมาหน้า OTP
- เมื่อทำการ Login ด้วยรหัสผ่านใหม่ที่ทำการ Reset กลับขึ้นว่ารหัสผ่านไม่ตรงกัน


```merm
sequenceDiagram
    actor Merchant
    actor Customer
    participant LINE
    participant LINE Bot Check Slip
    participant PaysoCheckSlip

    alt ใช้บริการในบัญชีทางการของ LINE Official Account ของ Merchant
        Merchant->>LINE: เพิ่ม Bot ใน LINE Official Account
        Merchant->>LINE Bot Check Slip: ส่ง TOKEN QR ผ่าน LINE Official Account
    end

    alt ลูกค้าของ Merchant อัปโหลดรูปภาพ
        Customer->>LINE Official Account: ส่ง slip ในกลุ่มแชท
        LINE Official Account->>LINE Bot Check Slip: ส่ง slip ที่อัปโหลด
        LINE Bot Check Slip->>PaysoCheckSlip: ตรวจสอบ slip
        PaysoCheckSlip->>LINE Bot Check Slip: ผลการตรวจสอบ slip
        LINE Bot Check Slip->>Customer: ส่งกลับผลลัพธ์ผ่าน LINE Official Account
    end
```
