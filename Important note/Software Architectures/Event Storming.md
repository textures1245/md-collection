[[Domain Driven Design (DDD)]] [[SA Overview|SA Overview]]

Ref: https://www.youtube.com/watch?v=p7IqD83ljvc, https://github.com/ddd-crew/eventstorming-glossary-cheat-sheet?tab=readme-ov-file

- EventStorming is the smartest approach to collaborate beyond silo boundaries. Comes from a diverse multi-disciplined group of people who, together, have a lot of wisdom and knowledge. Originally was invented  for workshop to model [[Domain Driven Design (DDD)]] 
- It now has a broader spectrum. From gaining a big-picture problem space of the whole domain to gaining insight into the entire software delivery flow and creating a long term planning.

## Core Concepts 
1. **Domain Event (<mark style="background: #FFB86CA6;">Orange</mark>)**: the main concept. It is an event that is relevant for the **domain experts** and contextual for the domain that is being explored. Verb is mark as the past tense
2. **HotSpot (<mark style="background: #FFB8EBA6;">Neon-Pink</mark>)**: used to visualise and capture hot conflicts. Conflicts caused by, and not exclusive to, inconsistencies (in language), frictions, questions, dissent, objections, issues or procrastinating going deep to explore for later. 
	![Event & HotSpot](https://github.com/ddd-crew/eventstorming-glossary-cheat-sheet/raw/master/_resources/core-concepts.jpg)
1. **Timeline**: Becomes a powerful tool when we have a story to tell, when we have a timeline. The **paper roll** on the wall represents time from left to right. We can have parallel streams from top to bottom on the paper roll.
	![Timeline](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQueEDF1uz83MlMeXhnBavp_TysBJPVdQ60aw&s)

4. **Chaotic Exploration**: used at the start of EventStorming. Each person writes Domain Events by themselves that they can think off. They will put these Domain Events in order they think they happen on the paper roll.
5. **Enforce the Timeline**: A phase happening after chaotic exploration, meaning we try to make the timeline consistent and remove duplicate stickies.

## Big Picture EventStroming
- assess the health of an existing line of business or explore the viability of a new startup business model. It helps the group create a shared state of mind of the vision of that domain of the company. We can use the output as input for Conway’s law alignment.

	by organising business flow around teams and software with emergent bounded contexts. you can look advantage for these key concepts
	
1. **Opportunity**: Hotspot can have a negative association, we also give people the chance to add opportunities.
	   Using <mark style="background: #BBFABBA6;">Green</mark> to represent something positive.
2. **Actor/Agent**: group of people, a department, a team or a specific person involved around a (group of) Domain Event(s).
	   Using <mark style="background: #FFF3A3A6;">Yellow </mark> to represent them
3. **Value**: We would add value into stream map, after we have made the timeline consistent. Make explicit where the value is in our domain
	   Using <mark style="background: #FF5582A6;">Red</mark> and <mark style="background: #BBFABBA6;">Green</mark> to show positive and negative value.
4. **Pivotal Event**:  we start looking for the few most significant events in the flow. For an e-commerce website, they might look like “Article Added to Catalogue”, “Order Placed”, “Order Shipped”, “Payment Received” and “Order Delivered”. These are often the events with the highest number of people interested.
	   ![Pivotal Event](https://github.com/ddd-crew/eventstorming-glossary-cheat-sheet/raw/master/_resources/pivotal-events.PNG)

5. **Swim-lanes**: Separating the whole flow into horizontal swimlanes, assigned to given actors or departments, is another tempting option since it improves readability. This seems the most obvious choice for people with a background in process modelling.****
	![](https://github.com/ddd-crew/eventstorming-glossary-cheat-sheet/raw/master/_resources/boundaries.PNG)

	**Now we got Big Picture EventStroming components to picturing emerging bounded contexts**
	![](https://github.com/ddd-crew/eventstorming-glossary-cheat-sheet/raw/master/_resources/big-picture-tools.jpg)

## Emerging Bounded Contexts
- First indicators of where to start deep-diving towards designing bounded contexts around business problems.
	![](https://github.com/ddd-crew/eventstorming-glossary-cheat-sheet/raw/master/_resources/emergent-bounded-contexts.PNG)
	![](https://github.com/ddd-crew/eventstorming-glossary-cheat-sheet/raw/master/_resources/big-picture-legend.jpg)

## Process modelling EventStorming
- Assess the health of a current process in the company. It helps the group create a shared state of mind of the current status quo of the process, find bottlenecks and identify parts of the system to decouple from the existing software.
	![](https://github.com/ddd-crew/eventstorming-glossary-cheat-sheet/raw/master/_resources/process-modelling.PNG)


1. **Policy**  A policy is a reaction that says “whenever X happens, we do Y”, eventually ending up with in the flow between a Domain Event and a Command/action. A policy can be an automated process or manual. A policy can also be named a reactor, eventual business constraint or rule or a lie detector because there is always more to policies than you first think.
2. **Command/Action**  Represents decisions, actions or intent. They can be initiated by an actor or from an automated process. During process EventStorming.
3. **Query Model/Information**: To make decisions an actor might need information, we capture these in a Query Model. For process EventStorming information might be more recognised by stakeholders
	![](https://github.com/ddd-crew/eventstorming-glossary-cheat-sheet/raw/master/_resources/process-design.jpg)

- **Enforce colour coding**: to visualize flow of the timeline which it creates a different dynamic. Often used after or during enforcing the timeline
	![](https://github.com/ddd-crew/eventstorming-glossary-cheat-sheet/raw/master/_resources/process-picture.jpg)
- **Constraint** :  A restriction we have or need to design from our problem space when we want to perform a command/action, another word could be consistent business constraint or rule. The official color to use is a big yellow post-it. 
	![](https://github.com/ddd-crew/eventstorming-glossary-cheat-sheet/raw/master/_resources/software-picture.jpg)

## Event Storming Case-Study on E-commerce
- Scenario from [[Domain Driven Design (DDD)]]

https://app.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1#Htextures1245%2Ftextures1245%2Fmain%2FE-commerce%20Event%20Stroming

![[E-commerce Event Stroming.drawio.png]]

