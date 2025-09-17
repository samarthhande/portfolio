---

toc: False
layout: post
title: Snake Blog
description: A writeup of our changes to snake
categories: ['Web Development', 'Hacks']
permalink: /snake/writeup
breadcrumb: true
---


This is the original snake game (<https://pages.opencodingsociety.com/snake>): 


![Alt text]({{site.baseurl}}/images/posts/oldsnake.png "Image of old snake game")

This is our new and improved version (<https://compsciteam.github.io/student/snake>)

![Alt text]({{site.baseurl}}/images/posts/newsnake.png "Image of old snake game")


Additionally, we make a new and improved settings screen:

![Alt text]({{site.baseurl}}/images/posts/newsnakesettings.png "Image of improved settings screen")

<h6>(Note: This image is outdated and needs to be replaced to include the Fruit Frenzy option)</h6>

<hr>

### Our changes

We have made multiple quality-of-life improvements and bugfixes for Snake. Below is a list of all the modifications we made.

- Changed "Points: ___" to "Apples: ___"
- Added WASD controls
  - Arrow keys could cause the page to scroll up and down. Adding WASD controls allowed for increased user flexibility and prevents this scrolling issue.
- Changed background theme
  - The background is, by default, checkered light and dark green. We added different color schemes to add even more flexibility, including light theme, dark theme, and **colorblind** mode.
- Added two new game modes!
  - We called the first game mode "Candied Apples", where the snake exponentially speeds up every time it eats an apple.
  - The second one is called "Fruit Frenzy", where a golden apple that bounces around can be eaten for 3 points. It has a 40% chance of spawning when you eat an apple.
- We made the snake blue and the apple red
- Cleaned up the settings UI significantly :)
  - See the above image
- Added two more game speeds
  - Honestly, the "Troll" and "Turtle" game modes were mainly just trolling. But if you're feeling daring, try to get a single apple at "Troll" speed with walls enabled >:)
  


## Play our new-and-improved snake game at [compsciteam.github.io/student/snake](https://compsciteam.github.io/student/snake)!

Made by Subteam 1: Anish Gupta, James Blazek, and Vihaan Budharaja (and Samarth Hande before he left us for the dark side, Subteam 2 🫡)
