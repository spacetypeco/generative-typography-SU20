# Session 2: Grids and Automation

This tutorial is mostly enacted through a weaving series of p5 sketches, interactive visualizations, and helpful excalidraw diagrams 😅

- [Full sketch collection](https://editor.p5js.org/kyeah/collections/M93qYCeRI)

## 1. In the Beginning

In the beginning, we had manual labor — we did the repetitious work of laying things out on the screen! Here is a simple version of Molnar's Squares, done manually by calling the `rect` function nine times.

![](https://lh3.googleusercontent.com/pw/ACtC-3eIDoiqDGosx81fFIsbZ_O6421vty8L-cOB9hMHLFsD0BZPu1z5BaQaJeIWYTPkbg54TDqkEtmI9WW943jOA11nAR1ygAJAhRh91mskDWjWrZfCESttRbCEecrB8p9kebSHSNSovHuZCnoBcEXq--KIAA=s800-no?authuser=0)

[Molnar Squares (Manual)](https://editor.p5js.org/kyeah/sketches/vzstQNiuZ)

This is okay, but it makes experimentation quite difficult and tedious. What if we wanted to expand our painting to encompass more squares? or draw each square multiple times?

![](https://lh3.googleusercontent.com/pw/ACtC-3dBFp0mZFN0hN_Vj8k5Py3r7yPj7Wli7gMKlLFxlwlqXPCfNCZuB2MXcVn-cfkb6ncUA33acNHAQC2c0g6gX-Xy645OnRngfdT8vOsdOY3qej_YJgAx6GA2tpxHwvMJfVm3nfrUJx2yB7_i2tKSFzWDjg=s800-no)

To do this, we need to break down the process of making these squares. What would it look like to do this by hand?

Perhaps we'd go row-by-row and draw each square:

![](https://lh3.googleusercontent.com/pw/ACtC-3cw2e0XLwMWFaXhzkSmgzbTENbTG-7pOYu1uvO5IRcKE-j4Ojav0qpO7P41pkJLrMFJPhGJ5ZM_hSH_ugEamvwtPyP2Lk0Cg8YsaMriLEsiwvDwyf-bk-OBhRXTKxH5gAR_pgu3i1D_xVGcSgHI6bAY0g=s800-no)

And if we managed to encode this **process** of drawing a bunch of squares in a grid format, we could easily go from a 3x3 grid to a 10x10 grid by describing the **variables** that we are interested in: the number of rows, and the number of columns.

To do this, we'll need to talk about automation in programming.

## 2. Automation and The Number Line

In general, automation in programming comes in the form of for-loops.

Code:

```js
for (let y = 0; y < 400; y += 10) {
  console.log("y = ", y)
}
```
Output:

```js
y = 0
y = 10
y = 20
...
y = 390
```

[Sketch example](https://editor.p5js.org/kyeah/sketches/T0iYDDR_0)

There are lots of different ways to visualize this! You can think of it like a number line:

![](https://lh3.googleusercontent.com/pw/ACtC-3cKRpWYODwX3_C4ytEDq6xVbhSOwFqk0UDJJZPsC-UAhNxDH3qqA7dYqopqsE98yTf4QkB8g8l_cszkzMedrmslp_MNuog32aydnufbNiR62NXhxWY3Y4s208oPIHISLnX1ZT7rA6-t6f1Yu_EMbzhrJQ=w1582-h452-no)

Where we define a variable, `y` that:

- starts at 0 (`let y = 0`)
- goes to 390 (`y < 400`)
- and changes in increments of 10 (`y += 10`).

Here is that same logic broken down next to the number line:

![](https://lh3.googleusercontent.com/pw/ACtC-3dAmjgVDEnjL7y5mIqzb0_wWkVxAfV7dd1DOGuyt8sspLO4ceVxOcoddwVlCh2xrJDnAljWTo7VIFs-t0ppGbEK_hXnL5r2v-csALFj9-8nYiTwvEoWWri8Av9wZXIZ2MVf7M5OxxSpwznlZgZB-47hcA=w1602-h658-no)

In this way, we can see that the first two components define the "number line":

```js
let y = 0; y < height;
```

and the last component defines the rate of change:

```js
y += 10
```

## 2b. Alternative Mental Models

In terms of logic that we've already talked about, we can think of it as:

- Defining a variable, `y`, which starts at 0
- Creating an "if" statement that loops over and over again:

![](https://lh3.googleusercontent.com/pw/ACtC-3elgXSPbmbIuRJQOVxssb9f4LcInQ4Bpt39wV_GXBr_dxlOkP1Ogx6HGHDdV7A6unaVmV-qYGf8SJpGPHOfA8Ym3KlvTX0t3n4_wp4lWxhIzTKSkDHuao6GXZw-I9mvjCs5wFCSjQhx6Dyftcg2kXI6YA=w1264-h1050-no)

But this is kinda abstract! Instead, I try to consider the actual _output_ of the code, which is essentially a **list of values** for `y`.

![](https://lh3.googleusercontent.com/pw/ACtC-3d0ZV3Tb-xZReS-9l9TwZ9nBn3TKV5bSbxgvsPWjWUWJ42zr83TQqKdLvfXJ9XE65fj_tA0UWNRKgqE4HAO_cl-Ynsg_vp6maFqeeEk1k1bCPwsPnwy1IL6NiJMGXmeQaeoOBrZPdAp9xl21pufjK_fcA=w1430-h574-no)

In fact, we could write out this list of values manually in JavaScript and use that instead:

```js
// Define all of our values
let yValues = [0, 10, 20, 30, 40, 50, 60]

// Go over the list of values and call it `y` in each iteration.
for (let y of yValues) {
  console.log("y = ", y)
}
```

Output:

```js
y = 0
y = 10
...
y = 60
```

## 3. Working with Grids

Now that we've covered for-loops, we can use it to break down our canvas into a grid space.

Take this process for example:

![](https://lh3.googleusercontent.com/pw/ACtC-3exaFYVOXFEaAfYb1gLdbHtaQ-P6ewASYg9Dz4shGFS5o_O3LTXbHMzuvX-mb_Yegw05_8WC-Pm4bGf5MvLXJKMWS1IcC6rrXTCvwwP7780ME_qzgAQDP54hGlykYZow3WQA4On6wV8B_9c_mmyl-FGTg=s400-no)

We can imagine this as two separate for loops:

- one for the red lines with changing x values
- one for the blue lines with changing y values

And that's exactly what's happening!

```js
// Draw vertical red lines
for (let x = 0; x < width; x += 10) {
  line(x, 0, x, height)
}

// Draw horizontal blue lines
for (let y = 0; y < height; y += 10) {
  line(0, y, width, y)
}
```

[Sketch Example](https://editor.p5js.org/kyeah/sketches/0Oia5Dv8c)

## 3b. Working with Grid Points

The above example is great! But, the most interesting things happen near the _center_ of our canvas, at the intersections of these red and blue lines. How can we access these points in an automated fashion? How might we draw, say, a grid of dots?

![](https://lh3.googleusercontent.com/pw/ACtC-3eODda031YlnOcuK8Ezl_tqDEGZ-5A7CiZv2TmX3yt8rAuedVzwAjK7kE0ttJAaPQOE7h-bfk9nCy7yTjJYKlcZvw4jfV6j6PEaVQ3_aqmktamVMXM9i6XzdMAMqrC63KgC_2jwqomD4ek3khDZcRkiRQ=s800-no?authuser=0)

To do this, we can put a for loop **inside of a for loop**. A nested for loop!

```js
for(let y = 0; y < height; y += 10) {
  for(let x = 0; x < width; x += 10) {
    circle(x, y, 5)
  }
}
```

- Go down the canvas with y = 0, y = 10, y = 20, ...
- For each y value, draw a row of dots with x = 0, x = 10, x = 20, ...

This is pretty confusing, but it may be more readable if we use a human-readable function name:

```js
for(let y = 0; y < height; y += 10) {
  drawRowOfDots(y)
}

function drawRowOfDots(y) {
  for(let x = 0; x < width; x += 10) {
    circle(x, y, 5)
  }
}
```

[Sketch Example](https://editor.p5js.org/kyeah/sketches/7sSx7ybrV)

This is saying:

- For each y value (0, 10, 20, 30...), draw a row of dots.
- To draw a row of dots, we take a y value and draw dots at (x=0, x=10, x=20, ...)

Alternatively, we can shift our mental model a bit and think about it in terms of rows and columns. Below, we're drawing a grid of 'a's:

```js
for (let row = 0; row < 6; row += 1) {
  for (let column = 0; column < 6; column += 1) {
    // draw an 'a'
  }
}
```

![](https://lh3.googleusercontent.com/pw/ACtC-3c7g9Oy1OFsx8qO8UmCq-95PU-pm_WY6G1yPF-pcAKrWEG5GMagVISAqelMl5_Ateijp68SvWohdnA0d9lK7v5dCKdZzpyyY1p5n8kUNtkKoVOpLKdF_fo8-R-9crO5RLKx-96eVHrSp6DjWiDJdF2d7A=s800-no?authuser=0)

Visually, we may think of these two loops as follows:

![](https://lh3.googleusercontent.com/pw/ACtC-3cMM4Fsm3zbqIMaGTO7hXFJiT8kt5-PGOCSm-t1hPuCGobtwfTyf-x8y9hp_EX1Dh-9H0sL0bWaj6WSw4nZu633lrsjRJ2rpThv5gTYVANz4Z_XY98yIJThXkXAGmkKx7vu9anSJjScaPkG8fYzXDqkvA=w1588-h1096-no?authuser=0)

- row starts at 0 and column loops through 0 --> 5. 
- Once it's done, the "inner" for-loop ends and we increment `row`. (row += 1)
- We start a new inner for-loop, where column goes from 0 --> 5.
- We increment row again. (row += 1)
- repeat until done :)

If you create for-loops in this manner, you may notice some differences in positioning. In particular, we need to do some math to space grid points properly:

```js
for (let row = 0; row < 6; row += 1) {
  for (let column = 0; column < 6; column += 1) {
    let x = row * 10    // grid points are 10 pixels apart
    let y = column * 10 // grid points are 10 pixels apart
    circle (x, y, 5)
  }
}
```

When modifying x and y directly as we were doing before, we can just use it:

```js
for(let y = 0; y < height; y += 10) {
  for(let x = 0; x < width; x += 10) {
    circle(x, y, 5)
  }
}
```

## 4. Molnar, automated

Once we're at this point, we can talk again about our Molnar Squares! Since we are interested in a 3x3 grid of squares, we can use a nested for-loop to build our composition.

[Sketch Example](https://editor.p5js.org/kyeah/sketches/7_Vibctrt)

This sketch uses a for-loop, but also harnesses the power of variables (in the vein of Meta Font.) This shows us the power of automation as it pertains to experimentation!

We have codified the **process** by which Molnar Squares are created, and by abstracting a few variables like:

- the number of rows/columns (`numRows`, `numCols`)
- and how big our squares are based on the number of rows/columns (`squareLength`)

We can now play around with different densities of square grid compositions. I recommend changing the number of rows/columns and seeing what happens!

## 5. Strings as Lists

Briefly, I mentioned that you can define lists of values in JavaScript ("arrays").

```js
let yValues = [0, 10, 20, 30, 40]
```

This is of interest for text as well! We can think of text like `"apple"` as being a list of characters:

```js
let chars = ["a", "p", "p", "l", "e"]
// same as let chars = "apple".split("")
```

Which means we can iterate over them with for loops:

```js
for (let char in chars) {
  console.log(char)
}

// a
// p
// p
// l
// e
```

or pick out individual characters based on their position:

```js
let num = 0
for (let column = 0; column < 6; column += 1) {
  // Get a character based on position.
  let char = chars[num]
  
  // Draw the character.
  let x = ...
  let y = ...
  text(char, x, y)

  // Go to the next character, wrapping back to the first character if needed.
  num = (num + 1) % chars.length
}
```

![](https://lh3.googleusercontent.com/pw/ACtC-3fcd0pa4zL2MFNDaU13e4Yui4HN-iRoi6eoCD1rvtTFiIi6yUes0itPm7-SpzEFfE1SfwkJ74K_BuDO7wh23L3choCEN3cosvXfr-U6iHLMnIFCLtDO4nfBf5JUTQqgCzocPNdd4owQu2rU_kK35raW_w=s800-no?authuser=0)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/krlk_bQaS)

We can even randomly pick values out of a list:

```js
for (let column = 0; column < 6; column += 1) {
  let char = random(chars)
}
```

[Sketch Example](https://editor.p5js.org/kyeah/sketches/6cHMtqMX7)


## 6. Images as Grids of Pixels

To get you thinking of how to go from images --> ASCII art, we should talk about accessing images. Images can be considered a grid of pixels:

![](https://flyingmeat.com/acorn/docs/pixel_grid0.jpg)

Which means we can access pixel values at an (x, y) position:

```js
img.get(x, y)
```

and get values from it, like brightness:

```js
colorMode(HSB, 255)

let pixel = img.get(x, y)
let brightness = brightness(pixel)

// Brightness is between 0 and 255
```

In this way, we can think about a program that might:

1. Load an image.
2. Resize it to fit the canvas.
3. Loop over every pixel value, and draw it on the canvas with a rectangle.

[Sketch Example](https://editor.p5js.org/kyeah/sketches/yvm447xyv)

You might think of it like a scanner, scanning each line and digitizing it :)

![](https://lh3.googleusercontent.com/pw/ACtC-3fCQ8i12Z9r1deuoWa8GsB_7qABrXc0zcCtSJWjjMCGUdoyv2yHYTysWoi86rV7M9FgR0qjSYqvl2Sw209-jC7_ru-Ig2WxxAnxA2kC5DpGeQvwv6QFKq2PPjX_1nhkbQm5gdevFreBf5qHDZiPf_Ck0A=s800-no)
