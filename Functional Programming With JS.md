## Getting Started With Functional Programming Using JavaScript

**Understanding *Functional Programming* & Why It Is The Programming Approach That You Need To Use As A Developer** 

If you start googling the term *functional programming*, you will more than likely be intimidated by how it is explained - with all of the academic lingo. I say this because it was for me  :).

**What Is Functional Programming?**

Functional Programming or *FP* is nothing more than a programming paradigm, meaning that it is a different way of thinking about the construction of software. *Yes! It is not a new language but rather a new ***approach*** that uses fome fundamental defining principles that we will look at below.* Functional programming is **declarative** rather than **imperative**. 
*What is the difference?* When you write your code using the imperative approach, you inherently have to write the logics of **how** to achieve the desired result. On the other hand, with imperative programming, your focus is on **what** you want to achieve.

**Governing Principles Of Functional Programming**

In order to wrap your head around the concept of functional programming, you need to understand the following concepts:

* Pure Functions
* Function Composition
* Avoid Shared State
* Avoid Mutating State (Immutability)
* Avoid Side Effects

Still wondering why even learn this approach? My answer to you is, functional code tends to be **more concise** (*who doesn't like concise code?*), **more predictable** and easier to test than that of imperative code.

#### Pure Functions

Pure functions have two characteristics:

* **Predictability: Same Input, Same Output**: This means that every time a particular input is made then you should get the same output or expected outcome (*predictability*).

* **No Side-Effects**: This simply means that there should be no mutating input, logging, HTTP calls or writing to disk (*immutability*).

**Predictability: Same Input, Same Output**
The function below illustrates the characteristic of predictability. One perform a calculation in their head of the expected outcome based on the two inputs and the output should reflect that unless the person did an incorrect calculation.

```

let product = (x, y) => {
    console.log(`The product of ${x} and ${y} is`, x*y)
}

//Function Call
  product(3, 4)
  
// Output: The product of 3 and 4 is 12
```

**No Side-Effects**
If a function performs any operation that is not related to calculating the final output of that function then it more than likely affecting something else (side effects).
A side effect is any application state change that is observable outside the called function other than its return value. To further explain this, please see the example below that shows a function that has side effects vs. on that does not.

```
let age = 25
console.log(`Age before function call: ${age}`) //Output: 25

var calAge = (birthYear, currentYear) => {
   return age = currentYear-birthYear
}

calAge(1996, 2020)

console.log(`Age after function call: ${age}`) //Output: 24
```
Considering the code snippet above, we can see that the function `calAge`, when called, will mutate the value of age, thus, making it impure.

If we are to take the same snippet of code and do a small modification like:
```
let age = 25

var calAge = (birthYear, currentYear) => {
   let newAge = currentYear-birthYear
}

calAge(1996, 2020)
```
Although the function does not modify any variable outside of its scope, it is still impure because it does not return a value.

If we are to further alter our code snippet to look like:
```
let age = 25

var calAge = (month) => {
   let currentYear = 2020
   let birthYear = 1996
   let myAge = currentYear-birthYear
   return myAge;
}

calAge(7)
```
The above function is still impure although it does not modify any variable outside and it returns a value. *Why is it still not pure?* Well, it is simple, the return value is not dependent on the value that was passed into the function.

Thus, as a take away, pure functions:
* **Should not** modify any variable outside of its scope
* **Should** return a value
* The returned value **should** be dependent on the argument passed into the function.

The snippet below is a pure function:
```
let myAge = 24
console.log(myAge)//Output: 24

var calMyAge = (currentYear, birthYear) => {
    let myCalAge = currentYear - birthYear
    return myCalAge
}
console.log(myAge) //Output: 24
```
#### Function Composition
Function composition works best when we *curry* a function, yes! curry it. What is that? Ah, well I glad you asked.

**Currying**

In its most basic sense, currying is a technique or method in functional programming in which a function with several arguments is transformed into a sequence in nesting functions. 
```
/*
Sentence Builder: This function expects a subject and predicate. When it receives both arguments, it combines them into one to form a complete sentence.
*/

        //The first argument that is expected is a subject
        var sentenceBuilder = (subject) =>{
 
         //Once a subject was passed in, the next function is executed
          return (predicate) =>{
              console.log(subject + " " + predicate)
          }
      }
  
  
  let displaySentence =  sentenceBuilder
  
  displaySentence("Devon Wintz")("is writing his new JS function.")

  //Output: Devon Wintz is writing his new JS function.


```

**Understanding The Concept of Higher-Order Functions**

A characteristic of JavaScript that makes it suitable for functional programming is the provision for **higher-order functions**. In a nutshell, a higher-order function is a function that can receive a function as an argument or return a function. 

You can find some sample codes that are well commented to guide you from top to bottom in the root directory, ***higher-order-functions.js***. In JavaScript, functions are treated as *first-class citizens*, meaning that JavaScript functions are regarded as objects. Thus, they can be assigned as a value of a variable, enabling them to be passed and returned just like any other reference variable.

**Taking Functions As Arguments**
```
/*
    **************      TAKING FUNCTIONS AS ARGUMENTS      **************

    <<Example 1>>

    In the following example, we will create a simple greetings function that
    will print a greetings based on the user's privilege level
*/

/* We first create two simple functions that displays the greetings using 'console.log'
   Take note that we are using the arrow function syntax
*/
        var greetAdmin = () => console.log("Hey Admin, welcome back. Do you want to continue form where you left off?")
        var greetUser = () => console.log("Hey User, welcome back. Do you want to continue form where you left off?")

/* Because functions are treated as objects in JS, we can assign them as values of variables.
   In doing so, we are able to pass them as arguments in functions.
*/
        let admin = greetAdmin
        let user = greetUser

/* Next we will create our greetings function that will take as arguments: level, which in this case 
   will be the user privilege. Note that the level is set to one, which simply means that if a level is
   not passed in, the user greetings will be displayed as that is the default user privilege level. The other arguments are our reference variables
   that now hold or point to our two greetings functions, defined above.
*/
        
/* Here we are using the 'conditional operation', which is a shorthand for the if condition.
   The condition is as follows: condition ? expression 1: expression 2
   If expression 1 evelaues to true then that is executed, else expression 2 is executed.
*/  
       var greet = (level=1, admin, user) => {
       level === 0 ? admin(): user()
       }

/* The following will display the user greetings since no value was passed in and we set the default to call the 
   user function
*/
       greet('', admin, user) 

/* However, if we explicitly set the level as 0, the admin greetings will be displayed. */
      greet(0, admin, user) 

/* Likewise, if we explicitly put the level as 1, the user greetings will be displayed. */
      greet(1, admin, user) 

```