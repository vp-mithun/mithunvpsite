---
title: Understanding Basic types in Typescript Tutorials
tags:
  - TypeScript
url: 462.html
id: 462
categories:
  - TypeScript
date: 2016-05-25 01:10:42
---

Basic types include Boolean (bool), Number, String, Array, Enum etc. in TypeScript. We will be exploring them to know that TypeScript is 'typed superset' of JavaScript.

JavaScript - Optional types & its effect
----------------------------------------

*   _JavaScript is dynamically typed_; variables do not have an associated type, so no type restrictions can be applied to operations.
*   We can assign a value of one type to a variable and later assign a value of a completely different type to the same variable (_Comment 1 in code block below)._
*   We can perform an operation with two incompatible values and get unpredictable results (_comment 2 in code block below)._
*   If we call a function, there is nothing to enforce that you pass arguments of the correct type and you can even supply too many or too few arguments(_comment 3, 4 in code below)._

Let's see working example of this in JavaScript, I hope you have [Setup Visual Studio Code for TypeScript tutorials](http://www.mithunvp.com/typescript-tutorials-setting-visual-studio-code/). Create _basictypes.js_ file and copy below code to explore ill effects of dynamically typed JavaScript

// 1\. Assignment of different types
var dynamic = 'A string';
dynamic = 52;

//2\. Operations with different types
var days = '7';
var hours = 24;
var week = days * hours;

//3\. 77 (concatenate 7 and 7)
var fortnight = days + days;


//4.Calling functions
function getVolume(width, height, depth) {
return width * height * depth;
}
// NaN (10 * undefined * undefined)
var volumeA = getVolume(10);
// 32 (the 8 is ignored)
var volumeB = getVolume(2, 4, 4, 8);

Now just copy the above JavaScript code into TypeScript file, create "_basictypes.ts_".  You would see similar screen shot of it \[caption id="attachment_464" align="aligncenter" width="1366"\][![basic types](http://www.mithunvp.com/wp-content/uploads/2016/05/types-check.png)](http://www.mithunvp.com/wp-content/uploads/2016/05/types-check.png) Type Checking\[/caption\] With this small example of how types is checked in TypeScript, lets learn basic types in this article.

Basic Types in TypeScript
-------------------------

**Boolean** This is most basic datatype which is has either TRUE or FALSE.

let isFamily: boolean = false;

**Number** As in JavaScript, all numbers in TypeScript are floating point values. These floating point numbers get the type number. In addition to hexadecimal and decimal literals, TypeScript also supports binary and octal literals introduced in ECMAScript 2015.

let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;

**String** There is no application, technology, language which does not deal with textual data or strings. Keyword '_**string**_' is used to represent textual data in TypeScript also. They are represented in double quotes or single quotes.

let firstName: string = "Mithun";
let lastName: string = "VP";

The concept of template strings, is widely used and esp in Angular 2 we can HTML written as template string in Component also. _**What is template strings**_? String which span multiple lines and have embedded expressions in them.

> _Template strings_ are surrounded by the backtick/backquote (`` ` ``) character, and embedded expressions are of the form _**`${ expr }`**_.

let fullName: string = \`Mithun VP\`;
let age: number = 31;
let sentence: string = `Hello, my name is ${ fullName }.

I'll be ${ age + 1 } years old next month.`

 sentence = "Hello, my name is " + fullName + ".\\n\\n" +
    "I'll be " + (age + 1) + " years old next month."
    
 console.log(sentence);

\[caption id="attachment_466" align="aligncenter" width="502"\][![basic types](http://www.mithunvp.com/wp-content/uploads/2016/05/templatestring.png)](http://www.mithunvp.com/wp-content/uploads/2016/05/templatestring.png) Template String code & output\[/caption\] **Arrays** An array is simply marked with the \[\] notation, similar to JavaScript, and each array can be strongly typed to hold a specific type. Below code has different ways of using Arrays.

//Declare array in first way
let list: number\[\] = \[1, 2, 3\];

//Declare array in second way
let listNumber: Array<number> = \[1, 2, 3\];

console.log("Array length : " + listNumber.length);

// Empty array
var emptyArray: any\[\] = new Array();

//Delcare and reading array
var skills: string\[\] = \["TypeScript", "Angular 2", "ASP.NET Core"\];

// standard for loop
for (var i = 0; i < skills.length; i++)
{
console.log("Standard for loop: " + skills\[i\]);
}
//for in loop way of reading array
for (var key in skills) {
    if (skills.hasOwnProperty(key)) {
        var element = skills\[key\];
        console.log(element);        
    }
}

**Enums** _**Enums**_ are a special type that has been borrowed from other languages such as C#, and provide a solution to the problem of special numbers. An enum associates a human-readable name for a specific number. By default, _enums_ begin numbering their members starting at `0`. You can change this by manually setting the value of one of its members or you can manually set all values in them.

enum ContactType {Family, Friends, Professional};
console.log("Automatic Enum Value assignment");
console.log("Family Enum Value : " + ContactType\["Family"\]);
console.log("Friends Enum Value : " + ContactType\["Friends"\]);
console.log("Professional Enum Value : " + ContactType\["Professional"\]);

enum ContactManualType {Family =10, Friends, Professional};
console.log("Semi Manual Enum Value assignment");
console.log("Family Enum Value : " + ContactManualType\["Family"\]);
console.log("Friends Enum Value : " + ContactManualType\["Friends"\]);
console.log("Professional Enum Value : " + ContactManualType\["Professional"\]);

enum ContactTypeManualValues {Family =10, Friends=20, Professional=30};
console.log("Fully manual Enum Value assignment");
console.log("Family Enum Value : " + ContactTypeManualValues\["Family"\]);
console.log("Friends Enum Value : " + ContactTypeManualValues\["Friends"\]);
console.log("Professional Enum Value : " + ContactTypeManualValues\["Professional"\]);

In next article of TypeScript Tutorials series, we will explore in detail about "_variable declarations, scoping using var & let keywords_"