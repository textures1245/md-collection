## Contents
 #PaySolutonShop-DB-Design-Solution-1
 #PaySolutonShop-DB-Design-Solution-2

## Using Account to store all Actors (Admin, Customer and Store) 
#PaySolutonShop-DB-Design-Solution-1
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
#PaySolutonShop-DB-Design-Solution-2
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

## Solution Summary
### Diagram 1: Single `Account` Table with Role-based Differentiation

#### Pros:

1. **Simplicity**:
    
    - All user-related data is stored in a single table, making it simpler to manage and query.
    - Easier to enforce unique constraints like email uniqueness across all roles.
2. **Reduced Redundancy**:
    
    - Eliminates the need to join multiple tables to get user-related data.
    - Less duplication of common fields (e.g., name, email) across different tables.
3. **Flexibility**:
    
    - Adding a new role or modifying existing roles is easier since it involves updating a single table.

#### Cons:

1. **Complexity in Role-specific Fields**:
    
    - The table might have many nullable fields that are only relevant for certain roles, leading to sparse data.
    - Complex logic in application code to handle role-specific behavior and data validation.
2. **Scalability Concerns**:
    
    - As the number of users grows, the single table might become a bottleneck.
    - Increased potential for locking issues during concurrent transactions affecting the same table.
3. **Potential for Data Integrity Issues**:
    
    - Enforcing role-specific relationships (e.g., only a store can manage products) requires complex constraints or application logic.

### Diagram 2: `Account` Table with Separate `Admin`, `Store`, and `Customer` Tables

#### Pros:

1. **Data Organization**:
    
    - Clear separation of role-specific data into separate tables, leading to better data organization and clarity.
    - Role-specific fields are only present in relevant tables, reducing the number of nullable fields.
2. **Data Integrity and Validation**:
    
    - Easier to enforce role-specific constraints and relationships using foreign keys.
    - Application logic is cleaner as each role's behavior is encapsulated in its respective table.
3. **Scalability**:
    
    - Distributing data across multiple tables can improve performance and reduce locking issues.
    - Easier to scale specific role tables independently if needed.

#### Cons:

1. **Complexity in Queries**:
    
    - Queries involving user data might require joins across multiple tables, complicating query logic and potentially impacting performance.
2. **Maintenance Overhead**:
    
    - Adding a new role or modifying existing roles involves changes in multiple tables.
    - Potentially more complex data migration and schema evolution tasks.
3. **Potential Redundancy**:
    
    - Some level of data redundancy might exist due to the presence of common fields in multiple role-specific tables.

### Recommendation

#### Best Choice for Simplicity

**Diagram 1** is the best choice for simplicity because:

- It minimizes the number of tables.
- Simplifies user management by consolidating all roles into a single table.
- Eases query complexity by avoiding joins across multiple tables.

While it has some drawbacks, such as potential sparse data and complexity in handling role-specific behavior, it offers a straightforward and manageable schema that can be easier to implement and maintain initially. This makes it suitable for simpler applications or projects where ease of use and development speed are prioritized over scalability and strict data integrity.
#### Best Choice for Production

**Diagram 2** is generally more suitable for production environments, especially for a complex system with distinct role-specific behavior and data requirements.

### Rationale:

- **Data Integrity**: Clear separation of role-specific data helps maintain data integrity and enforce role-specific constraints.
- **Scalability**: Distributing data across multiple tables reduces the likelihood of bottlenecks and improves scalability.
- **Maintenance**: Although initially more complex, the separation of concerns makes it easier to maintain and extend the system as it grows.

### Summary

- **Diagram 1** easy for querying and simplifies user management by consolidating all roles into a single table but could lead to complexity and scalability issues as the system grows.

- **Diagram 2** provides better organization, data integrity, and scalability, making it more robust and maintainable for production use, but may complex for querying and maintenance overhead 
