[[BlogDuaaeeg Overview]]

References
- [[App Requirement]]

**Contents**
#BlogDuaaeeg-SRS 
#BlogDuaaeeg-ERD 

- ## Software Requirements Specification (SRS)
	#BlogDuaaeeg-SRS 
	Based from [[App Requirement]] we had gathered. We can create document outlines for Application roadmap which can be describe as the following
	
	### **Introduction**:
	- Purpose: Describe the purpose of the SRS document and the intended audience.
	- Scope: Define the scope of the blogger website project and its key features.
	- Definitions and abbreviations: List any important terms or abbreviations used in the document.
	
	### **Overall Description**:
	- Product perspective: Provide an overview of the blogger website application and its context.
	- Product features: List the key features of the application, such as user registration, content creation, publishing, distribution, engagement, and discovery.
	- User characteristics: Describe the target users of the application (e.g., bloggers, writers, readers).
	- Operating environment: Specify the operating systems, browsers, and devices supported by the application.
	- Design and implementation constraints: Mention any known constraints or limitations (e.g., technology stack, third-party integrations).
	
	### **Specific Requirements**:
	- Functional requirements: Describe in detail the functional requirements for each feature or component of the application, such as user management, content management, content distribution, user engagement, content discovery, analytics, and additional features.
	- Non-functional requirements: Specify the non-functional requirements, such as usability, reliability, performance, security, maintainability, and portability.
	
	### **Interface Requirements**:
	- User interfaces: Describe the user interfaces, including the main screens, navigation, and interaction flow.
	- Hardware interfaces: Specify any hardware interfaces required (e.g., file upload, camera access).
	- Software interfaces: List any software interfaces or third-party integrations (e.g., social media APIs, analytics tools).
	- Communication interfaces: Define the communication interfaces, such as APIs or web services.
	
	### **Other Non-functional Requirements**:
	- Performance requirements: Specify any performance requirements, such as response times or throughput.
	- Security requirements: Define security requirements, such as authentication, authorization, and data encryption.
	- Software quality attributes: List any quality attributes the application should meet, such as scalability, extensibility, or maintainability.
	
	### **Other Requirements**:
	- Database requirements: Describe the database requirements, including the type of database, data models, and storage needs.
	- Operational and environmental requirements: Specify any operational or environmental requirements, such as deployment environments or hosting considerations.
	- Documentation requirements: List the documentation requirements, such as user manuals or developer guides.

```merm
stateDiagram-v2
    [*] --> Introduction
    Introduction --> OverallDescription
    OverallDescription --> SpecificRequirements
    SpecificRequirements --> FunctionalRequirements
    FunctionalRequirements --> FnReq
    state "FunctionalRequirements" as FnReq
	state "UserManagement" as FnReq
    state "ContentManagement" as FnReq
    state "ContentDistribution" as FnReq
    state "UserEngagement" as FnReq
    state "ContentDiscovery" as FnReq
    state "Analytics" as FnReq
    state "AdditionalFeatures" as FnReq
    SpecificRequirements --> NonFunctionalRequirements
    NonFunctionalRequirements --> UsabilityReliabilityPerformance
    NonFunctionalRequirements --> SecurityMaintainabilityPortability
    SpecificRequirements --> InterfaceRequirements
    InterfaceRequirements --> UserHardwareSoftwareCommunication
    InterfaceRequirements --> OtherNonFunctionalRequirements
    OtherNonFunctionalRequirements --> PerformanceSecurityQuality
    OtherNonFunctionalRequirements --> OtherRequirements
    OtherRequirements --> DatabaseOperationalDocumentation

    state "Usability, Reliability, Performance" as UsabilityReliabilityPerformance
    state "Security, Maintainability, Portability" as SecurityMaintainabilityPortability
    state "User, Hardware, Software, Communication" as UserHardwareSoftwareCommunication
    state "Performance, Security, Quality" as PerformanceSecurityQuality
    state "Database, Operational, Documentation" as DatabaseOperationalDocumentation
```
## Entity-Relationship Database Diagram
#BlogDuaaeeg-ERD  
- This ERD diagram depicts the following entities and their relationships:
	1. **User**: Represents a registered user of the application. It includes attributes such as userId, username, email, password, firstName, lastName, bio, and profilePicture.
	2. **Post**: Represents a blog post created by a user. It has attributes like postId, title, content, createdAt, updatedAt, published, and a foreign key (userId) to link it to the User entity.
	3. **Tag**: Represents a tag that can be associated with a post.
	4. **Category**: Represents a category that can be associated with a post.
	5. **PostTag**: A junction table that associates posts with tags, using postId and tagId as composite primary keys.
	6. **PostCategory**: A junction table that associates posts with categories, using postId and categoryId as composite primary keys.
	7. **Comment**: Represents a comment made by a user on a post. It has attributes like commentId, content, createdAt, and foreign keys (userId and postId) to link it to the User and Post entities, respectively.
	8. **Like**: Represents a like given by a user to a post. It has attributes like likeId and foreign keys (userId and postId) to link it to the User and Post entities, respectively.
	9. **Publication**: Represents a publication within the platform where posts can be submitted.
	10. **PublicationPost**: A junction table that associates publications with posts, using publicationId and postId as composite primary keys.

```merm
erDiagram
    User ||--o{ Post : creates
    User ||--o{ Comment : writes
    User ||--o{ Like : "likes"
    User {
        int userId PK
        string username
        string email
        string password
        string firstName
        string lastName
        string bio
        string profilePicture
    }
    Post {
        int postId PK
        string title
        string content
        date createdAt
        date updatedAt
        boolean published
        int userId FK
    }
    Tag ||--o{ PostTag : tagged
    Tag {
        int tagId PK
        string name
    }
    Category ||--o{ PostCategory : categorized
    Category {
        int categoryId PK
        string name
    }
    PostTag {
        int postId PK
        int tagId PK
    }
    PostCategory {
        int postId PK
        int categoryId PK
    }
    Comment {
        int commentId PK
        string content
        date createdAt
        int userId FK
        int postId FK
    }
    Like {
        int likeId PK
        int userId FK
        int postId FK
    }
    User ||--o{ Publication : "submits to"
    Publication {
        int publicationId PK
        string name
        string description
    }
    Publication ||--o{ PublicationPost : "publishes"
    PublicationPost {
        int publicationId PK
        int postId PK
    }
```
