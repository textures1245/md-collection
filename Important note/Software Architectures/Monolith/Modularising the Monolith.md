[[SA Overview]]

Starting with a single application that becomes a "big ball of mud" over time, making changes difficult and risky. Breaking it into front-end and back-end components often doesn't solve the problem.

...

- ## Modular Monolith
	While microservices is made for scalability. the `micro` aspect was overemphasised, leading to many services and a lack of understanding of how thet fit together.  

- ### Vertical Slice Architecture
	A "sliced architecture" is presented as a way of organizing code around the axes of change, which stretch from the front end all the way back down to the database.
	- Be independent 
	- Contain all logic and data to produce desired functionality
	- Defined the interface

- ### **Boundary Definition:** 
	Boundaries are easier to draw when there's an actual source of information and logic to look at. Defining boundaries and discovering them is one of the hard parts of the system. Organizational structure can inform the boundaries of the system.