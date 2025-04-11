[[Design Pattern Overview]]

Ref: https://refactoring.guru/design-patterns/mediator

Is a behavioral design pattern that reduce chaotic dependencies between objects. Restricts direct communications between the objects and forces the dependencies to collaborate only via a **Mediator object**.

Pattern comes to fix the problem that we have complex of couplings dependencies involve for example. Let say you have a dialog for creating and editing customer profiles. It consists of various form controls such as text fields, checkboxes, buttons, etc.

![Relations between elements from UI can become chaotic as the application evloves.](https://refactoring.guru/images/patterns/diagrams/mediator/problem1-en.png?id=86f99055b3e60bb8834dcc7222922bdf)

## Solution

So the Mediator pattern suggests that you should cease all direct communication between the components which you want to make independent of each other. instead, these components must collaborate indirectly, by calling a special mediator object that redirects the calls to appropriate components . As a result, the components depend only on a single mediator class instead of being coupled to dozens of their colleagues.

In the *previous problem*. the dialog class itself may act as the mediator. Most likely, the dialog class is already aware of all of its sub-elements, so you won't even need to introduce new dependencies into this class.

![Using Dialog class as Mediator itself. so classes don't need to communicate directly](https://refactoring.guru/images/patterns/diagrams/mediator/solution1-en.png?id=dd991a5b7830de8d43f82b084e021713)

## Applicability

1. Use the Mediator pattern when it’s hard to change some of the classes because they are tightly coupled to a bunch of other classes.
	 The pattern lets you extract all the relationships between classes into a separate class, isolating any changes to a specific component from the rest of the components.

 2. Use the pattern when you can’t reuse a component in a different program because it’s too dependent on other components.
	 After you apply the Mediator, individual components become unaware of the other components. They could still communicate with each other, albeit indirectly, through a mediator object. To reuse a component in a different app, you need to provide it with a new mediator class.

 3. Use the Mediator when you find yourself creating tons of component subclasses just to reuse some basic behavior in various contexts.
	 Since all relations between components are contained within the mediator, it’s easy to define entirely new ways for these components to collaborate by introducing new mediator classes, without having to change the components themselves.

## Structure

![](https://refactoring.guru/images/patterns/diagrams/mediator/structure-indexed.png?id=a82d4cf1b92a4f72af32f231ffd21131)

1. **Components**: reference to a mediator, declared with the type of mediator interface. The component isn't aware of the actual class of the mediator, so you can re-use the component in other programs by linking it to a different mediator.
2. **Mediator interface** : Declared methods of communication with components, which usually include just a single notification method. Components may pass any context as arguments of this method, including their own objects, but only in such a way that no coupling occurs between a receiving component and the sender's class
3. **Concrete Mediators**: Encapsulate relations between various components. Concrete mediators often keep references to all components they manage and sometimes event manage their life cycle.
4. **Components must not be aware of other components**:  Since the solution for Mediator is to handle coupling dependencies. Components must relate only one-to-one on Mediator. If something important happens within or to a component, it must only notify the mediator. When the mediator receives the notification, it can easily identify the sender, which might be just enoug to decide what component should be triggered in return.

## Usage
![](https://refactoring.guru/images/patterns/diagrams/mediator/example.png?id=3151c153533e816e226be0ef977211e8)

```ts
// The mediator interface declares a method used by components
// to notify the mediator about various events. The mediator may
// react to these events and pass the execution to other
// components.
interface Mediator is
    method notify(sender: Component, event: string)

// The concrete mediator class. The intertwined web of
// connections between individual components has been untangled
// and moved into the mediator.
class AuthenticationDialog implements Mediator is
    private field title: string
    private field loginOrRegisterChkBx: Checkbox
    private field loginUsername, loginPassword: Textbox
    private field registrationUsername, registrationPassword,
                  registrationEmail: Textbox
    private field okBtn, cancelBtn: Button

    constructor AuthenticationDialog() is
        // Create all component objects by passing the current
        // mediator into their constructors to establish links.

    // When something happens with a component, it notifies the
    // mediator. Upon receiving a notification, the mediator may
    // do something on its own or pass the request to another
    // component.
    method notify(sender, event) is
        if (sender == loginOrRegisterChkBx and event == "check")
            if (loginOrRegisterChkBx.checked)
                title = "Log in"
                // 1. Show login form components.
                // 2. Hide registration form components.
            else
                title = "Register"
                // 1. Show registration form components.
                // 2. Hide login form components

        if (sender == okBtn && event == "click")
            if (loginOrRegister.checked)
                // Try to find a user using login credentials.
                if (!found)
                    // Show an error message above the login
                    // field.
            else
                // 1. Create a user account using data from the
                // registration fields.
                // 2. Log that user in.
                // ...

// Components communicate with a mediator using the mediator
// interface. Thanks to that, you can use the same components in
// other contexts by linking them with different mediator
// objects.
class Component is
    field dialog: Mediator

    constructor Component(dialog) is
        this.dialog = dialog

    method click() is
        dialog.notify(this, "click")

    method keypress() is
        dialog.notify(this, "keypress")

// Concrete components don't talk to each other. They have only
// one communication channel, which is sending notifications to
// the mediator.
class Button extends Component is
    // ...

class Textbox extends Component is
    // ...

class Checkbox extends Component is
    method check() is
        dialog.notify(this, "check")
    // ...
```

