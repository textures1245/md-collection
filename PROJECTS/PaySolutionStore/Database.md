[[PaySolutionStore Note Detail]]
## Master Table (Following by the Ui)
- Account
- Admin
- Store
- Customer
- Product
- Transaction
- BankPayment

## ลายละเอียดแต่ละ Master Table

- Account
	- name
	- location
	- email
	- phone
	- password

- Admin 
	- images 
	- name
	- phone
	- roleAccessibility 
	- email
	- password
	- activeStatus

- Store
	- name
	- storeName
	- storeLocation
	- roleAccessibility
	- email
	- email
	- password
	- activeStatus

- Customer 
	- name
	- location
	- phone
	- emaildddd
	- password
	- activeStatus

## Detail
From this diagram, this diagram is about shopping store where it have 3 important actor roles following by these
1. Admin
2. Store
3. Customer
and all of these roles will be associated with other tables. where each of role had diffrents action but store as same table called Account
which had `accountRole` for indidcated each role. each roles had diffrents action following by these
Admin
1. Admin can view, create, delete and update account on every role (Customer, Store and Admin)
2. Admin can CRUD AdminPermisson
3. Admin can upload File
Store

1. Store can be registered by Admin or can be registered by their own account
2. Store can CRUD Product
3. Store can CRUD ProductCategory
4. Store can CRUD Bank
5. Store can take action to Order for operated Order's transaction to be success or failed (reject the Order)

Customer
1. Customer can view and buy (action) the Product
2. Customer can create Order that will be link to the Product that the Customer bought

And below these table are `use-cases` which will be associated with actor tables

Product
1. Product can Link to ProductCategory By id 
2. Product can have File

ProductCategory
1. ProductCategory will be link to Product (as FK key)
2. This table will be product group for querying
   
Bank
1. Bank will linked to Order for using in customer transaction

Order
1. Order can be linked with Products
2. Order will linked to Bank, so the Customer can be selected the payment during the action
   
I hope you understand what i'm trying to tell you. The task is i would like you anaysis this. and write `ER Database Diagram` and `Class Diagram`in mermaid.js  with all relationship associated. 

