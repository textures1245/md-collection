# Determine navigate/Confirm data
 # with CanDeactivate interface
#route 

 **CanDeactivate** Guard determines whether we can navigate away from a route, is called whenever we navigate away from the route before the current component gets deactivated.

The best use case for **CanDectivate** guard is the data entry component. The user may have filled the data entry and tries to leave that component without saving his work. The CanDeactivate guard gives us a chance to warn the user that he has not saved his work and give him a chance to cancel the navigation.

1. Created service to contain both **interface component and deactivate service**
 **(error reading attachment)**

 **(error reading attachment)**
*Interface we created to used with* ***CanDeactivate***

 **(error reading attachment)**
***CanDeactivate Guard*** *which it takes 4 params and return interface that we had created before* 

**Make sure to provided them on appModule**

1. Select which path we want to deactivate  
 **(error reading attachment)**

2. On component we deactivate, implement **Service we created for deactivate then created method canDeactivate() which followed constructor we had created.**
 **(error reading attachment)**
 **(error reading attachment)**
*For this example. At the core, we check if this server name and status are the same as before (did it changed) and  if had been changed yet.* ***If changed then display window to confirm data otherwise just navigated***  

 **(error reading attachment)**
***When we changed something and didn’t update but navigate to other path***