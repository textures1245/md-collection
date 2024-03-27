# Angular CLI
#NG_CLI

- **Important commands (type —help behind of these commands to get more information)**
1. **ng serve ->** start application 
2. **ng generate ->** generate angular files
3. **ng list ->** check if angular files were complete or not
4. ng buld -> build angular app (—configuration=production for production building)

- **Created Mutiply projects in one folder** 
1. **Set Up**
	1. **Created emtpy ng project folder to store ng projects -> ng new <folder-name> —create-application=false**
	2. **cd inside project we had created.**
	3. **created ng project -> ng generate application <application-name>**
1. **Run application**
	1. **ng serve  --<folder name that containes project>=<project-name> (ng serve —projects=shop-store)**
	2. **or angular.json file, you can config to auto serve application  which can be changed in the end of code->**  "defaultProject": “<project-name>” 

- **CLI Schematics**
 **(error reading attachment)**
It’s just like a blueprint you can custom **your own cli command to do or generate something which it can be include with third party package like Angular Material for example create navigation from ng marerial’s schematic**

1. **install Angular Material -> ng add @angular/material and ng add @angular/cdk**
2. **then** ng generate @angular/material:navigation <component-name> (main-nav)
 **(error reading attachment)**
 **(error reading attachment)**

- **Builder CLI**
 **(error reading attachment)**
1. when want to deploy application with angular CLI first we need to install **ng add @angular/fire**
2. **install host which you want to hosting your application (npm i -g firebase-tools for example)**
3. **then created app with -> ng build configuration=production**
4. and deploy app with **-> ng deploy**

- **Understand Differential Loading**
 **(error reading attachment)**
On image above, it shows diffrent download strategy between **modern and legacy browser**

when you building your application on **dist folder** you will see a lot of bunch of javascript. These JS files are **Diffrential Loading** which would be loaded to diffrent user browser
 **(error reading attachment)**
*for this example you would notice the different between these files, this is what it called Differentail Loading*
***which all these files would depend on user’s browser version***
1. ***if on Legacy Browser (old browser like IE 1-9) it would took es5 to be loaded on browser for old browser can be support it.***
2. ***if on Modern Browser, it would loaded es2015 instead which it’s modern, less code and support on modern browser***

***So this strategy, angular can still keep running on every browser version and not loaded unnecessary codes***

***more:*** [***https://www.udemy.com/course/the-complete-guide-to-angular-2/learn/lecture/17862114#content***](https://www.udemy.com/course/the-complete-guide-to-angular-2/learn/lecture/17862114#content)

- **Angular Libaries**
*Angular application that can be shared across of angular application like Angular Material*
1. ***ng generate library <application-name>***
2. ***ng build <application-name>***
3. ***cd to build application (dist/application-name)***
4. ***npm publish***
*when using own libaries*
***Global***
1. *ng build <application-name-that-had-been-built>*
2. *then import application* ***import { myExport } from ‘<app-library-name>’;***
***Local***
1. *you can export it from file location that you had build*
 **(error reading attachment)**


Rouded up: https://www.udemy.com/course/the-complete-guide-to-angular-2/learn/lecture/17862130#content