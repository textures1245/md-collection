[[Coupling In Software Design]]

![[Pasted image 20250410163826.png | Level of Coupling In Structured Design]]
## Connascence 
- Components are **born together** if
	- A change in one component, requires a change in another component 
	- You can postulate a *reasonable* change that would require both components to change

![[Pasted image 20250410175046.png]]
## **Three Dimensions of Coupling**:
    
- **Integration Strength**: How tightly components are bound together.
	- **Intrusive Coupling**
		- Dependence on implementation detail
	- Functional Coupling
	- Model Coupling
	- Contract Coupling
- **Distance**: The separation between coupled components (e.g., physical or logical).
	![[Pasted image 20250410175246.png | Distance from coupled coordination]]
- **Volatility**: The likelihood of changes propagating across coupled components.
	- ## **Key Characteristics of Volatility**
	1. **Rate of Change**:
	    - Components that change frequently are considered highly volatile.
	        
	    - Example: A core subdomain in Domain-Driven Design (DDD), such as pricing logic in an e-commerce system, is likely to evolve rapidly due to business demands[4](https://gist.ly/youtube-summarizer/balancing-coupling-in-software-design-understanding-integration-strength-distance-and-volatility).
	        
	2. **Reasons for Change**:
	    - Volatility arises when components share reasons for change. For instance:
	        
	        - Semantic coupling: Two components sharing a business domain model will both need updates if the model changes.
	            
	        - Functional coupling: Changes in one component's functionality may necessitate updates in another[3](https://www.infoq.com/news/2020/02/balancing-coupling-ddd-europe/).
	            
	3. **Impact on Maintenance**:
	    - If a highly volatile component is tightly coupled with others, changes can cascade across the system, increasing maintenance costs[6](https://www.linkedin.com/pulse/book-summary-balancing-coupling-software-design-bogdan-stirbat-dqw4f).
	- ## ## **Types of Volatile Components**
	1. **Core Subdomains**:
	    
	    - Represent areas critical to business success and competitive advantage.
	        
	    - Tend to be highly volatile due to frequent updates driven by business needs[4](https://gist.ly/youtube-summarizer/balancing-coupling-in-software-design-understanding-integration-strength-distance-and-volatility).
	        
	2. **Supporting Subdomains**:
	    
	    - Less volatile but still require regular updates to align with evolving business processes.
	        
	3. **Generic Subdomains**:
	    
	    - Least volatile; often use standard solutions or patterns that rarely change.