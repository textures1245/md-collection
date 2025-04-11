#Software-Achitrectures [[SA Overview]]

![[Pasted image 20250331180706.png]]

*this article had summarised from https://www.youtube.com/watch?v=oAoaMlS1PWo*

## Key Points:

- **Traditional Layered Architecture:** The video starts by describing the traditional layered architecture, where the user interface layer interacts with the business logic layer, which then interacts with the data access layer, and finally, the database.
    
- **Domain-Driven Design (DDD):** The video moves on to discuss DDD as an improvement over the traditional layered architecture. DDD replaces the business logic layer with services and a domain, and the data access layer with a repository.
    
- **Onion, Hexagonal, and Clean Architectures:** The video also touches on other popular layered architectures like Onion, Hexagonal, and Clean, which provide more strict rules for code organization and interactions between different areas of the code.
    
- **Challenges with Layered Architectures:** Despite the benefits, the video highlights the downsides of layered architectures, such as the need for instruction manuals for developers to navigate the codebase, the growth of layer objects over time, and the complexity of making changes across multiple layers.
    
- **Vertical Slice Architecture:** The video introduces the concept of vertical slice architecture, which emerged from the challenges faced with layered architectures. The video mentions that this architecture focuses on optimizing patterns observed in the application rather than upfront prescriptive rules.
    
- **CQRS (Command Query Responsibility Segregation):** The video also discusses CQRS, a principle that separates read and write operations into two different models or objects, which aligns well with the concept of vertical slices.
## Q
1. How do we handle in case if we build frontend and backend separate into their own repo? 
   - Do they both folder structure need be exactly match?
   - Do we need to have adapter (or gateway) for request/respond for both frontend and backend?
1. What is the point that this architecture might had cons if we compared to Layer architectures (Onion, Hex, Clean etc.). Even this was suppose to be corrects defects from Layer architecture

## A
## **1. Frontend/Backend Separation in Vertical Slice Architecture**

### **Folder Structure Alignment**

- **Exact folder structure matching is unnecessary**, but **logical alignment by feature** is critical. Each vertical slice (e.g., "Order Management") should encapsulate frontend and backend components for that feature, even across repositories[2](https://www.jimmybogard.com/composite-uis-for-microservices-vertical-slice-apis/).
    
- Example: A `Shipments` feature might have:
    
    - Frontend repo: `src/shipments/CreateShipmentForm.js`
        
    - Backend repo: `src/shipments/CreateShipmentEndpoint.cs`  
        This ensures cohesive development without rigid structural mirroring.
        

### **Adapters/Gateways**

- **Purpose-built APIs replace traditional gateways**. Vertical Slice APIs advocate for direct, system-specific communication between frontend components and their coupled backend APIs, avoiding generic gateways[2](https://www.jimmybogard.com/composite-uis-for-microservices-vertical-slice-apis/).
    
- **Shared contracts** (e.g., request/response DTOs) can be distributed via NuGet packages or shared assemblies to maintain consistency without tight coupling[2](https://www.jimmybogard.com/composite-uis-for-microservices-vertical-slice-apis/).
    
- Adapters are only needed if integrating third-party systems or legacy code not designed for VSA.
    

## **2. Vertical Slice Architecture vs. Layered Architectures**

### **Key Cons of VSA**

- **Code duplication**: Features with overlapping logic (e.g., authentication) may repeat code across slices, unlike layered architectures where utilities are centralized[1](https://medium.com/codex/vertical-slice-architecture-the-best-ways-to-structure-your-project-9b6eb35655ae)[3](https://codeopinion.com/is-vertical-slice-architecture-better-than-clean-architecture-or-ports-and-adapters/).
    
- **Cross-cutting concerns**: Logging, validation, or caching require careful implementation to avoid scattering logic, whereas layered architectures enforce centralized technical layers (e.g., `Infrastructure` in Clean Architecture)[3](https://codeopinion.com/is-vertical-slice-architecture-better-than-clean-architecture-or-ports-and-adapters/).
    
- **Learning curve**: Teams accustomed to horizontal layering may struggle with feature-centric organization[1](https://medium.com/codex/vertical-slice-architecture-the-best-ways-to-structure-your-project-9b6eb35655ae).
    
- **Scalability challenges**: Large features can lead to "tall files" or excessive complexity within a single slice[1](https://medium.com/codex/vertical-slice-architecture-the-best-ways-to-structure-your-project-9b6eb35655ae).
    

### **Advantages Over Layered Architectures**

- **Reduced coupling**: VSA minimizes dependencies between features, whereas layered architectures often create hidden coupling through shared abstractions (e.g., repositories)[3](https://codeopinion.com/is-vertical-slice-architecture-better-than-clean-architecture-or-ports-and-adapters/).
    
- **Faster iteration**: Changes are localized to a slice, avoiding cascading updates across layers like `Domain` or `Application`[3](https://codeopinion.com/is-vertical-slice-architecture-better-than-clean-architecture-or-ports-and-adapters/).
    
- **Alignment with business workflows**: Features map directly to user requirements, unlike technical layers that prioritize separation of concerns over business value[3](https://codeopinion.com/is-vertical-slice-architecture-better-than-clean-architecture-or-ports-and-adapters/).
    

### **Recommendations**

- For **frontend/backend separation**: Use shared contracts and CI/CD pipelines to synchronize feature slices across repos without enforcing identical structures[2](https://www.jimmybogard.com/composite-uis-for-microservices-vertical-slice-apis/).
    
- For **cross-cutting concerns**: Implement modular libraries or middleware (e.g., MediatR behaviors in .NET) to inject logging/validation without violating slice boundaries[1](https://medium.com/codex/vertical-slice-architecture-the-best-ways-to-structure-your-project-9b6eb35655ae).
    
- For **large systems**: Combine VSA with bounded contexts (from DDD) to group related features and mitigate duplication[2](https://www.jimmybogard.com/composite-uis-for-microservices-vertical-slice-apis/).
    