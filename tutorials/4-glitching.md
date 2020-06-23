# Section 4: Glitch Art

So far, we've been working with text as images. Let's talk about some ways to break text down into its components! We'll talk about independent objects in programs, how to re-build interesting shapes from the ground up, and then a little bit of how we can reconstruct the generated image pixels in order to make something new and glitchy.

[Sketch Collection](https://editor.p5js.org/kyeah/collections/wW-igCan0)

## textToPoints

P5js has a very accessible function for breaking text down into individual points:

```js
let points = font.textToPoints("B", 0, 0, fontSize)
```

[Sketch Example](https://editor.p5js.org/kyeah/sketches/BEybEe_du)

### Objects

If you `print` the list of points, you'll see them described as `Objects`:

```
print(points)
```

And if you expand this in the console, you can view individual point objects:

|points list|individual points|
|---|---|
|<img style='max-width: 600px' src='https://lh3.googleusercontent.com/pw/ACtC-3d1BA0tCoA_AleFi5TTAtUP1Ye4awvV2eD7yRYqEZ0_sGIPuZt5-RpRwOmTapVezPln0LUrUGYqRoSpqEbiTZsObdQvfpbQ07djBcxY1IFF7JuAt17Apz611XYyzlPow4N1sZ-fpTfxfX5rkSzDo08o_g=w1824-h624-no'/>|<img style='max-width: 600px' src='https://lh3.googleusercontent.com/pw/ACtC-3dMAaWFTDO57RlL1ujohzkjW7D3s7jnd-0Ae4L2sPeGo_t8HN4UbJkjg58GGM82NHdlcm-1f8UagqB_VoOjfPyeKIX0sOqkeViTFXZcBPWOh-7NG1OLXXsZO6Iwro6g4hkQ1LxgQth29_-3sqF2tNeZ7g=w716-h428-no'/>|

Objects are often used to represent many independent things with the same shape. In this example, textToPoints returns many objects with the following shape:

```js
{
  x: <number>,
  y: <number>,
  alpha: <number>
}
```

Which may be visualiized like this:

![](https://lh3.googleusercontent.com/pw/ACtC-3dfP2xR6oFYMsYXi4IVk_NLJHdiUWeqz2iEoIC_Wyw9s_Phki1qbbTARMXHOxIGeWzZpHQVwLhPlcKDlg5HOHiv3GojmER660CJeIIEMCTLxKxK3Rbdb_V3JstetnNR6OYskjV8wvlFhZPSMCKIh6Qb9Q=w1530-h309-no)

We can think of a _single_ object as a blue box of values, or perhaps as a very tiny dictionary! If we wanted to look up the meaning of x or y within a single point, we could do so:


```js
let firstPoint = points[0]

firstPoint.x
> 100

firstPoint.y
> 200
```

And if we wanted to get all the words in the dictionary, we can ask for it:

```js
Object.keys(firstPoint)
> ['x', 'y']
```

If we draw each object onto the canvas:

```js
for (let point of points) {
  ellipse(point.x, point.y, 5)
}
```

We can see that they correspond to the outline of our text!

![](https://lh3.googleusercontent.com/pw/ACtC-3dA8xdR62oOlo8a75Xe0WUvi1qJmoteo75UJhvyzsFZuoulEf7M9Fn_yjwfDNDjAGTjVNrSajoVUXUmU7Rc_Qg7IrWlOWOaW4pXaP0iJM5MpeVQhFJR5RUtmRn6f4OYCsJKdhwhDzxabX8VmK2aiamqBg=s1000-no)

In general, objects are very powerful structures for managing many independent moving things in your program. They can keep their own state independent of other objects, and you can update existing objects as well with new information. For example, if you wanted each point from `textToPoints` to have a separate color, you can assign colors to them:

```js
function setup() {
  points = textToPoints(...)
  
  for (let pt of points) {
    pt.color = color(random(255), random(255), random(255))
  }
}
```

![](https://lh3.googleusercontent.com/pw/ACtC-3dz64_Li4kQm7sFUWNEXnYMuq2OKqFtl6tZFhZCrmwY6RAQZtuFZZqgqelrT0pa_8I3semQ-jYcT-9pXQRJRFg8isQzVssOUPqQcsgkBHvMKbGx_r-I059fsrwIr8fhEDGJEkKNGFo0L3LnjxlLpCBSxg=s1500-no)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/JgZjNomvW)

In a little bit, we'll talk about how they can also be added, removed, or updated as your program progresses.

### Creating your own objects

Although we've been talking about point objects returned by `textToPoints`, you can create objects of any data shape that you need. For example, you could have individual pieces of text that you create and use later:

```js
let text1 = {
  word: "hello",
  x: 100,
  y: 100
}

let text2 = {
  word: "there",
  x: 200,
  y: 100
}

let texts = [text1, text2]

...

text(texts[0].word, texts[0].x, texts[0].y)
```

### textToPoints alpha

As a last note, you'll notice I haven't talked much about the `alpha` value from `textToPoints`. This value represents the overall path's angle (in degrees) at that point. For example, if we were to draw these angles as arrows, it'd look something like this:

```js
push()
translate(point.x, point.y)
rotate(radians(point.alpha))
line(0, 0, 10, 0)
pop()
```

![](https://lh3.googleusercontent.com/pw/ACtC-3dIDfpr4t7mMR3NKnBeFk5fT3fAo4A3yvZfirJric892yYx_5MW1aRCdCdmv9aKo9KLGwusTnltEphIrmooVL6F7J1NdCO02jjC7RzdCMnorb0NK1yio1QgK0bo938pgKCMItnn6w7CJ8HZA_ColqP6jg=s1000-no)

and if we were to rotate them an additional 90 degrees, we could view the tangents/normals:

```js
rotate(radians(point.alpha + 90))
```
![](https://lh3.googleusercontent.com/pw/ACtC-3d9sEJ-FFhfXIRt9rQ8fva0MHBze9pwxpM5Sx7H9cewFfZnOitemTUhGQLvjWIIm89UPJhdRRNjvUSryEts4Oxvqzf8OWL5s2qHVM-6fiHZoMx3ASCFm3ZKuObN_S6mB2OqucM5XyxPCFO8_4hduE_fyw=s1000-no)

We won't cover use cases for this in this class, but you should know that they are available if you are interested in any precise and advanced shape modifications (e.g. expansions.)

### Resolution

One important thing to note about `textToPoints` is that it downsamples by default. To receive better path resolution (more points), you should provide an additional option to the textToPoints call:

```js
let points = font.textToPoints("B", 0, 0, fontSize, {
  sampleFactor: 0.5 // default is 0.1
})
```

Note that this will decrease performance (because there are more points.)

|sampleFactor = 0.10|sampleFactor = 0.15|
|---|---|
|<img src='https://lh3.googleusercontent.com/pw/ACtC-3f63Vn5ksDRf3QOkhKMBC9TlsvUwnG-tsh_D-QNOEA3vC7G4lBozUi3q22EgAmH_M-6wERQGxRnA3S9_06VjUhouXxuR6o89kfxRwl9ZGYbk5iRdfdpKlH7-RqfNrZj3Cr23CE_l3rEVvM-624U3VQ12g=s1000-no'/>|<img src='https://lh3.googleusercontent.com/pw/ACtC-3fav4SSUsvoKYUb_zS0niZc9NBwv4MX-DYysdMd8SFnmFSVUIca6FzLt8F4J0ucDWwuHCawa1UgY2h5ndxtNN7fO1Aq56OuR2Hpf9f8SjW6Vy3UIYL_Uv1ex08RulBNEw6RrBq7qY5jVhgkzooGklC7qw=s1000-no'/>|

|sampleFactor = 0.20|sampleFactor = 0.25|
|---|---|
|<img src='https://lh3.googleusercontent.com/pw/ACtC-3dAYia_Rs6WROP41f3E03bK6-V8vXF_0ZVNJMnuWnrcVIqU-3-59q0pierQTEDw2Hd7HEQB_wuUkF9Ihd95YQWSc_oEjp04-KZ9IozA77w8MwfEtkesyKHDvBhKqTRhJZ40jJtkze9jzOBePppfsLGVKQ=s1000-no'/>|<img src='https://lh3.googleusercontent.com/pw/ACtC-3cDCLUmTS3Phzp1sDXwkz1IGP1MtUTAjFzD75sqqfTmdsJZafGmabYQCzr2tkKIBd16lB4pVWWG-FFtTVnBNWyIJvMPg82An0xuscruKYd7Z2lyOwNdhoVAoioVNxGc5kjVNbCqRrl_x4dfctBAG37Hkg=s1000-no'/>|

### Bounds

If you're interested in getting the exact _bounds_ of your text, you should take a look at [font.textBounds](https://p5js.org/reference/#/p5.Font/textBounds). These bounds are generally helpful for layouts — for example, centering your text visually instead of using the typeface attributes:

```
function setup() {
  points = font.textToPoints(...)
  bounds = font.textBounds(...)
  
  // Center all points around the origin
  for (let pt of points) {
    pt.x = pt.x - bounds.x - bounds.w/2
    pt.y = pt.y - bounds.y - bounds.h/2
  }
}

```

[Sketch Example](https://editor.p5js.org/kyeah/sketches/ByhZB7J-1)

## Point-based Art

By breaking down a piece of text into its individual components, we open up a lot of possibilities. This will cover only a couple of ideas that I think are interesting :)

### Adding and Removing points

With any list of things, you can imagine adding new things, removing old ones, or modifying their order!

Here, every JavaScript list has a function called [splice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice). The following snippet removes a single object from a random part of the list:

```js
let positionToRemove = int(random(points.length))
points.splice(positionToRemove, 1) // Remove one item at that position
```

We can do it many times to get something like this:

![](https://lh3.googleusercontent.com/pw/ACtC-3choIW263EqryVlHDCTEHS1cDohUK2nLsy3GKS1LVwYy7SiOOnKX8ahNcPj7BuO-OsE2lGTKVKmzAe_X5dPy3RDAQjSIRkl_skdQn0NHkpNe_4zB0qMhQWoGN1eP78BKlGs3lPC-kDjiX4YO4Vu7c0Tfg=s1000-no)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/-KNLb1XHU)

`splice` is also capable of inserting new items. The following snippet adds a new point at a random list position:

```js
let positionToAdd = int(random(points.length))

points.splice(positionToAdd, 0, {
  x: int(random(width)),
  y: int(random(height))
})
```

However, this point has nothing to do with the original shape — we are getting a random X and Y value. To be a little more precise, we can add a new point next to to an existing point:

```js
// Get an existing point
let pt = points[positionToAdd]

// Add a new point next to it with a little bit of randomness
points.splice(positionToAdd, 0, {
  x: pt.x + random(20) - 10,
  y: pt.y + random(20) - 10
})
```

If we do this many, many times, we can begin to see an odd sort of growth happening to our text :)

![](https://lh3.googleusercontent.com/pw/ACtC-3cp2ItozOE3nUaT6ZqOmDaI4Tlfan0XJNkTl7Zybr8f6eOTI7pN-wIIvRE_9bRqr5F5zIDUgItx2MtY2GBWv9NXxJtnGgxIaHttWbge1FOgxVA1o4qo0m7xpF-hfnmKLyFvCfFC8rQsmZjQxQHU3NXBUw=s1000-no)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/YU0Opxyqd)

For folks interested in the Game of Life or similar cellular automata, this hopefully gives you some inspiration on how you can apply those algorithms in a typographic context.

### Modifying objects over time

Objects can be modified as time progresses. For example, you may imagine that the points' positions are randomly updated over time:

```js
for (let pt of points) {
  ellipse(pt.x, pt.y, 5)
  pt.x += random(0.01) - 0.05  // Add or subtract up to 0.05
  pt.y += random(0.01) - 0.05  // Add or subtract up to 0.05
}
```

[Sketch Example](https://editor.p5js.org/kyeah/sketches/Rkg1uFfuF)

This type of modification would add up over time, so as your program progresses, the text would slowly become unreadable. Another approach would be to keep points the same, and only add randomness when you draw them:

```js
for (let pt of points) {
  ellipse(
    pt.x + random(50) - 25, 
    pt.y + random(50) - 25
    5
  )
}
```

### Extruding Shapes

Another way to use text points is to use them as a re-usable template. In this example, we extrude them diagonally by duplicating the points and drawing lines between them:

|sampleFactor = 0.1|sampleFactor = 0.5|
|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3f0Ikk4Ho3nhh9DWakclxLKATl97vqmqO9J_xO0KC-5q2nPjPUD9Q4M8uXy7yXOeNv_7KVPY9dFSBHEN8BMfD0dZXxDcnBEl40fT3jl0DFtDdiy6wq9oX5B8RjzxdaAWhoU7k53zGzMVY8CHkYQ88rvaQ=s1500-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3dyjnt8qsddhqPmHaWnmrKAGHzYplqZAJW3UrjR_S7H7HwgnveRyctEGgWlTGMNB-pvvLZKck2uSGY3RNqsu7XUCiIZaqhsBv1CEISpFyWxp0QNWqec0V2Zip1rfXc7VuRt5rB2N0bSe-qQDQ44IMpUdA=s1500-no?authuser=0"/>|

[Sketch Example](https://editor.p5js.org/kyeah/sketches/QTttEhIzb)

## Glitching with Randomness and Perlin Noise

So far, we've been using `random` to introduce variation into our sketches. 

```js
function draw() {
  let randomX = random(width)
  ...
}
```

Before we get into glitching, I'd like to introduce a similar tool which gives us a smoother and more predictable "randomness" – Perlin noise :)

Here's a series of x values from [0, width], calculated using `random` vs. `noise`:

![](https://lh3.googleusercontent.com/pw/ACtC-3fRi4pbuti0tfeus8rodAqZIS12iUdTVBrTbd9tg4n2G4y4H482MRZ-SqzWKjYY3Zxqru5l3XTcbKe0sxy8kIoi-gCiFNw8qu6yUORme2EGy9siXvC1unGsTEBVyaHVz1A9L0PoDSsuhYQGZTfk0XwfDg=s600-no)

Perlin noise provides the _feeling_ of randomness while changing smoothly over time. Perlin noise takes values from a smooth, predetermined graph (as seen in the visual.) We start at a random point in the graph and then move along it:

```js
// 1. Keep track of where we are on the predetermined graph
let noisePosition

function setup() {
  // 2. Start at a random position on the graph
  noisePosition = random(1)
}
 function draw() {
  // Grab a value from the graph and scale it to the canvas width
  let noiseX = width * noise(noisePosition)
  ...
  
  // Move forward slightly on the graph
  noisePosition += 0.01
}
```

As an example, maybe we start at `noisePosition = 0.05` and move in 0.01 increments to 0.60 and beyond:

![](https://lh3.googleusercontent.com/pw/ACtC-3cXmRWih45sbIRbh3k2_qlmFSEvm8bXuaYTGg_SLGXkwE_5ewtRMIEcE2NBq8qVm7taCrqzxLjVCJ-ha6j5wEG8x-vTH8qzu1nVVCC-CODLM6udkxRcwOh4bWW7dWXr-8xnVu5QSKUkcwx-b4ri6OOdvg=w1570-h408-no)

Depending on your visual goals, you can use random and noise in various ways to achieve different effects. I won't go too far into implementation details, but you can see the variety of use cases below when combining it with our previous extrusion examples!

|<img src="https://lh3.googleusercontent.com/pw/ACtC-3dKPEjoQ_uasK3R454rwb8ErmStQDHMAAMZ8ZZ9c9p7eziwL7BSpFxGGALcxnKdfDfhKK93jm8Qgvifv9ChUchzUAylIweIMDZvFpoVsR_4e3DjxB5fuQsiBd1PbwRPbXcQ7_9QbNEw5392cA6mxGI6-Q=s600-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3fWoyLDLNF5sTvXDQ_XFXh-PH9Nv9Zy4q-Oi3OYVl-Hsim2Nj3fZ_SWlUoh2z5OH5WkQ6ILnxrwMZx7DJy-Xb9PaB8HtvzfPOaSAl1y7oe0flaWYaNZz7IIBf2SrzDgk7b6QP3AglJW0ApNo8yT9yhVlg=s600-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3f0U1S3bXk2oDemhcAff3MQZFqk4IBpLcxEB6mrcq5O9xUXG7JRcEAK6cZj2IyoaTQTOnwmroJK2k7vYOpwUM-oX30pJLlq8xAjkKlviF9-B3bb-dulu1DfidcLTTGEtGj4rKtK8_AetIKpFOwlodzoVA=s600-no"/>|
|---|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3frqoGKYiKA0_Q02MeuXNrqCSx5GHWuAnqMVSnxTIb7nWC5LYeolbz8igS0WhudR94M1ChIgwQXbuDJUsPTy4JXWiNw0ZeKRnZjZU1g7hBY5XjjTsLvxB0fiFZyuQeGFaHWxHQmuYlqHWZcDph8RbVwxg=s600-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3dI9D4JTf7t4OZnK7PSxVIVa0zWrnFMeJ6wDrZ5Aj-lavx0NveQPRvpF1WyOj7j88jlEdRHZD0MG7nLjZi77TI6y_0gXEGCEAEFyaond8cb692Jd3fupwVhMunxIai7IeU0Rtv1id-oBPKaUP8cckDK4g=s600-no?authuser=0"/>|<img src=""/>|

## Shape-based Art

Another way to use textToPoints is to draw shapes using the provided points. For example, we can connect all of the given points using lines.

### Outlines with beginShape()

[Documentation](https://p5js.org/reference/#/p5/beginShape)

If you connect all the points from `textToPoints` using the shape methods, you can get something resembling your text!

```js
beginShape()
for (let pt of points) {
  vertex(pt.x, pt.y)
}
endShape(CLOSE)
```

![](https://lh3.googleusercontent.com/pw/ACtC-3cqWF1gZhdQIexyLvQkXW3RJVoqcw6Nzvk6WVSgNH0VbcOOYaJwUejXKSsAcIU_N2RE8b3ZkNb7H6rJSAx21T-xYVYV6W9zVmawnwYynv27cehN28skWFVEkfjO7gfXbMqUoHS9hSlDjSzckvyoiwR6jA=s1000-no)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/icQ4qSNX4)

### Cleaner Outlines

This is not too pretty, so let's try to remove the lines connecting our outline and counters (doughnut holes!) 

A quick and not-very flexible way of detecting when we've entered a new shape/counter is to see how far we are from the last point we drew. If we are sufficiently far enough, it's probably not part of the same shape/path.

```
let prevPt = points[0]

beginShape()
for (let pt of points) {
  // If we're far enough, close the current shape and begin a new one.
  if (ptDistance(prevPt, pt) > 20) {
    endShape(CLOSE)
    beginShape()
  }
}
endShape(CLOSE)
```

![](https://lh3.googleusercontent.com/pw/ACtC-3fqeAq-DvI--oRq3mYzlr86vdDpPPvPqrcKWjrGBLbPQpA_dP3pnwQbD9pxwYopuXWAlQIhDFC8vlaZo6YER7J1BvIu78-0s0AFHSh-okQkI1qixG4U72IvSnZAp-bvyG6PWof0Wi-l13je9_4JmzzGkg=s1000-no)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/Itx7Pmh5d)

### Fills and Contours

Instead of outlining our text, we can also fill it in! However, because doughnut holes are a thing, we have to use the [contours](https://p5js.org/reference/#/p5/beginContour) feature to cut holes in our shapes.

> NOTE! Contours do not work in WEBGL.

![](https://lh3.googleusercontent.com/pw/ACtC-3cKBN-jglj1WecIyrlUZl0mmeqC22zWzbzEO037hew3tBgoQ7GnL6IC_QF7hsq2TUPxHJlRBS1t380gzQ1hH1yGMRobN3cMbpj78tkA7Cs5FBsa6F7YAuK81-nKkP7ldQYdGKhuZoi5T0ceb5MX_0wZMw=s1000-no)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/HK6xb_H7t)

### Transformations and stretches

With connected shapes, you can more easily maintain the visual cohesiveness of text while transforming it drastically. Think about ways you can stretch and change various parts of your shape — for example, our original slit-scanning animation relies on drawing text shapes and stretching individual points when they move past an invisible line:

![](https://media.giphy.com/media/kaaDf1Yp2EI7mQo30g/giphy.gif)

## Working with Images: copy()

We can imagine doing all of these things using images as well! However, because images have a large number of pixels, it's better to think of them in terms of more mangeable units — rows, columns, or grid squares. 

For example, you can imagine swapping pixel rows or columns; changing rows into columns and vice cersa; nudging rows left or right when copying them; shuffling grid squares around...

For this, it's helpful to use the [copy](https://p5js.org/reference/#/p5/copy) function. This function copies a rectangular region from an image onto the canvas. This can be used to copy a row, column, or grid square.

For the purposes of this tutorial, let's look at how you might use this to read rows from top to bottom. Thinking like a scanner, we might read rows of height 5 (pixels) — and then we might think of a glitched effect, where the rows we read are shifted horizontally by a random number of pixels:

|Normal Copy|Shifted Copy|
|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3fvifCLeSvthSiGxcxWaNHaKeH3UT6DfIJpNetlsPuF-SBau1RolO7OGx7L3oXzjsstbYfNJTmI9j0OUvctmiLwXa9eAsyh-QilRkfBjY_47JehXlJouFRVyAhJ2SRmWH7ah689HVMyTaHGlk9CIj_0yw=s400-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3d0iXKB-cjNUZu6z78VHiNqxp2E0eMcGAIeRto8Tr6yMZNuYE98FUOHeLF0DmfoBaGgBIsoOzW7qlkqNT2HcYyXF2CZ2agjJvhXqr-BTVjdfQcr-DrMMVqB6mZn6KdHaA2eU5FyX53tC3d1i_ndbpe0DQ=s400-no"/>|

> Warning! Just like our video capture experiments, copy can be pretty performance-intensive. If you are running into slowdowns, try a lower frame rate, or creating a static image (add `noLoop()` to the bottom of your setup function.)

Here are two more examples using that same idea, but with a sine wave:

|rowSize = 1|rowSize = 5|
|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3f0qGMUVIofveiQreRrAXrHnQ9sEDXtiW4qneZlRST1E5LEVcfBvuCmE2AKOUuW5MlinAOTe8lQq4yZLxEBkp2jeZDCdLoJ0Q7t68zaqxCW2-SpH7HFX4bp00ZRjwsK53N4_dCSDRJHzoIb54kyvZMUwg=s800-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3eqvButegTzjTu3zsyUhgTCxujZEu2A37ZQMo29AD0x-j-BX0EfqIKRCvO4fP94djeCilQZE_NEJSCJnLmW37TtkAuQSIeYUJuBnxxoSqYvJMXD9Mgi9PyV347pSLSDRZ29VPcEex6MEbaenVtsoOPfTg=s800-no"/>|

There are a lot of other interesting ways to change your `copy` to make new, interesting things! You can skip certain rows in the destination image, or warp-scale the resulting image if the source width/height doesn't match the destination width/height. 

I recommend playing with different knobs and levers to see how it effects the final image:

|Skipping destination rows|Compaction Effect|x-nudging|
|---|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3fvtdqQnRN3cUqqmypUga89gAEftmGC1pw6BfCt6cE2fZWnNsGK-Ttt2G8m7jsYr6r6nGs5_aJZDm2EEOco159V0mtPXdJVwGig2YOiuyjd9KAUVWtNbU1L5BGFRPBj_nrpuEXn4zr3pthN4EiM5YTwUA=s800-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3evmZDNrYfTr1LNtxT-qEk2MFEc6umVbSUDfLEtNxEPNCZvnQR7P_BKv1o5LA-R_CwrEN3-SXuxpheW5OORsedn_TaaM0XOkFa62RcyHuoY5j9MB09eMJCVUjaxLhCM_LuLQM4PTrKrdJq-gScb-pI86Q=s800-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3cx5yE-XwtKZ-JNHzbv0HJi4ilhxuEVfyE24V0gI2O2JOaA9e5Mo_ymVMyeB9Gg1lIBDFgtuf97UF_8cyxMoHO001Ug6HhxXsjprVi5zTUlGVx6fq1zuZaLNG-yT0_NK9VDe3aCDlyBkKpipFkUcwGBpA=s800-no"/>

- [Sketch Example 1](https://editor.p5js.org/kyeah/sketches/y137XUugc)
- [Sketch Example 2](https://editor.p5js.org/kyeah/sketches/Gi1MViqEv)

Do you want things more cube-y? Curved? blinder'd? These are all attributes that you can affect using `copy`. And I've only covered one way to read images (row by row)! Try it out with columns or grid squares and see what you can make :)

|grids!|flowy grids!|
|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3cKAb_LxHO58CNkXlocmgIKbVS5gbMxcLVgr7lVclyrSeyMSdmYGoWD9j6sqtSi1_tZpmMLZNeEJLoD-jSjgoExhhZMF0TM7UG9yf37S_5SEWJbrhiFhCkm70NUT1srtZGWvECB0ob6kt78LfYIe3GzXw=s800-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3dLflumDnLl5fGelBpSwnbtqNfZpbYGlxWGq21v8-6AVqqR3JrDDT6GJPk1yvmOWL0nGJP294f8hj0UhBAv2XmiO61O9iR27I1ZamlkxxfY4nQba0JwKe-cFhK-kpwk6P2Jl3fYS9mtTUyLBxFIerEFjw=s800-no"/>|

## Color Glitching

One last thing! colors!

### Tweaking colors from original image 

If you have something more colorful, you can pull and tweak colors. For example:

```js
let pixelColor = image.get(x, y)
pixelColor[0] += random(25) // Add random value to the red color
pixelColor[2] += random(25) // Add random value to the green color
```

If you are copying pixels manually, you can set the fill directly before drawing a shape, or use the 
[tint](https://p5js.org/reference/#/p5/tint) function. If you are experimenting with overlaying shapes/images, take a look at [blendMode](https://p5js.org/reference/#/p5/blendMode).

### Adding colors

Even if your original image _doesn't_ have color, you can apply a tint to add color :)

In order to use `tint` with `copy`, you'll have to create new images before copying them to the canvas.

```js
// Draw image with a random red tint onto a new layer
let tintedLayer = createGraphics(width, height)
tintedLayer.tint(random(100, 255), 0, 0)
tintedLayer.image(img, 0, 0)

// Copy from the tinted layer instead of the original image
copy(tintedLayer, ...)
```

The examples below use the code snippet above within a for-loop in order to tint the image a different color every time we copy a row to the canvas:

|Red tints|Red or blue/green tints|
|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3d1py-pQL1F-jH2Kaeu6dTuSZyoGQyjGUG-MnEgRYTwQekE7xmkPedriOC3m5ABHSr5hg7xpS9X4OunKLmeEHibgdHWDGKQHx5WLh7U3mFdcVWmu1OPFtRPXiXv2qnCqWUgb5ZF5e3M7CCtwkz6q0COzw=s800-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3fnSRHdsi8bMND-QCy6clFSGP-KEA8Ix8OotyyPiqcca31AIohLNSpTzbsXhf-NUno5q4BOOt1dfgSRByc7WZ2zhtsi-DFgdnUBxTd6rCnVnhOx3mIcrtvg6uXwhdVJjD_JM9FBX969z_NMrGkI3rYMFg=s800-no?authuser"/>|

[Sketch Example](https://editor.p5js.org/kyeah/sketches/FHT2hTuwr)
