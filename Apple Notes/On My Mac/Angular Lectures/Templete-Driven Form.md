# Templete-Driven Form
#form

simple form control that you can created with angular on HTML side

- **Created form and binding with ngSubmit**
when created a form. We need to let **angular control our form behavior.** By doing this when submitting, angular will given infomation So you can handle with it **like check if form inputs are valid or not etc**.
 **(error reading attachment)**
	1. Binding ngSubmit to given angular control our form
	2. used local ref to handle form on backend (TS), there are 2 ways that we can use local ref with ngForm directive
		2.1  directly put local refs on argument
 **(error reading attachment)**
		2.2 created variable with **viewChild decorator** 
 **(error reading attachment)**
 **(error reading attachment)**
*with theses we can handle form that angular given to us*
 **(error reading attachment)**
 **(error reading attachment)**
Can’t submit if inputs aren’t valid

- **Binding data**
1. *No binding data*
 **(error reading attachment)**
2. ngModel -> gives angular to control our input 
3. # username -> a local ref for using with ngModel (given input information that we can handle with.)

4. *Two ways data binding*
 **(error reading attachment)**
 **(error reading attachment)**

- **Custom Form Validation**
 **(error reading attachment)**
When using with local ref and set it to ngModel you can handle those things like validation for example
1. add **required, email (for email input), local ref (set it to “ngModel”) and ngNativeValidate for custom validation**
2. on ngModel that angular given to us had a lot of things that we can manage
	1. valid -> check if this value is valid for input type or not
	2. touched -> check if this input had ever touch yet
1. **With this we can manage our custom validation**
 **(error reading attachment)**

CSS Stylesheet
 **(error reading attachment)**

- **Grouping Form Data**
 **(error reading attachment)**
*when got many input and we want to group it together we can set “ngModelGroup” it will grouped it as JS Object* 
*this example we grouped*  ***email and username***
 **(error reading attachment)**