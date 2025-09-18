---
layout: opencs
title: Background with Object
description: Use JavaScript to have an in motion background.
sprite: images/posts/buzz-lightyear2.png
background: images/platformer/backgrounds/alien_planet2.jpg
permalink: /background
---

<!-- Canvas element where the game world will be drawn -->
<canvas id="world"></canvas>

<script>
  // Get the canvas element and its 2D drawing context
  const canvas = document.getElementById("world");
  const ctx = canvas.getContext('2d');

  // Create Image objects for background and player sprite
  const backgroundImg = new Image();
  const spriteImg = new Image();

  // Set the source paths for images from the page frontmatter
  backgroundImg.src = '{{page.background}}';
  spriteImg.src = '{{page.sprite}}';

  // Counter to track when both images are fully loaded
  let imagesLoaded = 0;

  // Increment counter when background image loads, then attempt to start the game
  backgroundImg.onload = function() {
    imagesLoaded++;
    startGameWorld();
  };

  // Increment counter when sprite image loads, then attempt to start the game
  spriteImg.onload = function() {
    imagesLoaded++;
    startGameWorld();
  };

  // Starts the game only after both images are loaded
  function startGameWorld() {
    if (imagesLoaded < 2) return; // Wait until both images are ready

    // Base class for all drawable/movable objects in the game
    class GameObject {
      constructor(image, width, height, x = 0, y = 0, speedRatio = 0) {
        this.image = image;       // Image to draw
        this.width = width;       // Width of object
        this.height = height;     // Height of object
        this.x = x;               // X position
        this.y = y;               // Y position
        this.speedRatio = speedRatio; // Relative speed for scrolling/background movement
        this.speed = GameWorld.gameSpeed * this.speedRatio; // Actual speed based on game speed
      }

      // Default update method (to be overridden)
      update() {}

      // Draw the object on the canvas
      draw(ctx) {
        ctx.drawImage(this.image, this.x, this.y, this.width, this.height);
      }
    }

    // Background class extends GameObject for scrolling effect
    class Background extends GameObject {
      constructor(image, gameWorld) {
        // Fill the canvas completely
        super(image, gameWorld.width, gameWorld.height, 0, 0, 0.1);
      }

      // Update background X position for scrolling
      update() {
        this.x = (this.x - this.speed) % this.width;
      }

      // Draw two copies of the background side by side to create a continuous loop
      draw(ctx) {
        ctx.drawImage(this.image, this.x, this.y, this.width, this.height);
        ctx.drawImage(this.image, this.x + this.width, this.y, this.width, this.height);
      }
    }

    // Player class extends GameObject and has floating motion
    class Player extends GameObject {
      constructor(image, gameWorld) {
        // Scale player to half its original image size
        const width = image.naturalWidth * 1.3;
        const height = image.naturalHeight * 1.3;
        const x = (gameWorld.width - width) / 2; // Center horizontally
        const y = (gameWorld.height - height) / 2; // Center vertically
        super(image, width, height, x, y);

        this.baseY = y;  // Base vertical position for floating effect
        this.frame = 0;  // Frame counter for sine wave animation
      }

      // Update player's vertical position using sine wave for smooth floating
      update() {
        this.y = this.baseY + Math.sin(this.frame * 0.05) * 20; // 20px amplitude
        this.frame++;
      }
    }

    // Main game world class
    class GameWorld {
      static gameSpeed = 5; // Base speed for scrolling objects

      constructor(backgroundImg, spriteImg) {
        // Setup canvas dimensions
        this.canvas = document.getElementById("world");
        this.ctx = this.canvas.getContext('2d');
        this.width = window.innerWidth;
        this.height = window.innerHeight;
        this.canvas.width = this.width;
        this.canvas.height = this.height;

        // Style canvas to fill screen
        this.canvas.style.width = `${this.width}px`;
        this.canvas.style.height = `${this.height}px`;
        this.canvas.style.position = 'absolute';
        this.canvas.style.left = `0px`;
        this.canvas.style.top = `${(window.innerHeight - this.height) / 2}px`;

        // Initialize game objects
        this.gameObjects = [
          new Background(backgroundImg, this), // scrolling background
          new Player(spriteImg, this)          // floating player sprite
        ];
      }

      // Main game loop, called every frame
      gameLoop() {
        this.ctx.clearRect(0, 0, this.width, this.height); // Clear canvas

        // Update and draw each object
        for (const obj of this.gameObjects) {
          obj.update();
          obj.draw(this.ctx);
        }

        // Request next frame
        requestAnimationFrame(this.gameLoop.bind(this));
      }

      // Start the game loop
      start() {
        this.gameLoop();
      }
    }

    // Create a new game world instance and start the loop
    const world = new GameWorld(backgroundImg, spriteImg);
    world.start();
  }
</script>
