---
layout: post
title: "Playing with Snappy Graphics"
date: 2014-03-04 08:26:55 -0600
comments: true
categories: 
---

Working with SVGs
-----------------

So it's been a few weeks since I have been able to sit down to work on my blog...but my brain is so full of new ideas that I just have to share a few of the things I've been learning.

I'm really enjoying working with JavaScript. It's a very powerful language all on its own but when you add a few libraries in, Javascript becomes the all powerful, all knowing wizard of Oz. 

What's a JavaScript library? Glad you asked. Libraries are collections of pre-written JavaScript code which you can use to help make your awesome stuff more awesome. Libraries can help you accomplish a wide range of tasks from building forms to animation, from building graphs to displaying interactive maps and more. There are hundreds of libraries - with more being written each day. A great resource to look at is [JSDB](http://www.jsdb.io/) which has different category lists of JavaScript libraries.

My favorite library is definitely JQuery because it makes working with the DOM (the Document Object Model, i.e. the actual elements on your webpage) so easy. However, in the last two weeks, I've been spending more and more time playing with SVG libraries.

SVG? Gotta love those acronyms! SVG stand for scalable vector graphics. That cleared it up for you, right? Ha. Let's break it down. SVGs are like special images (*graphics*) that can shrink and expand (*scalable*) and don't pixelate (they don't get blurry, i.e *vectors*). You may have created SVGs in programs like Adobe Illustrator or the open-source Inkscape, but did you know you can make SVGs by writing actual code? Take a look below.

```html
<svg height="100" width="100">
  <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />
  Sorry, your browser does not support inline SVG.  
</svg> 
```

So what's happening in this snippet? We define an `<svg>` element that is 100 pixels wide and 100 pixels tall. Then we create a circle with a center point of (50, 50) and a radius of 40 pixels. We also add a message for the poor souls who are using antiquated browsers - telling them what they are missing out on. And the result...

<p data-height="268" data-theme-id="0" data-slug-hash="yoEKh" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/drewbie/pen/yoEKh'>yoEKh</a> by Drew Robinson (<a href='http://codepen.io/drewbie'>@drewbie</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

I know what you are thinking...That's a lot of code for a little circle. What if I wanted multiple circles?? Which brings me to my original topic. JavaScript libraries. There are actually JS libraries that help you easily create SVG elements! Let's look at one.

![Snap SVG](images/snap-svg.png)

[Snap SVG](http://snapsvg.io/) is a great library that easy enough for beginners to use and powerful enough for advanced developers to enjoy.

Getting started is a snap. Download the code from the site and set up your index.html page with a script tag in the body of your html.

`<script type="text/javascript" src="lib/snap.svg-min.js"></script>`

Next add an SVG element to your html. You can also create it with JavaScript if you prefer.

`<svg id="svg1" width="400" height="400"></svg>`

In your JavaScript file, add the following code:

```js
var canvas = Snap('#svg1');
var smallCircle = canvas.circle(50, 50, 40);
smallCircle.attr({
    fill: "red",
    stroke: "#000",
    strokeWidth: 5
});
```

<p data-height="268" data-theme-id="0" data-slug-hash="wvbgp" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/drewbie/pen/wvbgp'>wvbgp</a> by Drew Robinson (<a href='http://codepen.io/drewbie'>@drewbie</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

Cool, right? Snap makes SVGs easier and your code more readable. We create a circle with `canvas.circle(x, y, r)` and then we change the circle's attributes with `.attr()`. 

Okay, so, we created a circle again. What makes using JavaScript any better than just writing it out with HTML?

Well, for starters, you have a lot more options with what you can do with the circle. How about moving the circle around? Totally doable.

Add the following line below your circle code:

`smallCircle.drag();`

Now trying dragging the circle around the screen. Nice, huh? You can also try it out in the embedded codepen above on the page.

Still not convinced? You want more? Alright, check this out.

<p data-height="268" data-theme-id="0" data-slug-hash="AqiBE" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/drewbie/pen/AqiBE'>AqiBE</a> by Drew Robinson (<a href='http://codepen.io/drewbie'>@drewbie</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

Such pretty colors! Try dragging the circles around. Fun! Re-run the codepen and you get more pretty colors! Check out the code...

```js
var canvas = Snap('#svg1');

var randInt = function(minVal, maxVal) {
  var rInt = minVal + (Math.random() * (maxVal - minVal));
  return Math.round(rInt);
}

for (var i = 50; i < 400; i += 100) {
  var red = randInt(0, 255);
  var green = randInt(0, 255);
  var blue = randInt(0, 255);
  
  var circle = canvas.circle(i, 50, 50);
  
  circle.drag();
  
  circle.attr('fill', 'rgb(' + red +', ' + green + ', ' + blue + ')')
}
```

I use a loop to create multiple circles dynamically with Javascript. I changed the x value of each circle I created so that it doesn't sit on top of its neighbor. If I was using HTML, I would have to write out the circle code four times. Using JavaScript and Snap SVG, I can simply use a for loop. 

And the pretty random colors? I use `randInt()` to create random numbers. The function takes a range of numbers and returns a random number in that range. Then I use the fill attribute to create a random rgb color to apply to each circle created in the loop.

Snap does a lot more than just pretty circles. If you're interested, I encourage you to check out the documentation and start experimenting. I've embarked on a project to build the game Battleship, ehm, I mean BattleDrones, and I'm using Snap to create my game boards and ships. Snap is making things a lot easier...it has ways of checking if elements are overlapping and other helpful methods that make a game easier to design.

I'll be writing about my experience creating Battleship soon, so stay tuned!

![cute cat picture](images/catRoll.jpg)


