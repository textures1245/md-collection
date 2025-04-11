[[SA Overview]]

References
[Balancing Coupling in Software Design - Vlad Khononov - DDD Europe 2023](https://www.youtube.com/watch?v=KTy4rqgPOjg)


*Coupling* refers to degree of interdependence between modules, to achieved or produce value that might not be produce from the independence module.

By coupling modules to reducing cost and reusable, make it easy to scale application and reduce time consumed at the start. But on the insight, when application had growth more. the cost for scale and testing application are significant increment. 

![[LinerGraphOfCouplingCostDevelopement.excalidraw | Time Consumer ]]

We know the hardest problem about design software architecture, especially the microservice. Is not about written modules but the design how we should balancing coupling modules. 

Some say that when come to design coupling, the goal is "you should not coupling them". Because the technical debt is start from "you tried to reuse too many component" and then in the means time modules got strong coupling with hard to be fix, even you first intention was just do little coupling and there would be fine. But in reality, is not.

So SOLID participle has born to solve this problem. but everything have their trade-off. 

Yes, this principle solve this by giving the rule that "everything should have one reason to modified" is means that you can't make them coupling (many/one-to-many) because by coupling. means that you will have more than one reason by all mean. but it force you to make (one-to-one). 

This principle is such an "ideal" methodology to help you organizing software structure. but also in the mean time, this would lead to massive code structure as well. and this principle are likely to make it too using more cost to developed new features because each features, modules are independence.

This will lead to investigation about "if we not go extreme on any side".  But we balancing the pro and cons for each of them. to achieved the goal we defined. 

## The Coupling Spectrum

The reality exists on a spectrum between tight coupling and complete isolation: 

```markdown
Tight Coupling <------------------------------------> Complete Isolation
Monolith       Modular      Service-Oriented      Microservices
               Monolith     Architecture          
```


