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

<hr>

**Changed "Points: ___" to "Apples: ___"**
<details>
<summary>Show code</summary>

```html
<p class="fs-4">Apples: <span id="score_value">0</span></p>
```

</details>

<hr>

**Added WASD controls**
  - Arrow keys could cause the page to scroll up and down. Adding WASD controls allowed for increased user flexibility and prevents this scrolling issue.

<details>
<summary>Show code</summary>

```js
let changeDir = function(key){
    switch(key) {
        case 37: case 65: // left arrow / 'A'
            if (snake_dir !== 1) snake_next_dir = 3;
            break;
        case 38: case 87: // up arrow / 'W'
            if (snake_dir !== 2) snake_next_dir = 0;
            break;
        case 39: case 68: // right arrow / 'D'
            if (snake_dir !== 3) snake_next_dir = 1;
            break;
        case 40: case 83: // down arrow / 'S'
            if (snake_dir !== 0) snake_next_dir = 2;
            break;
    }
}
canvas.onkeydown = function(evt) {
    changeDir(evt.keyCode);
}
```
</details>

<hr>


**Changed background theme**
  - The background is, by default, checkered light and dark green. We added different color schemes to add even more flexibility, including light theme, dark theme, and **colorblind** mode.


<details>
<summary>Show code</summary>

```js
function applyMode(mode){
    switch(mode){
        case 'colorblind':
            color_light_tile = '#ffd97a'; color_dark_tile  = '#ffd15a';
            color_snake = '#0000ff'; color_apple = '#ff00ff';
            break;
        case 'light':
            color_light_tile = '#f0f8e8'; color_dark_tile  = '#dfeccf';
            color_snake = '#0b63d6'; color_apple = '#d32f2f';
            break;
        case 'dark':
            color_light_tile = '#355a2b'; color_dark_tile  = '#243b1b';
            color_snake = '#1e90ff'; color_apple = '#ff6b6b';
            break;
        default:
            color_light_tile = '#a9d750'; color_dark_tile  = '#a2d148';
            color_snake = '#2f00ffff'; color_apple = '#ff0000ff';
    }
    // repaint if playing
    if(snake && snake.length){
        for(let y = 0; y < canvas.height / BLOCK; y++) {
            for(let x = 0; x < canvas.width / BLOCK; x++) {
                ctx.fillStyle = ((x + y) % 2 === 0) ? color_light_tile : color_dark_tile;
                ctx.fillRect(x * BLOCK, y * BLOCK, BLOCK, BLOCK);
            }
        }
        for(let i = 0; i < snake.length; i++) activeDot(snake[i].x, snake[i].y);
        activeApple(food.x, food.y);
    }
}
</details>

<hr>


**Added two new game modes!**
  - We called the first game mode "Candied Apples", where the snake exponentially speeds up every time it eats an apple.

<details>
<summary>Show code</summary>

```js
if(current_gamemode === 'candied'){
    const newSpeed = Math.max(6, Number(snake_speed) - 5);
    setSnakeSpeed(newSpeed);
}
```

</details>


- The second game mode is called "Fruit Frenzy", where a golden apple that bounces around can be eaten for 3 points. It has a 40% chance of spawning when you eat an apple.

<details>
<summary>Show code</summary>

```js
// Code to spawn golden fruit
if(current_gamemode === 'fruit_frenzy' && Math.random() < 0.4 && !goldenFood.active){
    let gx = Math.floor(Math.random() * ((canvas.width / BLOCK) - 1));
    let gy = Math.floor(Math.random() * ((canvas.height / BLOCK) - 1));
    goldenFood.x = gx; goldenFood.y = gy;
    goldenFood.vx = (Math.random() < 0.5) ? 1 : -1;
    goldenFood.vy = (Math.random() < 0.5) ? 1 : -1;
    goldenFood.active = true;
}
```

```js
// Code to move the fruit
// Move golden fruit each frame
if(goldenFood.active){
    let nextX = goldenFood.x + goldenFood.vx;
    let nextY = goldenFood.y + goldenFood.vy;
    if(nextX < 0 || nextX >= canvas.width / BLOCK) goldenFood.vx *= -1; // reverse vx and/or vy if needed
    if(nextY < 0 || nextY >= canvas.height / BLOCK) goldenFood.vy *= -1;
    goldenFood.x = nextX;
    goldenFood.y = nextY;
}
```

</details>
<hr>

**We made the snake blue and the apple red**

<details>
<summary>Show code</summary>

```js
let activeDot = function(x, y){
    ctx.fillStyle = color_snake; // blue snake
    ctx.fillRect(x * BLOCK, y * BLOCK, BLOCK, BLOCK);
}
let activeApple = function(x, y){
    ctx.fillStyle = color_apple; // red apple
    ctx.fillRect(x * BLOCK, y * BLOCK, BLOCK, BLOCK);
}
```
</details>

<hr>


**Cleaned up the settings UI significantly :)**
  - See the above image

<details>
<summary>Show code</summary>

```css
#setting input{ display:none; }
    #setting label{ cursor: pointer; }
    #setting input:checked + label{ background-color: #FFF; color: #000; }

    #setting { 
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 1rem;
    }
    .settings-card{
        width: 100%;
        max-width: 420px;
        background: rgba(0,0,0,0.18);
        border-radius: 12px;
        padding: 18px;
        box-shadow: 0 6px 18px rgba(0,0,0,0.35);
        text-align: left;
    }
    .settings-title{
        font-size: 1.15rem;
        margin-bottom: 8px;
        font-weight: 600;
        color: #fff;
    }
    .setting-group{
        margin: 12px 0;
    }
    .option-row{
        display: flex;
        gap: 8px;
        flex-wrap: wrap;
        margin-top: 8px;
    }

    #setting input + label{
        display: inline-block;
        padding: 8px 14px;
        border-radius: 999px;
        background: rgba(255,255,255,0.06);
        color: #eaeaea;
        border: 1px solid rgba(255,255,255,0.06);
        transition: all 0.15s ease-in-out;
        user-select: none;
    }
    #setting input:checked + label{
        background-color: #fff;
        color: #000;
        box-shadow: 0 4px 10px rgba(0,0,0,0.2) inset;
        transform: translateY(-1px);
    }
    #setting p{ color: #f1f1f1; }
    

    @media (max-width: 520px){
        .game-shell{ padding: 12px; border-radius: 10px; }
        .screen-card, .settings-card{ margin: 10px; padding: 14px; }
        canvas{ border-width: 4px; }
    }
```
</details>

<hr>


**Added two more game speeds**
  - Honestly, the "Troll" and "Turtle" game modes were mainly just trolling. But if you're feeling daring, try to get a single apple at "Troll" speed with walls enabled >:)

<details>
<summary>Show code</summary>

```html
<div class="option-row">
    <input id="speed_turtle" type="radio" name="speed" value="220"/>
    <label for="speed_turtle">Turtle</label>
    <input id="speed1" type="radio" name="speed" value="120" checked/>
    <label for="speed1">Slow</label>
    <input id="speed2" type="radio" name="speed" value="75"/>
    <label for="speed2">Normal</label>
    <input id="speed3" type="radio" name="speed" value="35"/>
    <label for="speed3">Fast</label>
    <input id="speed_troll" type="radio" name="speed" value="8"/>
    <label for="speed_troll">Troll</label>
</div>
```
</details>

<hr>


**Improved the UI of the Game Over screen**
  - Added an image:

![Alt text]({{site.baseurl}}/images/hacks/snake-gameover.png "snake-gameover.png")

### Here are our GitHub Issues:
- #10: <https://github.com/CompSciTeam/student/issues/10>: Page Scrolling, add WASD controls
- #1: <https://github.com/CompSciTeam/student/issues/1>: Add different game modes
- #2: <https://github.com/CompSciTeam/student/issues/2>: Improve points UI and the settings/game over screens' UIs
- #3: <https://github.com/CompSciTeam/student/issues/3>: Change background color scheme + add themes
- #4: <https://github.com/CompSciTeam/student/issues/4> Change apple color

## Play our new-and-improved snake game at [compsciteam.github.io/student/snake](https://compsciteam.github.io/student/snake)!

Made by Subteam 1: Anish Gupta, James Blazek, and Vihaan Budharaja (and Samarth Hande before he left us for the dark side, Subteam 2 🫡)
