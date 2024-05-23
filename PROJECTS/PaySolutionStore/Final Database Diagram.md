[[PaySolutionStore Note Detail]]
## ER Diagram

```merm
erDiagram
    ACCOUNT ||--o{ FILE : "uploads"
    ACCOUNT ||--|| ADMINPERMISSION : "manages : if role is ADMIN"
    ACCOUNT ||--o{ LOG : "manages : if role is ADMIN"
    ACCOUNT ||--o{ PRODUCT : "manages : if role is STORE"
    ACCOUNT ||--o{ PRODUCTCATEGORY : "manages : if role is STORE"
    ACCOUNT ||--o{ BANK : "manages : if role is STORE"
    ACCOUNT ||--o{ ORDER : "places : if role is CUSTOMER"
    ORDER ||--o{ FILE : "uploads"
    ORDER ||--|{ PRODUCT : "contains"
    ORDER }|--|{ BANK : "processs"
    BANK ||--o{ FILE : "uploads"
    ORDER ||--o{ ORDER_PRODUCT : includes
    PRODUCT ||--o{ ORDER_PRODUCT : is_part_o
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
        int stock
        varchar detail
        bool status
        int categoryId FK
        int storeId FK
        int productId FK
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
        float sumPrice
        date timestamp
        enum state "ENUM(PENDING, PREPARED, SEND, SUCCEED, REJECTED)"
        varchar deliverytType "nullable if Order.state != SEND"
        varchar parcelNumber "nullable if Order.state != SEND"
        date sentDate "nullable if Order.state != SEND"
        int customerId FK
        int storeId FK
        int bankId FK
    }

    ORDER_PRODUCT {
        int orderId FK
        int productId FK
        int quantity
    }
    
    BANK {
        int id PK
        varchar name
        varchar accNumber
        varchar accName
        varchar status
        int storeId FK
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
        enum entityType "ENTITY_TYPE(ACCOUNT, ORDER, PRODUCT, BANK)"
        int entityId FK
    }

    LOG {
        int id 
        String fullName
        String menuRequest
        String actionRequest
        timestamp createdAt
        timestamp updatedAt 
        int accountId FK
    }
```

## Class Diagram

```merm
classDiagram
    class Account {
        int id
        String name
        String password
        String phone
        String location
        String email
        timestamp createdAt
        timestamp updatedAt 
        Role role
        boolean status
        +uploadFile(File file)
    }

    class Admin {
        int id
        int accountId
        int permissionId
        timestamp createdAt
        timestamp updatedAt 
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
        timestamp createdAt
        timestamp updatedAt 
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
        timestamp createdAt
        timestamp updatedAt 
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
        timestamp createdAt
        timestamp updatedAt 
        +uploadFile(File file)
    }

    class ProductCategory {
        int id
        String name
        boolean status
        String code
        String detail
        timestamp createdAt
        timestamp updatedAt 
    }

    class Order {
        int id
        String orderId
        float totalAmount
        String topic
        float price
        Date timestamp
        enum state
        OrderProduct[] orderProducts
        int customerId
        int storeId
        int bankId
        timestamp createdAt
        timestamp updatedAt 
        +uploadFile(File file)
    }

    class Bank {
        int id
        String name
        String details
        timestamp createdAt
        timestamp updatedAt 
    }

    class AdminPermission {
        int id
        String[] menuPermission
        timestamp createdAt
        timestamp updatedAt 
    }

    class File {
        int id
        String name
        String pathUrl
        String type
        EntityType entityType
        int entityId
        timestamp createdAt
        timestamp updatedAt 
    }

	class Log {
        int id 
        String fullName
        String menuRequest
        String actionRequest
        timestamp createdAt
        timestamp updatedAt 
    }

	class OrderProduct {
		<<interface>>
		Product product
		int quantity
		float sumPrice 
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

	class OrderState {
        <<enumeration>>
        PENDING
        PREPARED
        SEND
        SUCCEED
        FAILED
    }

    Account <|-- Admin : inherits
    Account <|-- Store : inherits
    Account <|-- Customer : inherits
    Account -->	 Role : references
    
    Admin --* AdminPermission : manages
    Admin --o File : uploads
    Admin --> Log : manages
    
    Store --o File : uploads
    Store --> Product : manages
    Store --> ProductCategory : manages
    Store --> Bank : manages
    Store --> Order : handles

    File --> EntityType : references
    
    Customer --o Order : places
    
    Order --o File : uploads
    Order --> Product : has
    Product --o OrderProduct : contains
    Order --* OrderProduct : has
    Order --> OrderState : references
    Order <..>	 Bank : uses
    
    Product --o File : uploads
    Product --* ProductCategory : categorized into
```
