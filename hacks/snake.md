---
layout: base
title: Snake Game
permalink: /snake/
---

Here is the writeup for our snake game: <https://compsciteam.github.io/snake/writeup>

<style>
    /* Page and layout */
    body{
        // background: linear-gradient(180deg, #0f1724 0%, #112131 60%);
        color: #e6eef8;
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        padding: 28px 12px;
    }
    .wrap{
        margin-left: auto;
        margin-right: auto;
        display: block;
    }

    /* central game card */
    .game-shell{
        max-width: 520px;
        margin: 0 auto;
        background: rgba(255,255,255,0.03);
        border-radius: 14px;
        padding: 18px;
        box-shadow: 0 10px 30px rgba(2,6,23,0.6);
    }

    canvas{
        display: none;
        width: 100%;
        height: auto;
        border-radius: 10px;
        border: 6px solid rgba(255,255,255,0.08);
        box-shadow: 0 8px 18px rgba(0,0,0,0.6), inset 0 0 18px rgba(255,255,255,0.02);
        background: linear-gradient(180deg, #99d750 0%, #74c02a 100%);
    }
    canvas:focus{ outline: none; }

    /* All screens style (menu, settings, gameover) */
    #gameover p, #setting p, #menu p{ font-size: 18px; margin: 8px 0; }

    .screen-card{
        background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
        padding: 18px;
        border-radius: 10px;
        margin: 14px auto;
        max-width: 420px;
        box-shadow: 0 6px 18px rgba(0,0,0,0.45);
    }

    /* make the action anchors look like buttons */
    .link-alert{
        display:inline-block;
        padding: 10px 16px;
        border-radius: 8px;
        margin: 6px 6px 0 0;
        background: linear-gradient(180deg,#2b7af0,#1453c7);
        color: #fff;
        text-decoration: none;
        font-weight: 600;
        box-shadow: 0 6px 16px rgba(20,40,90,0.45);
        transition: transform 0.12s ease, box-shadow 0.12s ease;
    }
    .link-alert:hover{ transform: translateY(-2px); box-shadow: 0 10px 24px rgba(20,40,90,0.55); cursor:pointer; }

    #menu{ display: block; }
    #gameover{ display: none; }
    #setting{ display: none; }

    /* keep settings input hidden but style labels as pills (see below) */
    #setting input{ display:none; }
    #setting label{ cursor: pointer; }
    #setting input:checked + label{ background-color: #FFF; color: #000; }
    /* New styles for improved settings UI */
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
    /* pill style labels for radios */
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
    /* small responsive tweak */
    @media (max-width: 520px){
        .game-shell{ padding: 12px; border-radius: 10px; }
        .screen-card, .settings-card{ margin: 10px; padding: 14px; }
        canvas{ border-width: 4px; }
    }
</style>

<h2>Snake</h2>
<div class="container">
    <p class="fs-4">Apples: <span id="score_value">0</span></p>

    <div class="game-shell" style="text-align:center;">
        <!-- Main Menu -->
        <div id="menu" class="py-4 text-light screen-card">
            <p>Welcome to Snake — press <span style="background-color: #FFFFFF; color: #000000; padding:2px 6px; border-radius:4px">space</span> to begin</p>
            <a id="new_game" class="link-alert">New Game</a>
            <a id="setting_menu" class="link-alert">Settings</a>
        </div>
        <!-- Game Over -->
        <div id="gameover" class="py-4 text-light screen-card" style="display:none;">
            <p>Game Over — press <span style="background-color: #FFFFFF; color: #000000; padding:2px 6px; border-radius:4px">space</span> to try again</p>
            <a id="new_game1" class="link-alert">New Game</a>
            <a id="setting_menu1" class="link-alert">Settings</a>
        </div>
        <!-- Play Screen -->
        <canvas id="snake" class="wrap" width="320" height="320" tabindex="1"></canvas>
        <!-- Settings Screen -->
        <div id="setting" class="py-4 text-light" style="display:none;">
            <div class="settings-card">
                <div class="settings-title">Settings</div>
                <p style="margin:0 0 8px 0; font-size:0.95rem; color: #ddd;">Press <span style="background-color: #FFFFFF; color: #000000; padding:2px 6px; border-radius:4px">space</span> to go back to playing</p>
                <div class="setting-group">
                    <div style="display:flex; justify-content:space-between; align-items:center;">
                        <div style="font-weight:600; color:#fff;">Speed</div>
                        <a id="new_game2" class="link-alert" style="font-size:0.95rem;">new game</a>
                    </div>
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
                </div>
                <div class="setting-group">
                    <div style="font-weight:600; color:#fff;">Wall</div>
                    <div class="option-row">
                        <input id="wallon" type="radio" name="wall" value="1" checked/>
                        <label for="wallon">On</label>
                        <input id="walloff" type="radio" name="wall" value="0"/>
                        <label for="walloff">Off</label>
                    </div>
                </div>
                <div class="setting-group">
                    <div style="font-weight:600; color:#fff;">Mode</div>
                    <div class="option-row">
                        <input id="mode_default" type="radio" name="mode" value="default" checked/>
                        <label for="mode_default">Default</label>
                        <input id="mode_colorblind" type="radio" name="mode" value="colorblind"/>
                        <label for="mode_colorblind">Colorblind</label>
                        <input id="mode_light" type="radio" name="mode" value="light"/>
                        <label for="mode_light">Light</label>
                        <input id="mode_dark" type="radio" name="mode" value="dark"/>
                        <label for="mode_dark">Dark</label>
                    </div>
                </div>
                <div class="setting-group">
                    <div style="font-weight:600; color:#fff;">Gameplay</div>
                    <div class="option-row">
                        <input id="gm_default" type="radio" name="gamemode" value="default" checked/>
                        <label for="gm_default">Default</label>
                        <input id="gm_candied" type="radio" name="gamemode" value="candied"/>
                        <label for="gm_candied">Candied Apples</label>
                        <input id="gm_fruit" type="radio" name="gamemode" value="fruit_frenzy"/>
                        <label for="gm_fruit">Fruit Frenzy</label>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    (function(){
        /* Attributes of Game */
        /////////////////////////////////////////////////////////////
        // Canvas & Context
        const canvas = document.getElementById("snake");
        const ctx = canvas.getContext("2d");
        // HTML Game IDs
        const SCREEN_SNAKE = 0;
        const screen_snake = document.getElementById("snake");
    const ele_score = document.getElementById("score_value");
    const speed_setting = document.getElementsByName("speed");
    const wall_setting = document.getElementsByName("wall");
    const mode_setting = document.getElementsByName("mode");
    const gamemode_setting = document.getElementsByName("gamemode");
        // HTML Screen IDs (div)
        const SCREEN_MENU = -1, SCREEN_GAME_OVER=1, SCREEN_SETTING=2;
        const screen_menu = document.getElementById("menu");
        const screen_game_over = document.getElementById("gameover");
        const screen_setting = document.getElementById("setting");
        // HTML Event IDs (a tags)
        const button_new_game = document.getElementById("new_game");
        const button_new_game1 = document.getElementById("new_game1");
        const button_new_game2 = document.getElementById("new_game2");
        const button_setting_menu = document.getElementById("setting_menu");
        const button_setting_menu1 = document.getElementById("setting_menu1");
        // Game Control
        const BLOCK = 10;   // size of block rendering
        let SCREEN = SCREEN_MENU;
        let snake;
        let snake_dir;
        let snake_next_dir;
        let snake_speed;
        let food = {x: 0, y: 0};
        let score;
        let wall;
    // Color scheme variables (default values)
    let color_light_tile = "#a9d750"; // default light tile
    let color_dark_tile = "#a2d148";  // default dark tile
    let color_snake = "#2f00ffff";    // blue snake
    let color_apple = "#ff0000ff";    // red apple
    let color_golden = "#ffd700";     // golden apple color
    // gameplay mode
    let current_gamemode = 'default';
    // golden moving fruit state (used in Fruit Frenzy mode)
    let goldenFood = {
        x: -1,
        y: -1,
        vx: 1, // velocity in blocks per tick (can be negative)
        vy: 1,
        active: false
    };
        /* Display Control */
        /////////////////////////////////////////////////////////////
        // 0 for the game
        // 1 for the main menu
        // 2 for the settings screen
        // 3 for the game over screen
        let showScreen = function(screen_opt){
            SCREEN = screen_opt;
            switch(screen_opt){
                case SCREEN_SNAKE:
                    screen_snake.style.display = "block";
                    screen_menu.style.display = "none";
                    screen_setting.style.display = "none";
                    screen_game_over.style.display = "none";
                    break;
                case SCREEN_GAME_OVER:
                    screen_snake.style.display = "block";
                    screen_menu.style.display = "none";
                    screen_setting.style.display = "none";
                    screen_game_over.style.display = "block";
                    break;
                case SCREEN_SETTING:
                    screen_snake.style.display = "none";
                    screen_menu.style.display = "none";
                    screen_setting.style.display = "block";
                    screen_game_over.style.display = "none";
                    break;
            }
        }
        /* Actions and Events  */
        /////////////////////////////////////////////////////////////
        window.onload = function(){
            // HTML Events to Functions
            button_new_game.onclick = function(){newGame();};
            button_new_game1.onclick = function(){newGame();};
            button_new_game2.onclick = function(){newGame();};
            button_setting_menu.onclick = function(){showScreen(SCREEN_SETTING);};
            button_setting_menu1.onclick = function(){showScreen(SCREEN_SETTING);};
            // speed
            setSnakeSpeed(150);
            for(let i = 0; i < speed_setting.length; i++){
                speed_setting[i].addEventListener("click", function(){
                    for(let i = 0; i < speed_setting.length; i++){
                        if(speed_setting[i].checked){
                            setSnakeSpeed(speed_setting[i].value);
                        }
                    }
                });
            }
            // wall setting
            setWall(1);
            for(let i = 0; i < wall_setting.length; i++){
                wall_setting[i].addEventListener("click", function(){
                    for(let i = 0; i < wall_setting.length; i++){
                        if(wall_setting[i].checked){
                            setWall(wall_setting[i].value);
                        }
                    }
                });
            }
            // mode setting (colors)
            function applyMode(mode){
                switch(mode){
                    case 'colorblind':
                        // high contrast, colorblind-friendly palette
                        color_light_tile = '#ffd97a'; // warm yellow
                        color_dark_tile  = '#ffd15a';
                        color_snake = '#0000ff'; // bright blue
                        color_apple = '#ff00ff'; // magenta
                        break;
                    case 'light':
                        color_light_tile = '#f0f8e8';
                        color_dark_tile  = '#dfeccf';
                        color_snake = '#0b63d6';
                        color_apple = '#d32f2f';
                        break;
                    case 'dark':
                        color_light_tile = '#355a2b';
                        color_dark_tile  = '#243b1b';
                        color_snake = '#1e90ff';
                        color_apple = '#ff6b6b';
                        break;
                    default:
                        // default green scheme
                        color_light_tile = '#a9d750';
                        color_dark_tile  = '#a2d148';
                        color_snake = '#2f00ffff';
                        color_apple = '#ff0000ff';
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
            for(let i = 0; i < mode_setting.length; i++){
                mode_setting[i].addEventListener('click', function(){
                    for(let j = 0; j < mode_setting.length; j++){
                        if(mode_setting[j].checked) applyMode(mode_setting[j].value);
                    }
                });
            }
            // gameplay mode listeners
            for(let i = 0; i < gamemode_setting.length; i++){
                gamemode_setting[i].addEventListener('click', function(){
                    for(let j = 0; j < gamemode_setting.length; j++){
                        if(gamemode_setting[j].checked) current_gamemode = gamemode_setting[j].value;
                    }
                });
            }
            // activate window events
            window.addEventListener("keydown", function(evt) {
                // spacebar detected
                if(evt.code === "Space" && SCREEN !== SCREEN_SNAKE)
                    newGame();
            }, true);
        }
        /* Snake is on the Go (Driver Function)  */
        /////////////////////////////////////////////////////////////
        let mainLoop = function(){
            let _x = snake[0].x;
            let _y = snake[0].y;
            snake_dir = snake_next_dir;   // read async event key
            // Direction 0 - Up, 1 - Right, 2 - Down, 3 - Left
            switch(snake_dir){
                case 0: _y--; break;
                case 1: _x++; break;
                case 2: _y++; break;
                case 3: _x--; break;
            }
            snake.pop(); // tail is removed
            snake.unshift({x: _x, y: _y}); // head is new in new position/orientation
            // Wall Checker
            if(wall === 1){
                // Wall on, Game over test
                if (snake[0].x < 0 || snake[0].x === canvas.width / BLOCK || snake[0].y < 0 || snake[0].y === canvas.height / BLOCK){
                    showScreen(SCREEN_GAME_OVER);
                    return;
                }
            }else{
                // Wall Off, Circle around
                for(let i = 0, x = snake.length; i < x; i++){
                    if(snake[i].x < 0){
                        snake[i].x = snake[i].x + (canvas.width / BLOCK);
                    }
                    if(snake[i].x === canvas.width / BLOCK){
                        snake[i].x = snake[i].x - (canvas.width / BLOCK);
                    }
                    if(snake[i].y < 0){
                        snake[i].y = snake[i].y + (canvas.height / BLOCK);
                    }
                    if(snake[i].y === canvas.height / BLOCK){
                        snake[i].y = snake[i].y - (canvas.height / BLOCK);
                    }
                }
            }
            // Snake vs Snake checker
            for(let i = 1; i < snake.length; i++){
                // Game over test
                if (snake[0].x === snake[i].x && snake[0].y === snake[i].y){
                    showScreen(SCREEN_GAME_OVER);
                    return;
                }
            }
            // Snake eats food checker
            if(checkBlock(snake[0].x, snake[0].y, food.x, food.y)){
                snake[snake.length] = {x: snake[0].x, y: snake[0].y};
                altScore(++score);
                addFood();
                activeDot(food.x, food.y);
                // If Candied Apples gamemode is enabled, speed up slightly on each apple
                if(current_gamemode === 'candied'){
                    // each candied apple makes the snake a bit faster: decrease delay by 5ms per apple, floor at 6ms
                    const newSpeed = Math.max(6, Number(snake_speed) - 5);
                    setSnakeSpeed(newSpeed);
                }
            }
            // Fruit Frenzy: check golden moving fruit collision
            if(goldenFood.active && checkBlock(snake[0].x, snake[0].y, goldenFood.x, goldenFood.y)){
                // grow snake by one block
                snake[snake.length] = {x: snake[0].x, y: snake[0].y};
                // award 3 points for golden apple
                score += 3;
                altScore(score);
                // deactivate golden and respawn normal food
                goldenFood.active = false;
                addFood();
                // small speed boost when golden eaten (optional): decrease delay by 8ms, but not below 6ms
                if(current_gamemode === 'fruit_frenzy'){
                    const newSpeed = Math.max(6, Number(snake_speed) - 8);
                    setSnakeSpeed(newSpeed);
                }
            }
            // Repaint canvas with checkered background
            for(let y = 0; y < canvas.height / BLOCK; y++) {
                for(let x = 0; x < canvas.width / BLOCK; x++) {
                    ctx.fillStyle = ((x + y) % 2 === 0) ? color_light_tile : color_dark_tile;
                    ctx.fillRect(x * BLOCK, y * BLOCK, BLOCK, BLOCK);
                }
            }
            // Paint snake
            for(let i = 0; i < snake.length; i++){
                activeDot(snake[i].x, snake[i].y);
            }
            // Paint food
            activeApple(food.x, food.y);
            // Paint golden food if active
            if(goldenFood.active){
                // draw golden with a different color
                ctx.fillStyle = color_golden;
                ctx.fillRect(goldenFood.x * BLOCK, goldenFood.y * BLOCK, BLOCK, BLOCK);
            }
            // Debug
            //document.getElementById("debug").innerHTML = snake_dir + " " + snake_next_dir + " " + snake[0].x + " " + snake[0].y;
            // Recursive call after speed delay, déjà vu
            // move golden fruit between frames (simple bouncing logic)
            if(goldenFood.active){
                // advance position by velocity
                goldenFood.x += goldenFood.vx;
                goldenFood.y += goldenFood.vy;
                // bounce off edges (reverse velocity when hitting walls)
                if(goldenFood.x < 0){ goldenFood.x = 0; goldenFood.vx *= -1; }
                if(goldenFood.x >= canvas.width / BLOCK){ goldenFood.x = (canvas.width / BLOCK) - 1; goldenFood.vx *= -1; }
                if(goldenFood.y < 0){ goldenFood.y = 0; goldenFood.vy *= -1; }
                if(goldenFood.y >= canvas.height / BLOCK){ goldenFood.y = (canvas.height / BLOCK) - 1; goldenFood.vy *= -1; }
            }

            setTimeout(mainLoop, snake_speed);
        }
        /* New Game setup */
        /////////////////////////////////////////////////////////////
        let newGame = function(){
            // snake game screen
            showScreen(SCREEN_SNAKE);
            screen_snake.focus();
            // game score to zero
            score = 0;
            altScore(score);
            // initial snake
            snake = [];
            snake.push({x: 0, y: 15});
            snake_next_dir = 1;
            // food on canvas
            addFood();
            // reset golden fruit state
            goldenFood.active = false;
            goldenFood.x = -1; goldenFood.y = -1; goldenFood.vx = 1; goldenFood.vy = 1;
            // activate canvas event
            canvas.onkeydown = function(evt) {
                changeDir(evt.keyCode);
            }
            mainLoop();
        }
        /* Key Inputs and Actions */
        /////////////////////////////////////////////////////////////
        let changeDir = function(key){
            // test key and switch direction
            switch(key) {
                case 37: // left arrow
                case 65: // 'A'
                    if (snake_dir !== 1) // not right
                        snake_next_dir = 3; // then switch left
                    break;
                case 38: // up arrow
                case 87: // 'W'
                    if (snake_dir !== 2) // not down
                        snake_next_dir = 0; // then switch up
                    break;
                case 39: // right arrow
                case 68: // 'D'
                    if (snake_dir !== 3) // not left
                        snake_next_dir = 1; // then switch right
                    break;
                case 40: // down arrow
                case 83: // 'S'
                    if (snake_dir !== 0) // not up
                        snake_next_dir = 2; // then switch down
                    break;
            }
        }
        /* Dot for Food or Snake part */
        /////////////////////////////////////////////////////////////
        let activeDot = function(x, y){
                    ctx.fillStyle = color_snake;
            ctx.fillRect(x * BLOCK, y * BLOCK, BLOCK, BLOCK);
        }

        let activeApple = function(x, y){
                    ctx.fillStyle = color_apple;
            ctx.fillRect(x * BLOCK, y * BLOCK, BLOCK, BLOCK);
        }   

        /* Random food placement */
        /////////////////////////////////////////////////////////////
        let addFood = function(){
                    // If Fruit Frenzy mode is active, occasionally spawn a moving golden fruit
                    if(current_gamemode === 'fruit_frenzy' && Math.random() < 0.25 && !goldenFood.active){
                        // attempt to place golden fruit not on the snake
                        let gx = Math.floor(Math.random() * ((canvas.width / BLOCK) - 1));
                        let gy = Math.floor(Math.random() * ((canvas.height / BLOCK) - 1));
                        for(let i = 0; i < snake.length; i++){
                            if(checkBlock(gx, gy, snake[i].x, snake[i].y)){
                                // collision with snake, retry
                                return addFood();
                            }
                        }
                        goldenFood.x = gx;
                        goldenFood.y = gy;
                        // give it random small velocity of -1, 0, or 1 (avoid 0,0)
                        goldenFood.vx = (Math.random() < 0.5) ? 1 : -1;
                        goldenFood.vy = (Math.random() < 0.5) ? 1 : -1;
                        goldenFood.active = true;
                        // also place a normal food somewhere else
                        food.x = Math.floor(Math.random() * ((canvas.width / BLOCK) - 1));
                        food.y = Math.floor(Math.random() * ((canvas.height / BLOCK) - 1));
                        for(let i = 0; i < snake.length; i++){
                            if(checkBlock(food.x, food.y, snake[i].x, snake[i].y)){
                                return addFood();
                            }
                        }
                        return;
                    }
                    // default single static food
                    food.x = Math.floor(Math.random() * ((canvas.width / BLOCK) - 1));
                    food.y = Math.floor(Math.random() * ((canvas.height / BLOCK) - 1));
                    for(let i = 0; i < snake.length; i++){
                        if(checkBlock(food.x, food.y, snake[i].x, snake[i].y)){
                            addFood();
                        }
                    }
        }
        /* Collision Detection */
        /////////////////////////////////////////////////////////////
        let checkBlock = function(x, y, _x, _y){
            return (x === _x && y === _y);
        }
        /* Update Score */
        /////////////////////////////////////////////////////////////
        let altScore = function(score_val){
            ele_score.innerHTML = String(score_val);
        }
        /////////////////////////////////////////////////////////////
        // Change the snake speed (delay in ms) - larger = slower
        // Turtle = 220 (hilariously slow)
        // Slow   = 120
        // Normal = 75
        // Fast   = 35
        // Troll  = 8   (ridiculously fast)
        let setSnakeSpeed = function(speed_value){
            snake_speed = speed_value;
        }
        /////////////////////////////////////////////////////////////
        let setWall = function(wall_value){
            wall = wall_value;
            if(wall === 0){screen_snake.style.borderColor = "#606060";}
            if(wall === 1){screen_snake.style.borderColor = "#FFFFFF";}
        }
    })();
</script>