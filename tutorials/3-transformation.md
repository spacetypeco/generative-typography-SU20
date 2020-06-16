## Section 3. Transformation

Today we look at transformations through the lens of P5. ( :

- [Full sketch collection](https://editor.p5js.org/kyeah/collections/kgJDCjMtk)

## 1. Coordinate Systems

So far, we've been specifying coordinates for each shape, text, or image that we place on the screen. This has been working well, but will start to break down as we venture further. In order to build more dynamic and interesting pieces using complex transformations (rotation, scaling) and objects (text vector points), we'll need to understand the P5 coordinate system and how things are placed onto the canvas.

Let's start with a static sketch:

```js
text("B", width/2, height/2)
```

![](https://lh3.googleusercontent.com/pw/ACtC-3fp8T8myKAbJoYzIJwu4ljEO2Eb-QhI-DqnCmsKNnRqYvOIrRAvSNClv-KdrEulNhUwCq74lIlXQ_1hV1BYdvdZsKkWW7kZEyF9giNm3jPqeeAo3CvovEnP7xKRZk30zezJtK2snMXnzGlzLottEUQP1w=s1422-no)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/wysIBbYoX)

Let's say that we want to rotate this character in place. We might try something like this:

```js
rotate(rotateAngle())
text("B", width/2, height/2)
```

[Sketch Example](https://editor.p5js.org/kyeah/sketches/7uS-X-pBm)

Which works pretty well! ...except that it doesn't really look like what we intended. In particular, instead of rotating in place, the letter is swinging like a pendulum:

![](https://lh3.googleusercontent.com/pw/ACtC-3c5GDzhaMNEdFDCAv9iziqaaH6KZAkG0TD5WEFGJTv5d8jVbWhSJJV3D3VuJGbaVnaJJxhF5LlTifZWxoICGSBzMttH1APvNz3DzE6OGXRczZFPIeFIyBNyl1xc-LSVh78Qxgp6ILzzrtcOkho4mZzrnw=s500-no?authuser=0)

While the visual above is helpful in understanding how the text is moving through the canvas, it makes it really hard to understand where shapes and components will go. We asked the text to be placed at position `(width/2, height/2)`, but based on our previous animation, it's completely different!

Instead, consider the coordinate system as a piece of graph paper. P5 _did_ place the text at, say, (200, 200), but we've moved the graph paper for convenience. Instead of thinking of the _text_ as rotating, we should think of the _graph paper_ rotating:

![](https://lh3.googleusercontent.com/pw/ACtC-3eLjvAZXAKCF55Cv2WNa7MzC7SoJIpKxAuK3CuryMKiy_imaK7pb1n-G6NoRSEPbBz4oJySpERFv5uCWcF71CXoQb9ZGVeAAkmAwYvipHXZCcgFKU0CFoN_-TTRos44hv8XolXBf8xcN4-5ePWVBbJGbg=s500-no)

With our grid lines properly rotated, we can see exactly where the text will be placed! Now we can tease out the next step for rotating the "B". Since all rotations happen around the origin, we need to move our origin closer and draw our "B" around that point.

```js
// 1. Move the coordinate system to the center of the canvas
translate(width/2, height/2)

// 2. Rotate the coordinate system
rotate(rotateAngle())

// 3. Draw our text at the origin :)
text("B", 0, 0)
```

![](https://lh3.googleusercontent.com/pw/ACtC-3e3lZIRiEt9ahQuj8WKEGpgQhvj6hXPTjz6s8UDehhAjEme0kPV82xs2MQl4687hU8H_wprpZyp2-fh-XteLwArE8KQf5nDMugKQwCVo5pZq97pHYxun_ORYYSMyVoIEoU6BcnoVyyZtrgAv5K_ElAvTg=s500-no)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/yKjh1DH1_)

Success!

Of course, with anything artistic, you may decide that you do want to rotate your objects in different ways — perhaps to mimic a pendulum, or a letter "pinned" on the wall at a particular location. Have fun with it and get creative!

## 2. Multiple Objects

Let's talk about multiple objects now. Imagine adding a second "B" to the composition, which swings the opposite way. With one coordinate system, you might imagine doing something like this:

```js
translate(width/2, height/2)
rotate(rotateAngle())
text("B", 0, 0)

// Rotate twice as far the other way - once to get the coordinate system
// to normal, and again to swing the other way.
rotate(-2*rotateAngle())
text("B", 0, 0)
```

While this works, we have to do more math! And this is only with two pieces of text, at the same location. What happens with 500? What if they are laid out at random on the canvas? We'd have to tally and sum up all of our 500+ transformations. That's a lot of mental juggling ( :

Instead, we can automatically "reset" the canvas using push and pop. I like to think of these functions as new pieces of graph paper; or perhaps as coordinate system "layers" being added and removed.

```js
translate(width/2, height/2)

push()
rotate(rotateAngle())
text("B", 0, 0)
pop()

push()
rotate(-rotateAngle())
text("B", 0, 0)
pop()
```

![](https://lh3.googleusercontent.com/pw/ACtC-3eCtSUdtmJn9LCr-5530VVothBreR2f_e0Pd1NNhxKzi9uYAEX2YQgp1_HSTJL4LbvCxey0UpF26vFiyheTyowVZHRXrKo4_TP67dtgLql2Rhn6nZMmrDrewm8685TzThxBChfr6pxT6_XcPbYC32MkVg=w499-h500-no)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/mDJ4aQgnu)

## 3. Shearing and Scaling

So far we've talked about translation and rotation. Shearing and scaling are also fun transformation tools! These work just like the other two, except they stretch and manipulate our graph paper in physically-impossible ways:

![](https://lh3.googleusercontent.com/pw/ACtC-3fjVwkXDkw2FTN0Cosc9IxbdDBVpMTFkuhlR-Gj4n6LNm3mIxqUqil5qaKEZiJpgvRbhFROyMQh4fxi2CD9eha9XXCq_TGg2X4ZuUdaf1cCcGHoG90tyE3Jx_Z7HpSIoi_HXCxzYylN1N0_efuoaj-Gxg=s500-no)

[Sketch Example](https://editor.p5js.org/kyeah/sketches/79eIFqKua)
[Animated Sketch Example](https://editor.p5js.org/kyeah/sketches/ihtNX0LB8)

## 4. Layouts using map()

When you have a particular layout in mind, combining these transformations with an easing tool may help you lay things out. `map()` is an excellent, general use tool for doing this!

`map()` is a function to go from one set of boundaries to another. For example, if you want to go from [0, 10] to [0, 100] with a value of 3, you can use:

```js
let mappedValue = map(3, 0, 10, 0, 100)
// mappedValue === 30
```

![](https://lh3.googleusercontent.com/pw/ACtC-3d686uUzdCq8IR6I05hbTi7hv6LDfx05-xguz-QtUaLPjymqaw3jpby3mq8r5o0yaftUceuRUosh6YKLxes2_S9jdUXRImxHC9SECY_lhIiBa7XggOhAoV2xHth1uYjufcErK9zxxOBMMdrMqdZjIyccg=w1307-h563-no)

We've been able to get by without this by doing our own math (think: `column * xStep`), but with rotations it gets pretty confusing. `map()` is a much more accessible way to lay things out evenly.

Consider a "rainbow" of characters rotating around the origin:

|image|visual|
|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3eZ8AO32ItzCnK5R04_lqoO7YwYbzkEbSSIvtM-OsH3TDMEpE18okncN046eTdHzLtgD9GKrW9jd1QkHOnxHDsLSv93upSNQGG242qxgjAtxFp4s7mUReMRdEF0B6emGey6uuVRMdsAeSqD9Dc9xCy7Ng=w499-h498-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3dPYa2XLQGgzRazNMZBJRXS9fVauL82bky8a_GOZ8bD0yXK5opE3AFdQResl6jfG-CdzFeJcwWRSZ_fDh5CwdNs3tupRrcr4jq6fjCHpq9sIaraW-BwK3Nw_LPGrRnCS8SJyT366-Z01XBOkHae9ndZUg=w496-h497-no?"/>|

[Sketch Example](https://editor.p5js.org/kyeah/sketches/C3WmMxlKD)

Each character gets it's own rotation angle from -45 to 45 degrees:

```
for (let c = 0; c < 10; c += 1) {
  let angle = map(c, 0, 9, -45, 45)
  ...
}
```

Visually, it may look like this:

![](https://lh3.googleusercontent.com/pw/ACtC-3fyjEQ14qQDr_i_SV4JKuiODiBehswDP83USktJtPXHrRhaV0j94l-U7LE9tzBwCuXWutiqp6CJs2s1gqUyHDWro8NuMQFXyqTLF0DRg-ElxQL5xbJnbdwe4Lpv4mB-4DZDILRrnNiSDzy9iI6kF8WNDw=w1297-h716-no)

In the following example, we use `map` twice to calculate both an X coordinate and a rotation based on which letter we're on:

|image|visual|
|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3dVTD1VwsLr0_CpQKQEUrdULd2pEvzEFHcl4DlNgdupaDPRYUc0p4Il7OelIuN5OGLQQWWftZpu4wtRKjHsxJAiYxS25gfUkDyTfxaK42FJHEAATHgoNYx-rYtno9GOJ2KqoVohj_44lTJ3a6J3XrRy3A=w493-h495-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3fzCSxAjUwRlragRZgyVzGgKPCwAP3CoU6r8S1NdQe8DkEEZgQ7gshEfqeo6P7Eues90cQ5SxavfRUTDmUrXjFutkKrTnsY6vYpDWMmhVlGU7qfSZHTY3i2vMY3EQB3mVF3sdzHVCn7TYme4wD9Z3USMQ=w544-h542-no?authuser=0"/>|

[Sketch Example](https://editor.p5js.org/kyeah/sketches/mKa8letYB)

## 5. WebGL and 3D

So far in P5, we've mostly discussed 2D compositions. With transformation, many things open up if we add the third dimension, so let's finish by adding some depth!

By enabling WEBGL when we create the canvas, we can use 3D graphics functions:

```js
createCanvas(windowWidth, windowHeight, WEBGL)
```

However, note that the default coordinate system is _different_ from the normal P5 system. WebGL canvasses automatically move the coordinate system to the center, so **you do not have to translate the coordinate system**.

|Default P5 Grid|Default P5 WebGL Grid|
|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3erTTm7NcGV8y6GyIcxrnigYG3YaWqqodrRfq_b9MEOiJRfow82F_ci919zNYhi_PwYFT_uXYUxDKhaypJr4MRRWdHf1lv7bxvx9LMy7wNLR-FzsnUWWCQxr5vG0xZBgmmutiO6Q7gNqFSaGARn5MGDiQ=s500-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3c4Y2Efqgi-UZVjwfz1dHnIZYbYCHl29zmzsw5m3XwkRJgODEpS8q8ReEt5Z4Yt7xekN5JgR9AP9a9LbwJV-6_LN6LNDS_wKs6lD5j8Io3qR5EcHn8HrF-xUylS6Krr7WCeolAJfMTjFnO44xtXlpC46g=s500-no"/>|

With WebGL enabled, we can utilize new 3D functions! You can use things like boxes, cylinders, and spheres. I highly recommend looking at these [3D primitive shapes](https://p5js.org/examples/3d-geometries.html).

Here, we're interested in the new 3D transformations that we can do. The translate function now takes an optional 3rd parameter:

```
// Translate 50 pixels back in the Z direction
translate(0, 0, -50)
```

and there are new rotate functions:

```
// Rotate 15 degrees around the X axis
rotateX(radians(15))

// Rotate 15 degrees around the Y axis
rotateY(radians(15))

// Rotate 15 degrees around the Z axis (the normal rotation)
rotateZ(radians(15))
```

[Sketch Example](https://editor.p5js.org/kyeah/sketches/0_NcDG7OX)

## 5b. WebGL and Transparency

Danger!! If you are drawing with transparency and depth, you need to draw objects from back to front. Otherwise, the transparency will not work correctly. Here's an example of two "B"s, one at `z = -50` (blue) and one at `z = 0` (orange):

|Back to Front <br> (blue drawn first) |Front to Back <br> (blue drawn last)|
|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3dqhV9LiVzsBepUao25xOsBNzlxiMgWjiORiJaECuopqNEkKoaV-Zo3bOpUCkIl9a-qhRccwLzw8f462-IWay-WYE1Jg0n6S5CtbAXVoRnAjGue4or30ghfU5hq1AGD9Ac19QnHlrtrcVwTD9hfAl5m2A=w1002-h998-no?authuser=0"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3dnnnFkFEOSCtZOeqNRt6LxHVvbLQ5AtegDWxHRi1zUcKZFdBZ0fTtBuXAknz1jVB3qxhbKvxKtWxUMdSPj7jWtu_0a9h6JxGk-91VVfWnZ8JA0F9v03nk9-0Z9jr5Q50OFUHFoEHyUodUrD5VECCHiag=w996-h1002-no?authuser=0"/>|

For those interested in animation, this means that you cannot rotate a scene fully around 360 degrees without having to swap the order of items being drawn. An example of the two B's being rotated counter-clockwise:

|Frame 100 <br> (Viewing from the "front") |Frame 200 <br> (Viewing from the "back")|
|---|---|
|<img src="https://lh3.googleusercontent.com/pw/ACtC-3eZdI6Y9PcFwCa3yT0bSNeLLT-FUJV3laGrxtlDm4QF1jJuxwTfrSSjNNSO4LTrnIuv6kDMEVjlcu9IXKd2GFoUXn6Q86M_-uIlIhzGUDHVrG8Alz2rMZV7CEGScfwr-pq_puHE1zIsJkFGEztg-QxhIg=w1000-h1002-no"/>|<img src="https://lh3.googleusercontent.com/pw/ACtC-3ddMT_vANRBp12qFqe5X_RvGYlUcImgRtW-SyjxejATX4qDeTlFKbIiOL-OfQVuji5Ts5UWLs46PMCoMbrcpLSE_M8n50qj_XbbuEWRQCFdXjNffsIf7kY9VTjMGW3SiE7WEn5Nb7RJzE3HGTz0JFO9nw=s994-no"/>|
