## Using Account to store all Actors (Admin, Customer and Store)  
### ER Diagram
- v1
```merm
erDiagram
    ACCOUNT ||--o{ FILE : "uploads"
    ACCOUNT ||--|| ADMINPERMISSION : "manages : if role is ADMIN"
    ACCOUNT ||--o{ PRODUCT : "manages : if role is STORE"
    ACCOUNT ||--o{ PRODUCTCATEGORY : "manages : if role is STORE"
    ACCOUNT ||--o{ BANK : "manages : if role is STORE"
    ACCOUNT ||--o{ ORDER : "places : if role is CUSTOMER"
    ORDER ||--o{ FILE : "uploads"
    ORDER ||--|{ PRODUCT : "contains"
    ORDER }|--|{ BANK : "uses"
    PRODUCT ||--o{ FILE : "uploads"
    PRODUCT ||--|| PRODUCTCATEGORY : "categorized into"
    
    ACCOUNT {
        int id PK
        varchar name
        varchar password
        varchar phone
        varchar location
        varchar email "UNIQUE"
        enum role "ROLE(ADMIN, STORE, CUSTOMER)"
        boolean status "DEFAULT(TRUE)"
        varchar storeName "nullable if not STORE"
        varchar storeLocation "nullable if not STORE"
        int permissionId "nullable if not ADMIN"
    }
    
    PRODUCT {
        int id PK
        varchar name
        decimal price
        int categoryId FK
        int ownerId FK
    }
    
    PRODUCTCATEGORY {
        int id PK
        varchar name
        bool status
        varchar code
        varchar detail
    }
    
    ORDER {
        int id PK
        varchar orderId "UNIQUE"
        decimal totalAmount
        varchar topic
        float price
        date timestamp
        bool status
        int customerId FK
        int storeId FK
        int bankId FK
    }
    
    BANK {
        int id PK
        varchar name
        varchar details
    }
    
    ADMINPERMISSION {
        int id PK
        varchar[] menuPermission
    }
    
    FILE {
        int id PK
        varchar name
        varchar pathUrl
        varchar type
        enum entityType "ENTITY_TYPE(ACCOUNT, ORDER, PRODUCT)"
        int entityId FK
    }
```
### Class Diagram
- v1 (Based on ER Diagram v1)
```merm
classDiagram
    class Account {
        int id
        String name
        String password
        String phone
        String location
        String email
        Role role
        boolean status
        String storeName
        String storeLocation
        int permissionId
        +uploadFile(File file)
        +managePermission(AdminPermission permission) : if role is ADMIN
        +manageProduct(Product product) : if role is STORE
        +manageProductCategory(ProductCategory category) : if role is STORE
        +manageBank(Bank bank) : if role is STORE
        +placeOrder(Order order) : if role is CUSTOMER
    }

    class Product {
        int id
        String name
        float price
        int categoryId
        int ownerId
        +uploadFile(File file)
    }

    class ProductCategory {
        int id
        String name
        boolean status
        String code
        String detail
    }

    class Order {
        int id
        String orderId
        float totalAmount
        String topic
        float price
        Date timestamp
        boolean status
        int customerId
        int storeId
        int bankId
        +uploadFile(File file)
    }

    class Bank {
        int id
        String name
        String details
    }

    class AdminPermission {
        int id
        String[] menuPermission
    }

    class File {
        int id
        String name
        String pathUrl
        String type
        EntityType entityType
        int entityId
    }

    class Role {
        <<enumeration>>
        ADMIN
        STORE
        CUSTOMER
    }

    class EntityType {
        <<enumeration>>
        ACCOUNT
        ORDER
        PRODUCT
    }

    Account --o File : "uploads"
    Account --> AdminPermission : "manages -> if role is ADMIN"
    Account --o Product : "manages -> if role is STORE"
    Account --o ProductCategory : "manages -> if role is STORE"
    Account --o Bank : "manages -> if role is STORE"
    Account --o Order : "places -> if role is CUSTOMER"

    Order --o File : "uploads"
    Order --|> Product : "contains"
    Order <|--|> Bank : "uses"

    Product --o File : "uploads"
    Product --|> ProductCategory : "categorized into"

```



##  Using  `Account` as Master table And Extracted actors as child table to Admin, Customer and Store
### ER Diagram
- v1
```merm
erDiagram
    ADMIN ||--|| ACCOUNT : inherits
    ADMIN ||--|| ADMINPERMISSION : "เรียกใช้/จัดการ"
    ADMIN ||--|| FILE : "uploads"
    STORE ||--|| ACCOUNT : inherits
    STORE ||--|| FILE : "uploads"
    STORE ||--|{ PRODUCT : "จัดการ"
    STORE ||--|{ PRODUCTCATEGORY : "จัดการ"
    STORE ||--|{ BANK : "จัดการ"
    STORE ||--|{ ORDER : "ดำเนินการ"
    CUSTOMER ||--|| ACCOUNT : inherits
    CUSTOMER ||--|{ ORDER : "ทำรายการ"
    ORDER ||--|| FILE : "uploads"
    ORDER ||--|{ PRODUCT : "มี"
    ORDER }|--|{ BANK : "เรียกใช้"
    PRODUCT ||--|| FILE : "uploads"
    PRODUCT ||--|| PRODUCTCATEGORY : "จัดหมวดหมู่"
    
    ACCOUNT {
        int id PK
        varchar name
        varchar password
        varchar phone
        varchar location
        varchar email
        enum role "ROLE(ADMIN, STORE, CUSTOMER)"
        boolean status "DEFAULT(TRUE)"
    }
    
    STORE {
        int id PK
        varchar storeName
	    varchar storeLocation
        int accountId FK
    }
    ADMIN {
        int id PK
        int accountId FK
        int permissionId FK "ADMIN_PEREMISSON"
    }
    
    
    CUSTOMER {
        int id PK
        int accountId FK
    }
    
    PRODUCT {
        int id PK
        varchar name
        decimal price
        int categoryId FK
        int ownerId FK
    }
    
    PRODUCTCATEGORY {
        int id PK
        varchar name
        bool status
        varchar code
        varchar detail
    }
    
    ORDER {
        int id PK
        varchar orderId
        decimal totalAmount
        varchar topic
        float price
        date timestamp
        bool status
        int customerId FK "ข้อมูลลูกค้า"
        int storeId FK "ข้อมูลร้านค้า"
        int bankId FK "PAYMENT"   
         }
    
    BANK {
        int id PK
        varchar name
        varchar details
    }
    
    ADMINPERMISSION {
        int id PK
        varchar[] menuPermission
    }
    
    FILE {
        int id PK
        varchar name
        varchar pathUrl
        varchar type
        int adminFk1Id FK 
        int storeFk2id FK
        int slipOrderFk3Id FK
        int product4Id FK
    }
```
- v2
```merm
erDiagram
    ADMIN ||--|| ACCOUNT : inherits
    ADMIN ||--|| ADMINPERMISSION : "manages"
    ADMIN ||--o{ FILE : "uploads"
    STORE ||--|| ACCOUNT : inherits
    STORE ||--o{ FILE : "uploads"
    STORE ||--|{ PRODUCT : "manages"
    STORE ||--|{ PRODUCTCATEGORY : "manages"
    STORE ||--|{ BANK : "manages"
    STORE ||--|{ ORDER : "handles"
    CUSTOMER ||--|| ACCOUNT : inherits
    CUSTOMER ||--o{ ORDER : "places"
    ORDER ||--o{ FILE : "uploads"
    ORDER ||--|{ PRODUCT : "contains"
    ORDER }|--|{ BANK : "uses"
    PRODUCT ||--o{ FILE : "uploads"
    PRODUCT ||--|| PRODUCTCATEGORY : "categorized into"
    
    ACCOUNT {
        int id PK
        varchar name
        varchar password
        varchar phone
        varchar location
        varchar email "UNIQUE"
        enum role "ROLE(ADMIN, STORE, CUSTOMER)"
        boolean status "DEFAULT(TRUE)"
    }
    
    STORE {
        int id PK
        varchar storeName
        varchar storeLocation
        int accountId FK
    }
    ADMIN {
        int id PK
        int accountId FK
        int permissionId FK
    }
    
    CUSTOMER {
        int id PK
        int accountId FK
    }
    
    PRODUCT {
        int id PK
        varchar name
        decimal price
        int categoryId FK
        int ownerId FK
    }
    
    PRODUCTCATEGORY {
        int id PK
        varchar name
        bool status
        varchar code
        varchar detail
    }
    
    ORDER {
        int id PK
        varchar orderId "UNIQUE"
        decimal totalAmount
        varchar topic
        float price
        date timestamp
        bool status
        int customerId FK
        int storeId FK
        int bankId FK
    }
    
    BANK {
        int id PK
        varchar name
        varchar details
    }
    
    ADMINPERMISSION {
        int id PK
        varchar[] menuPermission
    }
    
    FILE {
        int id PK
        varchar name
        varchar pathUrl
        varchar type
        enum entityType "ENTITY(ADMIN, STORE, ORDER, PRODUCT)"
        int entityId FK
    }
```


### Class Diagram
- v1 : (Base from ER v2)
```merm
classDiagram
    class Account {
        int id
        String name
        String password
        String phone
        String location
        String email
        Role role
        boolean status
    }

    class Admin {
        int id
        int accountId
        int permissionId
        +uploadFile(File file)
    }

    class Store {
        int id
        String storeName
        String storeLocation
        int accountId
        +manageProduct(Product product)
        +manageProductCategory(ProductCategory category)
        +manageBank(Bank bank)
        +handleOrder(Order order)
        +uploadFile(File file)
    }

    class Customer {
        int id
        int accountId
        +placeOrder(Order order)
    }

    class Product {
        int id
        String name
        float price
        int categoryId
        int ownerId
        +uploadFile(File file)
    }

    class ProductCategory {
        int id
        String name
        boolean status
        String code
        String detail
    }

    class Order {
        int id
        String orderId
        float totalAmount
        String topic
        float price
        Date timestamp
        boolean status
        int customerId
        int storeId
        int bankId
        +uploadFile(File file)
    }

    class Bank {
        int id
        String name
        String details
    }

    class AdminPermission {
        int id
        String[] menuPermission
    }

    class File {
        int id
        String name
        String pathUrl
        String type
        EntityType entityType
        int entityId
    }

    class Role {
        <<enumeration>>
        ADMIN
        STORE
        CUSTOMER
    }

    class EntityType {
        <<enumeration>>
        ADMIN
        STORE
        ORDER
        PRODUCT
    }

    Account --> Role : contains

    Admin --* Account : Composition
    Store --* Account : Composition
    Customer --* Account : Composition

    Admin "1" --> "1" AdminPermission : manages
    Admin "1" --> "*" File : uploads

    Store  "1" --> "*" Product : manages
    Store "1" --> "*" ProductCategory : manages
    Store "1" --> "*" Bank : manages
    Store "1" --> "*" Order : handles
    Store "1" --> "*" File : uploads

    Customer "1" --> "*" Order : places

    Order "1" --> "*" Product : contains
    Order "1" --> "*" File : uploads
    Order "1" --> "1" Bank : uses

    Product "1" --> "*" File : uploads
    Product "1" --> "1" ProductCategory : categorized into
    File  -->  EntityType : contains
```
v2 : (Base from ER v2-refine)
```merm
classDiagram
    class Account {
        int id
        String name
        String password
        String phone
        String location
        String email
        Role role
        boolean status
        +uploadFile(File file)
    }

    class Admin {
        int id
        int accountId
        int permissionId
        +createAccount(Account account)
        +viewAccount(int accountId)
        +updateAccount(Account account)
        +deleteAccount(int accountId)
        +managePermission(AdminPermission permission)
        +uploadFile(File file)
    }

    class Store {
        int id
        String storeName
        String storeLocation
        int accountId
        +registerAccount(Account account)
        +manageProduct(Product product)
        +manageProductCategory(ProductCategory category)
        +manageBank(Bank bank)
        +handleOrder(Order order)
        +uploadFile(File file)
    }

    class Customer {
        int id
        int accountId
        +viewProduct(int productId)
        +buyProduct(Product product)
        +placeOrder(Order order)
    }

    class Product {
        int id
        String name
        float price
        int categoryId
        int ownerId
        +uploadFile(File file)
    }

    class ProductCategory {
        int id
        String name
        boolean status
        String code
        String detail
    }

    class Order {
        int id
        String orderId
        float totalAmount
        String topic
        float price
        Date timestamp
        boolean status
        int customerId
        int storeId
        int bankId
        +uploadFile(File file)
    }

    class Bank {
        int id
        String name
        String details
    }

    class AdminPermission {
        int id
        String[] menuPermission
    }

    class File {
        int id
        String name
        String pathUrl
        String type
        EntityType entityType
        int entityId
    }

    class Role {
        <<enumeration>>
        ADMIN
        STORE
        CUSTOMER
    }

    class EntityType {
        <<enumeration>>
        ADMIN
        STORE
        ORDER
        PRODUCT
    }

    Account <|-- Admin : inherits
    Account <|-- Store : inherits
    Account <|-- Customer : inherits
    Account -->	 Role : references
    
    Admin --* AdminPermission : manages
    Admin --o File : uploads
    
    Store --o File : uploads
    Store --> Product : manages
    Store --> ProductCategory : manages
    Store --> Bank : manages
    Store --> Order : handles

    File --> EntityType : references
    
    Customer --o Order : places
    
    Order --o File : uploads
    Order --> Product : contains
    Order <..>	 Bank : uses
    
    Product --o File : uploads
    Product --* ProductCategory : categorized into
```



Diagram : 1 -> Using Account to store all Actors (Admin, Customer and Store) 
```
erDiagram
    ACCOUNT ||--o{ FILE : "uploads"
    ACCOUNT ||--|| ADMINPERMISSION : "manages : if role is ADMIN"
    ACCOUNT ||--o{ PRODUCT : "manages : if role is STORE"
    ACCOUNT ||--o{ PRODUCTCATEGORY : "manages : if role is STORE"
    ACCOUNT ||--o{ BANK : "manages : if role is STORE"
    ACCOUNT ||--o{ ORDER : "places : if role is CUSTOMER"
    ORDER ||--o{ FILE : "uploads"
    ORDER ||--|{ PRODUCT : "contains"
    ORDER }|--|{ BANK : "uses"
    PRODUCT ||--o{ FILE : "uploads"
    PRODUCT ||--|| PRODUCTCATEGORY : "categorized into"
    
    ACCOUNT {
        int id PK
        varchar name
        varchar password
        varchar phone
        varchar location
        varchar email "UNIQUE"
        enum role "ROLE(ADMIN, STORE, CUSTOMER)"
        boolean status "DEFAULT(TRUE)"
        varchar storeName "nullable if not STORE"
        varchar storeLocation "nullable if not STORE"
        int permissionId "nullable if not ADMIN"
    }
    
    PRODUCT {
        int id PK
        varchar name
        decimal price
        int categoryId FK
        int ownerId FK
    }
    
    PRODUCTCATEGORY {
        int id PK
        varchar name
        bool status
        varchar code
        varchar detail
    }
    
    ORDER {
        int id PK
        varchar orderId "UNIQUE"
        decimal totalAmount
        varchar topic
        float price
        date timestamp
        bool status
        int customerId FK
        int storeId FK
        int bankId FK
    }
    
    BANK {
        int id PK
        varchar name
        varchar details
    }
    
    ADMINPERMISSION {
        int id PK
        varchar[] menuPermission
    }
    
    FILE {
        int id PK
        varchar name
        varchar pathUrl
        varchar type
        enum entityType "ENTITY_TYPE(ACCOUNT, ORDER, PRODUCT)"
        int entityId FK
    }
```

Diagram 2 -> Using  `Account` as Master table And Extracted actors as child table to Admin, Customer and Store

```
erDiagram
    ADMIN ||--|| ACCOUNT : inherits
    ADMIN ||--|| ADMINPERMISSION : "manages"
    ADMIN ||--o{ FILE : "uploads"
    STORE ||--|| ACCOUNT : inherits
    STORE ||--o{ FILE : "uploads"
    STORE ||--|{ PRODUCT : "manages"
    STORE ||--|{ PRODUCTCATEGORY : "manages"
    STORE ||--|{ BANK : "manages"
    STORE ||--|{ ORDER : "handles"
    CUSTOMER ||--|| ACCOUNT : inherits
    CUSTOMER ||--o{ ORDER : "places"
    ORDER ||--o{ FILE : "uploads"
    ORDER ||--|{ PRODUCT : "contains"
    ORDER }|--|{ BANK : "uses"
    PRODUCT ||--o{ FILE : "uploads"
    PRODUCT ||--|| PRODUCTCATEGORY : "categorized into"
    
    ACCOUNT {
        int id PK
        varchar name
        varchar password
        varchar phone
        varchar location
        varchar email "UNIQUE"
        enum role "ROLE(ADMIN, STORE, CUSTOMER)"
        boolean status "DEFAULT(TRUE)"
    }
    
    STORE {
        int id PK
        varchar storeName
        varchar storeLocation
        int accountId FK
    }
    ADMIN {
        int id PK
        int accountId FK
        int permissionId FK
    }
    
    CUSTOMER {
        int id PK
        int accountId FK
    }
    
    PRODUCT {
        int id PK
        varchar name
        decimal price
        int categoryId FK
        int ownerId FK
    }
    
    PRODUCTCATEGORY {
        int id PK
        varchar name
        bool status
        varchar code
        varchar detail
    }
    
    ORDER {
        int id PK
        varchar orderId "UNIQUE"
        decimal totalAmount
        varchar topic
        float price
        date timestamp
        bool status
        int customerId FK
        int storeId FK
        int bankId FK
    }
    
    BANK {
        int id PK
        varchar name
        varchar details
    }
    
    ADMINPERMISSION {
        int id PK
        varchar[] menuPermission
    }
    
    FILE {
        int id PK
        varchar name
        varchar pathUrl
        varchar type
        enum entityType "ENTITY(ADMIN, STORE, ORDER, PRODUCT)"
        int entityId FK
    }
```

hey do you still remember 
