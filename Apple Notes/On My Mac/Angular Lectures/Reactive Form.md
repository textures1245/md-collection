# Reactive Form
#form 

create and manage form on TS (more prowerful then Templete-Driven form)

- **Create form**
 **(error reading attachment)**
1. Create variable to mark as **FormGroup** object
2. Assign form on ngOnInit and created new FormGroup
3. FormGroup will group your form controls together
4. You can created nest form group inside of FormGroup (userData is nest form group)
5. Inside FormGroup you created, you can create new FormControl to your FormGroup (Input, select etc)
6. on **hobbies** is how you make form as Array

- **Connect form on fontend**
 **(error reading attachment)**
1. **formGroup ->** connect with form we created on TS
2. **formGroupName ->** form group controls, connect with nest form group (userData)
3. **formControlName ->** connect with formControl we created on TS


- **Validators**
 **(error reading attachment)**
1. put Validators on seconde arguments (if was a async Validator, put on third arguments )
2. email validators required if use with email control
3. you can style css like template drive did
4. **On callin in HTML**
 **(error reading attachment)**
5. call a form name we created (formOject) and call method name **get** for acessing a form control (this is a **group form control** so we calling from parent follows with children ‘userData.username’ etc)

- **Custom Validators**
- **Synchronous**
 **(error reading attachment)**
1. Asume that we have **forbiddenName** that not allow user to used it (forbiddenUsernames = ["Chris", "Anna"];
2. created a method with params as **FormControl** which return like json data (string type with value of boolean (typeScript syntax))
3. create some condition this exmaple, we check if
	1. **value that enter is like a string array we store or not**
 	2. if not it will return -1 (return null in the end) means not found because we use method **indexOf (return specify index of same value)**
	3. if have (same name) return **{‘nameIsForbidden’: true}** (later use on HTML side)
1. put method on form control we want to validate
 **(error reading attachment)**
*if we create method on the same TS that we create formGroup we must* ***use bind method (this.method.bind(this))***
1. **When using, on HTML side**
 **(error reading attachment)**
2. acessing form control with **errors method** follows with name that we had return
3. you can look what validators we can use on form control element on console
 **(error reading attachment)**
- **Asynchronuous**
 **(error reading attachment)**
1. created method, return as promise or Observable
2. just like how above, resolve boolean json if is true, not just resovle as null
3. return promise we created
4. put on form control, third argument if we have other sync validators
5. how its look like we used it
6. old status -> pending -> new status


- **Array form control**
- ***Note: please look at code for more detail, cuz i lazy to explain lol***