# Angular Animation
#ngAnimation

Animation provides the illusion of motion: HTML elements change styling over time. Well-designed animations can make your application more fun and straightforward to use, but they aren't just cosmetic. Animations can improve your application and user experience in a number of ways
- Without animations, web page transitions can seem abrupt and jarring
- Motion greatly enhances the user experience, so animations give users a chance to detect the application's response to their actions
- Good animations intuitively call the user's attention to where it is needed

- **Setup**
1. import **BrowserAnimationsModule from @angular/platform-browser/animations in app.module.ts**
 **(error reading attachment)**
2. inside @Component decorator add **animation property**
 **(error reading attachment)**

- **Trigger animation css style**
1. created property binding to mark html element as **target**
 **(error reading attachment)**
***@divState -> targetName ref, state -> stateValue***

1. on **animations** created followed by these
 **(error reading attachment)**
2.1 **trigger ->** function for trigger html element which takes 2 arguments,
 targetName (‘divState’) and callback function which are array 

2.2 **state ->** Declares an animation state within a trigger attached to an element. also takes 2 arguments,
stateValue and callback function

2.3 **style -> passing object which can works as CSS style**

2.4 created event listener to listening an event changed
 **(error reading attachment)**

- **Implementing  basic transition**
 **(error reading attachment)**
1. **transition ->** take 2 arguments, 
	1. stateValue that has been changed which can indicate how it could be changed (=>, <=, <=>)
	2. animate method which takes parameter how long can transition would be

- **Implementing advanced transition**
 In another way when using transition. You want to swap stateValue from the initial state to any stateValue that will be affected by any changes. You can use * (wildcard) as a placeholder for means that affected any stateValue that had been occurring
 **(error reading attachment)**
*new trigger that will effected any stateValue*
 

![[Screen Recording 2565-06-23 at 16.48.34.mov]]

1. we can nested transition, when we want to style more complex transition
 **(error reading attachment)**
	2.1 on the second argument you passed array which holds a bunch of styling
	2.2 animate can pass the second argument which will be executed after the transition animated was succeed
	2.3 you can use another animate as a fallback phase transition
1. using ‘**void’ for inital state,** to set as initalize start animation before it changed to default state
 **(error reading attachment)**
	3.1 Start with ‘void’ state (start with opacity 0 with trasnslate X from -100px ) then wait for changing to ‘default’ valueState 
	(use * to make affected to any valueState)
	3.2 then start trasition to opacity 0 with trasnslate X to 0px 

![[Screen Recording 2565-06-23 at 17.38.55.mov]]

1. **Keyframe**
 **(error reading attachment)**
5. Group transition, this will start transition animate at the same time
 **(error reading attachment)**

- **Callback animation**
 **(error reading attachment)**
you can triggerd callback function which can work wth animation