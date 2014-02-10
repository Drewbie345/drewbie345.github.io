---
layout: post
title: "Week 1 Wrap-Up!"
date: 2014-02-09 20:06:54 -0600
comments: true
categories: 
---
Rock, Paper, Scissors
-------------

It's Sunday evening and I'm finally getting to sit down and really reflect on my first week at school. And my first thought is...has it really been just a week??

So this weekend, I decided to try my hand at building a simple Rock, Paper, Scissors game. Inspired by an Ruby assignment earlier this week, I thought it would be fun to make a site where you could visually see and play Rock, Paper, Scissors. 

I also thought it would be a good review of the basics of JavaScript, since we'll be starting on that this week.

The code below is actually the fourth revision and I'm still not totally happy with it. But I figure as I get better in JavaScript, I'll be able to improve the code.

![RPS Webpage](images/rps.png)

If you want to try the game out yourself: [Rock, Paper, Scissors](http://fatcatstudio.org/rps/).

We'll start with the HTML for the game. I used the [Foundation CSS framework](http://foundation.zurb.com) to layout my page and JavaScript and [JQuery](http://jquery.com/) to create the game. I've included the head of my HTML document so that you can get a better idea of what I needed to include to get it to work.

Tip: Make sure your JQuery file is above your script.js file in `<head></head>`, otherwise your page can act up.

```html
<head>
   <meta charset="UTF-8">
   <title>Rock Paper Scissors</title>
   <link rel="stylesheet" href="normalize.css">
   <link rel="stylesheet" href="foundation.css">
   <link rel="stylesheet" type="text/css" href="style2.css">
   <script type="text/javascript" src="jquery.js"></script>
   <script type="text/javascript" src="modernizr.js"></script>
   <script type="text/javascript" src="script3.js"></script>
</head>
```

Among other things, Foundation makes it super easy to set up a simple grid layout it just a few minutes. Having, in the past, written my CSS from scratch, it has taken a little time for me to let go of my control issues regarding this (well, I'm *trying* to work on it!).

I won't post the whole code here (feel free to take a look at the code on [GitHub](https://github.com/Drewbie345/rockpaperscissors)). However, to give you an idea of how I named stuff, here's a snippet.

```html
<div class="large-6 columns">
  	<div class="row">
		<div id="player-game-pieces" class="row bump">
			<div class="small-2 large-4 columns">
	  			<a id="rock" href="#">
		  			<img id="rock1-p" src="rock.gif" />
				</a>
	  		</div>
	 		<div class="small-4 large-4 columns">
	 			<a id="paper" href="#">
		 			<img id="paper1-p" src="paper.png" />
	 			</a>
	 		</div>
	  		<div class="small-6 large-4 columns">
	  			<a id="scissors" href="#">
		  			<img id="scissors1-p" src="scissors.png" />
	  			</a>
	  		</div>
		</div>
	</div>
</div>
```

So, personally, I find it easier when I'm using JQuery to give the important stuff meaningful ids. As you can see each image is contained within a link that goes nowhere but each image also gets an id so that later on I can add a border around the picture picked by the player. The code for the computer is similar except there's no need for the links since the computer's choice is artificially created.

I did minimal styling but there are a couple of things I wanted to point out. There were a couple of `<div>` elements that I didn't want to display immediately when the page loaded...so I simple set `display: none`. Later on, with JavaScript, I will make them appear when they're needed.

```html
<div id="display-winner" class="row">
  	<div class="small-6 large-centered columns">
  		<div id="who-won">Placeholder Text!</div>
  			<a id="play-again" href="#" class="button">Play Again?</a>
  	</div>
</div>
```

```css
#display-winner {
	display: none;
}
```

Okay, now for a little JavaScript and JQuery. I started out by simply mapping out what I wanted my program to do. In my `script.js` file, I commented out each step of the game.

```js
// Player picks rock, paper, or scissors by clicking on that image.
// Whatever image she picks, add a border around it to denote its selection.
// Disable other image links.
// Computer picks choice and border is added to computer's choice.
// Player's choice and Computer's choice are compared.
// Based on rules of game, determine a winner and display it.
// Invite to play again, clear image borders and renable links.
```

I ended up deciding to create a player Object and a computer Object to hold my values. I also gave myself the ability to change the value with a method built into the Objects. (Sadly enough, this didn't occur to me until the fourth revision). Note: an Object is simply a special type of data with properties and methods. Properties are the values associated with an Object and methods are  actions that can be used on Objects.

```js
var player = new Object();
player.pick = "";
player.changePick = function(choice) {
	player.pick = choice;
}

var computer = new Object();
computer.pick = "";
computer.changePick = function(choice) {
	computer.pick = choice;
}
```

Next I created two functions `updateComputer()` and `updatePlayer()` to change the values in my player and computer Objects. The `updatePlayer()` was really straightforward but the `updateComputer()` uses another function I created called `computerInput()` which randomly picks a value.

```js
function updateComputer() {
	var ai = computerInput();
	computer.changePick(ai);
}

var computerInput = function() {
	var randomNumber = randInt(1, 3);
	var computerChoice = "";

	switch(randomNumber) {
		case 1:
			$('#rock1-c').removeClass('border-c')
			computerChoice = "rock";
			$('#rock1-c').addClass('border-c')
			break;
		case 2:
			$('#paper1-c').removeClass('border-c')
			computerChoice = "paper";
			$('#paper1-c').addClass('border-c')
			break;
		case 3:
			$('#scissors1-c').removeClass('border-c')
			computerChoice = "scissors";
			$('#scissors1-c').addClass('border-c')
			break;
	}
	return computerChoice;
}

function randInt(minVal, maxVal) {
	var rInt = minVal + (Math.random() * (maxVal - minVal));
	return Math.round(rInt);	
}
``` 

Originally I used a if/else if/else statement but *switched* to a switch statement (I amuse myself). I'm actually thinking about switching back to if/else if statement because I want to try not rounding my random number and see if I get more variety in the game that way (a problem for another day!). FYI: You can't use logical operators in a switch statement because a switch works by comparing what is in switch() to each case.

Up next is a `determineWinner()` function that actually does the comparing between the player's choice and the computer's choice. 

```js
var determineWinner = function() {
	var play = player.pick;
	var comp = computer.pick;
	var winner;

	if (play === comp) {
		winner = "tie";
	} else {
		if (comp === "rock" && play === "paper") {
			winner = "player";
		} else if (comp === "rock" && play === "scissors") {
			winner = "computer";
		} else if (comp === "paper" && play === "rock") {
			winner = "computer";
		} else if (comp === "paper" && play === "scissors") {
			winner = "player";
		} else if (comp === "scissors" && play === "rock") {
			winner = "player";
		} else if (comp === "scissors" && play === "paper") {
			winner = "computer";
		} else {
			return "Error!";
		}
	}

	if (winner === "player" || winner === "computer") {
		$('#who-won').removeClass('again');
		$('#who-won').text("The " + winner + " wins!");
		$('#display-winner').show();
	} else {
		$('#who-won').removeClass('again');
		$('#who-won').text("A Tie!");
		$('#display-winner').show();
	}
}
```

It's really just a big if/else if statement. First, it checks to see if there is a tie and if there's not a tie then it works it's way down the list of possibilities until it finds one that matches. I'm definitely interested in seeing if there are other ways to do this comparison but this is what I came up with for now. The last bit of this function actually displays the winner on the page. You'll see I used JQuery for this and removed classes and changed text as necessary.

I wrote a couple of smaller functions to help me out with the image links.

```js
function disableButtons() {
	$('#rock, #paper, #scissors').addClass('pick-choice');
	$('#dare').hide();
}

function enableButtons() {
	$('#rock, #paper, #scissors').removeClass('pick-choice');
}

function clearButtons() {
	$('#rock1-p, #paper1-p, #scissors1-p').removeClass('border-p');
	$('#rock1-c, #paper1-c, #scissors1-c').removeClass('border-c');
}
```

Finally I was ready to perform some magic (not really but sometimes it feels like magic!).

```js
$(document).ready(function() {
	$('#rock').click(function(event){
		updatePlayer("rock");
		$('#rock1-p').addClass('border-p');
		disableButtons();
		updateComputer();
		determineWinner();
		event.preventDefault();
	});

	$('#paper').click(function(event){
		updatePlayer("paper");
		$('#paper1-p').addClass('border-p');
		disableButtons();
		updateComputer();
		determineWinner();
		event.preventDefault();
	});

	$('#scissors').click(function(event){
		updatePlayer("scissors");
		$('#scissors1-p').addClass('border-p');
		disableButtons();
		updateComputer();
		determineWinner();
		event.preventDefault();
	});

	$('#play-again').click(function(event) {
		clearButtons();
		enableButtons();
		$('#who-won').addClass('again').text("Pick Again!");
		event.preventDefault();
	});
});
``` 

Based on which button the player picks, a series of events occur. I know what you're thinking. There's this whole concept of DRY (Don't Repeat Yourself) in programming...and you're right. I'm absolutely positive there's a better way of doing it - I just couldn't see a solution for it today. Maybe tomorrow.

The cool thing about the program is that I reset the game without refreshing the page so that means there's a lot of potential for going further with the program. I'm seeing a score tracker in the future!

All in all, this was a very fun way to review JavaScript basics and I really like the end product. But, as always, I've got plans for improvement! I'll keep you updated on my revisions.

For now, I must go to sleep. Tomorrow is the start of a new week!

![Cute Dog Picture](images/dog.jpg)