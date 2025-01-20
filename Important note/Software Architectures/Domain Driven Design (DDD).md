[[SA Overview|SA Overview]] #Ubiquitous-Language

Ref: [DDD](https://medium.com/devspree/domain-driven-design-934c495e7e0b)

- The design to solving business complexity by guildine the way to thinking which way we can implement solution for complexity or grows-up business.

## Concept
- Domain-Driven Design is an approach to software development that centers around the core domain of your application. 
	- It's a set of principles and patterns that enable you to create software systems that accurately model and reflect the complexities of the problem domain they're built to address. In essence, DDD aligns software design with the business domain.
- Business or Domain concept shouldn't coupled with any technology or framework. this will leads to huge problem when it comes to changing environments 
## Key Concepts

1. **Ubiquitous Language** 
	The Ubiquitous Language is a common, shared language that both technical and non-technical team members use to discuss the domain. It ensures that everyone understands the domain's terms and concepts.

 2. **Bounded Contexts**
	Bounded Contexts are explicit boundaries within which a particular domain model is defined and applicable. They help prevent conflicts and misunderstandings when working on large, complex systems.

3. **Entities and Value Objects**
	Entities are objects with distinct identities that are defined by their attributes. Value Objects, on the other hand, are objects defined by their attributes but without a distinct identity. They represent concepts like dates, money, or geographical coordinates.

 4. **Aggregates**
	Aggregates are groups of related Entities and Value Objects treated as a single unit. They enforce consistency and transactional boundaries within the domain.

## Design
### 1. Strategic Domain-Driven Design
![Strategic Domain-Driven Design](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*hCPeJf71tN0P2Pza)

- In term of strategic design analytic. we focus on high-level domain concept which have 2 importance roles

1. Domain Expert — Someone who has knowledge and understanding of the business and industry in which the company operates. Including an understanding of the company’s high level business concept (various flows in the system).
	1. A Domain Expert can be a Product Designer or someone from the Business side. All you need is an understanding of the business model and various flows of the system. 
	   
2. Technical Expert — At this point you might be a software engineer or architect, someone who will take a domain concept and develop it into a solution.

	So In this step we would group domain expert and technical expert to diagram the business communication for compile works and concepts to defines **bounded context** on problem (domain) where they must be resolve. therefore resources sharing will appropriate. 
	
	The case-study that we may can take a look is **[Event Storming](https://www.eventstorming.com/)** . A flexible workshop format for collaborative exploration of complex business domains.

#### Strategic Domain-Driven Design Step

1. **Understand the domain** which is the customer’s problem that our system will solve. Here we can split the domain into smaller domains, called subdomains, to focus on smaller problems. 
2. **Business discussion on ubiquitous language**: Domain experts and technical experts discuss business concepts and come to an agreement on how to describe them. and call things What do we need to do in these systems to get a ubiquitous language, which is a common language used in the development process.
3. **Divided into bounded contexts**: or work systems. Each work section must have a clear concept that does not overlap with other work systems.
4. **Creating a context map:** that shows the relationship of all bounded contexts.

#### Domain and Subdomain
- Domain is problems that our business will solve. it leads to problem reflection from target customers 
	Assume, we building product that help facilitate the rental of real estate domains. the core domain is **rental management**

	But we can see the problem from this design where only have one model that would be hard for it can be explains the whole business. Since single business often has details or functions that are further broken down. For example, Companies that do business related to finance There may be a division of work into **Personal finance** or **corporate finance**, or there may be an **accounting system.** these we called them **Subdomains** 

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ghlhz6IbMvDKPF6Auh7T5Q.png)
	
- So in the Business Industry. we can't have only one domain to explains the whole business. to know how do we should design our business domain. we should know the type of domain
	1. **Core (sub)domain** —  this part is the main activity of the organization. And it is a competitive advantage that the organization must do its best. For example, in an e-commerce system there may be a catalog as the core domain because without this domain the company cannot sell products. This part is where the company must invest the most resources. For example, best practices may be enforced in various areas of development, such as the team in charge of using concept extreme programming to produce quality code. 
	2. **Supporting (sub)domain** — This is the part that supports the work of the core. domain from a marketing perspective This will create an augmented attribute of that product, that is, it will make the core domain work better. For example, in e-commerce, there may be a recommendation subdomain to recommend products to the target group. This will help the core domain such as catalog work better. However, this part may not be the main income generating part or the main activity for the organization. It may not be necessary to enforce best practice much. You may consider hiring an outsourcer to do it. 
	3. **Generic (sub)domain** — This part can be used to purchase ready-made software or services available in the market. The obvious thing is that notification systems might consider using SendGrid, or if they want a user management system, they might look at solutions like Auth0 or Okta to use.
	in [[eTOM]] framework have provides tons of domains for every business industry. 

## ## Problem Space & Solution Space

- Problem Spaces: Focusing on customer problem and requirement 
- Solution Space: Focusing on creating a set of solutions in the problem space or various domains in the problem space. Take the solving process to analysis, understand and export it into product. each solution would come in term of bounded contexts that consists of many small sub-systems.
  
![](https://miro.medium.com/v2/resize:fit:1024/format:webp/0*9iiFmrxpgtOAE_5p)

## Ubiquitous Language 
#Ubiquitous-Language

![](https://miro.medium.com/v2/resize:fit:1070/format:webp/0*neItX62UhMJ1WlUI)

1. Can change with time according to the business context and changing interpretations It was not thought up once and then used forever. 
2. It must be used by everyone working in the relevant work area, such as an engineer or product designer 
3. Used to write code in the domain model.
4. It must not be a concept that was created solely by an engineer or domain expert. 
5. It doesn’t have to be a term actually used in that industry. Let it be a word that can be understood between those working together in that domain. 
6. Concepts or words that are not relevant should be eliminated.

## Bounded Contexts
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*6raAFbFQ0SFdkJZd)

- System boundary that is separated from a domain. Each bounded context has a unique domain model and Ubiquitous Language.
	A subdomain is a set of problems in the problem space, but a bounded context is a set of solutions in the solution space.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*68QShLn5gOgZUxwZACsLPA.png)

- if we look deep inside into model layer. we will discovery the concept of product is in both purchasing and personal sales bounded contexts. Even they are both have Product model, but The interpretation in each context is completely different.
	- On **Personal Sales Context** We may only be interested in product information related to individual sales.
	- On **Purchasing Context** may only be interested in the product code (SKU), price, or the number of products the customer puts in the cart to create an order.

- In summary. Bounded context is a system that has its own business / domain concept and can work on its own. At present, many Companies that have applications microservices will view bounded context as each microservice.

## Context Map

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*z4ynI8VcisA0637p)

- It the diagram displays system overview. it explains detail relationship between each bounded contexts

## Case Study e-commerce platform

You are tasked with building an e-commerce platform with the following features:

1. **Product Catalog**: Manage product information (name, price, description, stock, etc.).
2. **Shopping Cart**: Allow customers to add/remove items from their carts and calculate totals.
3. **Order Management**: Handle order creation, payment, and shipping.
4. **Customer Management**: Maintain customer profiles, purchase history, and addresses.

### **Step-by-Step DDD Design Process**

#### 1. **Understand the Domain**

Start by breaking the system into **domains** and **subdomains** based on the problem space.

- **Core Domain** (most critical to business):
    
    - **Order Management**: This drives revenue and must be highly accurate and consistent.
- **Supporting Subdomains**:
    
    - **Shopping Cart**: Supports the customer in preparing an order.
    - **Product Catalog**: Manages product information but doesn’t directly handle transactions.
    - **Customer Management**: Supports order processing and personalization.
- **Generic Subdomains**:
    
    - **Payment Integration**: Handles payment processing with external services.

#### 2. **Define Bounded Contexts**
#### 3. **Identify Core Concepts**
#### 4. **Define Relationships Between Contexts**
#### 5. **Implement Anti-Corruption Layers (ACLs) and Open Host Service (OHS)**

![[Drawing 2025-01-16 13.24.47.excalidraw]]


