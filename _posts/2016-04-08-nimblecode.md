---
layout: post
title: nimblecode - How fast can you code?
description: Code typing racing game with singleplayer and live multiplayer modes. Built in React/Redux.
keywords: projects, single page application, React, Redux, React-router, socket.io, Ace Editor, Node, Express, MySQL, Bootstrap, Google Material UI, nimblecode, nimblecode.io, coding, typing, racing, code typing, ghost replay
---

Nimblecode is a code type-racing game with singleplayer and live multiplayer modes. It was built using React.

The idea of nimblecode was borne out of an afternoon when I was playing <a href="http://play.typeracer.com/">typeracer</a> with a few other developers. I thought to myself: "What if we built out a game, like typeracer, but instead of typing out passages, we typed out code?" If it could be made into a competitive game, it could be educational and fun at the same time. That day, <a href="http://nimblecode.io">nimblecode</a> was born.

<div id="container">
  <iframe width="100%" height="315" src="https://www.youtube.com/embed/hVtSYTtRgIg" frameborder="0" allowfullscreen> </iframe>
</div>
<br>

<h5>Fun and Educational</h5>

These principles guided our development. The best way we could provide value to our users would be to make the game fun and educational. So we got to it.

These are some of our early mockups, prepared by one of our developers, <a href="http://www.rickyeh.com">Rick</a>.

![Mockup1](images/nimblecode/Cogile_Mockups_1.jpg "Mockup1")
![Mockup2](images/nimblecode/Cogile_Mockups_2.jpg "Mockup2")
![Mockup3](images/nimblecode/Cogile_Mockups_3.jpg "Mockup3")

<h5>Simple and Easy</h5>

I wanted to keep it as simple and as easy as possible for a user to type out code. Repetition is a powerful way to learn something, and one of the ways our users can benefit from nimblecode is the practice of typing out code and the practice of typing out different types of code snippets, whether it be JavaScript ES6 syntax, or a function in Python. This exposure to different languages and code snippets is highly valuable to any budding developer.

Simple and easy, educational and fun were the goals. And we achieved those goals with nimblecode.

<h5>Features</h5>

We split up the game modes into a singleplayer mode and a live multiplayer mode. One of the most interesting features we were able to implement was a ghost replay feature for singleplayer. The high score of each level is stored in our database, and the actual typing replay is also stored. This ghost replay is played back while a user is playing to provide a competitive aspect to the singleplayer mode.

![Singleplayer](images/nimblecode/nimblecode-single.gif "Singleplayer")

We also implemented a live multiplayer mode where users can play random levels and race against each other. Our team leveraged web sockets for real-time, bidirectional communication between all users in multiplayer mode.

![Multiplayer](images/nimblecode/nimblecode-multi.gif "Multiplayer")

As you can see in the gif above, the multiplayer mode has progress bars and code editor mini-views to visually display all the participants' progress.

<h5>Tech Stack</h5>

nimblecode was built using React and Redux. React was great to work with because of the reusable components. While we didn't employ Redux at the very beginning, once our app grew more and more complex, it became a huge help to implement Redux to manage our application state. 

Some of the other technologies we used were:
 
* React
* Redux
* Node
* Express
* MySQL
* socket.io
* Ace Editor

Take a shot at the high scores at <a href="http://nimblecode.io">nimblecode.io</a> or have a look at our <a href="https://github.com/nimblecode/nimblecode">repository</a>.
