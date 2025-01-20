[[User Story Mapping (USM)]][[Event Storming]][[SA Overview]]

1. **USM and ES Goals**: Both are intended to foster a better understanding of the business, but they do so in distinct ways:
    
    - **USM** focuses on visualizing **high-level actions** and workflows from a user's perspective. It is excellent for aligning teams on what the product should do and prioritizing user-centric features.
    - **ES** focuses on understanding the **underlying business processes**, often through the lens of Domain-Driven Design (DDD), identifying problems and opportunities for improvement.
2. **USM for High-Level Understanding**:
    
    - You are right that USM is simpler and easier for teams to grasp at a high level, especially for those without technical expertise. It works well to establish a shared understanding of user journeys.
3. **ES for Deeper Insight**:
    
    - ES dives deeper into the **problem space** and **solution space**. By leveraging a **Ubiquitous Language** and involving both domain and technical experts, it can bridge the gap between business and technology. This makes it a more comprehensive tool for designing software architecture.
4. **Using Them Together**:
    
    - Your perspective that USM and ES can be used together is correct. While USM is great for visualizing workflows and user needs, ES can add depth by modeling the events, processes, and interactions within the system.

---

## **Where I Think You Could Refine Your Understanding**

1. **Sequence of USM and ES**:
    
    - You propose starting with USM because it is higher-level and easier to understand before moving to ES. While this can work, many practitioners prefer the opposite:
        
        - **ES first**: By mapping out the domain's events and processes in detail, teams can gain a foundational understanding of the business logic. This makes it easier to then create a USM that aligns with the real-world processes.
        - Starting with ES allows you to address **complexities in the domain** early on, which might otherwise be oversimplified or missed in a high-level USM.
    - That said, this sequence is not a rule. It depends on the team's context:
        
        - If the organization is new to agile or lacks clarity, **starting with USM** may be a gentler introduction.
        - For teams familiar with DDD or tackling complex domains, **ES first** may yield better results.
2. **Limitations of Only USM or ES**:
    
    - You correctly noted that **USM alone cannot help organizations globally understand** their business—it focuses on the user journey rather than internal processes.
    - Similarly, **ES alone might lack user-centric insights**, focusing more on how the system works than on user priorities. Combining both ensures a balance between user needs and system design.
3. **Migration Between USM and ES**:
    
    - Migrating between USM and ES is not a straightforward conversion. They serve different purposes:
        - **USM outputs user workflows and priorities**—it doesn’t inherently model the events or processes behind those workflows.
        - **ES outputs domain events and processes**—it doesn’t inherently prioritize features or workflows for users.
    - Instead of thinking about "migrating" one into the other, it’s better to think of them as **complementary tools** that inform each other.

---

### **A Suggested Approach**

You are on the right track in thinking both approaches can coexist. Here’s how they might complement each other in practice:

1. **Start with ES for Domain Understanding**:
    
    - Gather domain experts and technical teams.
    - Use ES to map out events, identify aggregates, and uncover bottlenecks or areas for improvement.
2. **Use Insights from ES to Create USM**:
    
    - Translate the processes uncovered in ES into user workflows and high-level stories.
    - Prioritize features based on user needs while ensuring alignment with the business processes mapped in ES.
3. **Iterate Between Both**:
    
    - Revisit ES when new complexities arise.
    - Update USM as priorities or user needs shift.