# Programs, Javascript, and p5.js

**Goals**: 

By the end of this section, we hope to answer the question: _what is a program, and how do I build one?_ 

This assumes no prior programming experience. Even if you are well versed in the act of programming, this is a nice conceptual refresher and serves as a reference sheet for writing in Javascript and p5.js! 👩🏽‍💻✨

By the end of this, you should:

1. [Have a rough mental model of a program's state and lifecycle](#what-is-a-program);
2. [Know the core grammars of Javascript](#learning-javascript); and
3. [Understand how a p5.js program works](#the-anatomy-of-a-p5js-program).

Let's get started 🤗

## What is a Program?

A program is a way to communicate with your computer. When we write a program in any sort of programming language, our goal is for the computer to interpret it and do something.

But accessible programming languages are pretty new for everyone. Computers evolved from calculators, so naturally that is what they understand and do best. The earliest programming languages were centered around math precision, and when we talk about computer representation, it's often centered around the idea of binary — ones and zeroes.

That is far removed from our daily experiences and interactions with computers. Over time, we have built programs to perform increasingly complex tasks. We use computers to get our news, communicate with friends and family, and apply for benefits. We send and receive vast quantities of numbers in ways that hide their true makeup, and we consume them in formats that are easy for us to understand in our own visual and linguistic terms. 

We've invented a ton of programming languages in the last few decades to bridge this gap between the computer and the machine. The most flexible and intuitive of them you may have heard of: Python, Ruby, Javascript, C++. The most esoteric you may enjoy as mental exercises in perspective: [Chef](http://progopedia.com/language/chef/), which represents computer instructions as real human recipes...

```
Lobsters with Fruit and Nuts.

This recipe prints "Hello, World!" in a most delicious way.

Ingredients.
72 g hazelnuts
101 eggs
108 g lobsters
111 ml orange juice
44 g cashews
32 g sugar
87 ml water
114 g rice
100 g durian
33 passion fruit
10 ml lemon juice

Method.
Put lemon juice into the mixing bowl.
Put passion fruit into the mixing bowl.
Put durian into the mixing bowl.
Put lobsters into the mixing bowl.
Put rice into the mixing bowl.
Put orange juice  into the mixing bowl.
Put water into the mixing bowl.
Put sugar into the mixing bowl.
Put cashews into the mixing bowl.
Put orange juice into the mixing bowl.
Put lobsters into the mixing bowl.
Put lobsters into the mixing bowl.
Put eggs into the mixing bowl.
Put hazelnuts into the mixing bowl.
Liquify contents of the mixing bowl.
Pour contents of the mixing bowl into the baking dish.

Serves 1.

```

or [Piet](https://www.dangermouse.net/esoteric/piet/samples.html), which strives to represent computer instructions as abstract geometric art, inspired by real human artist Piet Mondrian.

![](https://upload.wikimedia.org/wikipedia/commons/d/d0/Piet_Program.gif)

and, in fact, machine knitters use something like this to program physical knitting patterns:

![](https://images.deepai.org/converted-papers/1904.05681/figures/implementation/glove_instructions.png)

Our goal in this workshop is to utilize one of these (normal, useful) languages for the purpose of _expressing_, talking and thinking with our computers more like a human fax machine than in the ways we may be used to.

> If the task you have for your computer is a common, well-understood one, such as showing you your email or acting like a calculator, you can open the appropriate application and get to work. But for unique or open-ended tasks, there probably is no application.

## Learning Javascript

Learning any programming language is all about research. The first thing people generally learn in any new language, programming or otherwise, is how to say 'Hello!' 

For computers, we like to hear it say 'Hello World!' — a sure sign that it is well and alive and ready to talk. Here is how you say 'Hello World!' in Javascript:

```js
console.log('Hello World!')
```

When you run this, it should say:

```
Hello World!
```

Of course, it's not reasonable to know this off the bat. How did I know that `console` was a thing in Javascript? What does `.log` do? How do I know that putting text at the end of it will get the computer to talk to me?

**Language** is something that has to be learned, through reference material and documentation and the tried and true method of others teaching you how to say the things you want to say.

[Hello World in 28 Programming Languages](https://excelwithbusiness.com/blog/say-hello-world-in-28-different-programming-languages/)

When we get down to coding real, living examples, you'll see how many programmers tend to work — by relying heavily on Google and trying things out.

But there's a second problem: where exactly do you run this piece of code? 

For most languages, it's not immediately obvious. Luckily, Javascript is the language of the web.

### About Web Browsers

Web browsers are computer programs that allow you to view web pages. Web pages are formatted using a language called HTML, often alongside JavaScript.

JavaScript was invented in the 1990s as a way to verify data and provide interactivity on web pages. Today it's widely used by all types of programmers, in all types of scenarios — not just in web browsers but in web servers, desktop and mobile applications, video games, and even microcontrollers.

Almost all browsers allow you to type and run JavaScript code directly from your browser, through a "console." 

The console is often used as a tool for debugging (finding, investigating, and fixing code problems). It's also a great way to play around with JavaScript and get a feel for how it works.

![](https://i.imgur.com/ieymxiq.png)

### Primitive Values

|Type|Description|
|----|--------|
|Undefined| Represents unintentionally missing values.
|Null| Represents intentionally missing values.
|Booleans| True and False (used for logical operations)
|Numbers| All types of numbers! (see below)
|Strings| Used for text.

#### Numbers

|Type|Examples|
|----|--------|
|Positive Numbers|`1, 2, 3`|
|Negative Numbers|`-1, -2, -3`|
|Fractionals|`1.01, 2.05938`|
|Infinities|`+Infinity, -Infinity` e.g. `1 / 0, -1 / 0`|
|"Not a Number"|`NaN` e.g. `0 / 0`|

#### Strings

```
> "hello!"
"hello!"
```
 
#### Mathematical Operations

```js
> (2 * 5) + 4 + (8 / 2) - 1
17
```
   
#### Booleans

|Operation|Example Input|Example Output|
|---------|-------|-----|--------------|
|Equality | `1 === 1`, `"hello!" === "hello"`   | `true`, `false` |
|Inequality| `1 !== 2`. | `true` |
|Comparison| `1 > 1`, `1 >= 1` | `false` `true` |
|Combinations| `true && false`, `true || false` | `false`, `true` |

```
> 1 == 2 || 1 > 0
true
```
 
#### Logical Operations ("Conditionals")

```js
if (100 < 200) {
  "this number is small!"
}
   
"this number is small!"
```
   
```
if (300 < 200) {
  "this number is small!"
}
   
undefined
```

```
if (300 < 200) {
  "this number is small!"
} else {
  "this number is big!"
}
   
"this number is big!"
```

#### Variables

```js
let mode = "disco"
if (mode === "disco") {
  console.log("disco time!")
}
```
   
Changing values:
   
```
let mode = "disco"
   
// do some stuff
   
mode = "alt rock"
```
   
Missing Values:

```js
let x
// x is undefined ("not yet ready to be used")

x = 200
/// x is 200 ("x is a value that is ready to be used!")

x = null
// x is null ("x is intentionally nothing now")
```
   
### Breaking Down a Grammar

Now that we've written a few pieces of JavaScript, let's take a pause and look at how the computer is actually interpreting our code.

- Numbers are pretty recognizable from how they're represented in calculators.
- Strings are declared using quotation marks (single or double quoted)
- We use `let` to declare new variables and store data.
- Parentheses are used in a couple different places.
- Curly braces are used to enclose "blocks" of code.

Under the hood, we may also observe the following traits of our program:

- A program runs one "expression" at a time
	- usually one line, but can be multiple for readability. 
	- _generally_, it goes top to bottom. but there are many grammars that tell it to go back to a previous line, or to go somewhere else entirely outside of our program. We've already done this with `console.log`, actually!
	- Look for grammmar that indicates a "container" of code: curly braces, brackets, parentheses.

### Mental Models for Programming

This is a lot to think about! With any new language, it's important to consider how sentences are structured and the ways that memory is shaped by sentence structure. Studies have shown that [the word order within languages informs how our brains remember information](https://www.nature.com/articles/s41598-018-37654-9), and that's true for programs as well.

When we run a program, it's remembering things about the state of the world at any given moment. When we declare a variable:

```js
let message = "hello!"
```

We've introduced something that we want to remember and use later on. The program's world may grow to contain many memorable things, and variables are our way of holding on to those things.

## The Anatomy of a p5.js program

[See the introductory collection here.](https://editor.p5js.org/kyeah/collections/y65SYurzv)

## Recommended Readings

g### Practical Readings

* For more applicable examples, Allison Parrish's notes and interactive tutorials on creative coding in JS and p5.js are extremely valuable. 

  These tutorials go over expressions, variables, and how we can leverage them to make our lives easier; they also delve a little bit into our next class, around building our own programmatic loops:
	* [Javascript in the browser console](https://creative-coding.decontextualize.com/browser-console/)
	* [Expressions, Variables, and Loops](https://creative-coding.decontextualize.com/expressions-variables-and-loops/)

  A few relevant p5-specific tutorials are available on [First Steps](https://creative-coding.decontextualize.com/first-steps/) and [Text and Type](https://creative-coding.decontextualize.com/text-and-type/).

### Programming Mental Models

* [Just Javascript](https://justjavascript.com/) is a new, Work-In-Progress newsletter for _understanding_ JavaScript, with a focus on building mental models for how programs work.

* [Eloquent Javascript](https://eloquentjavascript.net/00_intro.html) (Chapters 0-2) gives you a bottom-up perspective on programming, Javascript, and coding as a system of language.
