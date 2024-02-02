---
toc: false
comments: true
layout: post
title: Game v2
description: Week 2 hacks
type: hacks
courses: { compsci: {week: 2} }
---

<style>
  body {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    margin: 0;
  }

  #canvas {
    background-color: #f0f0f0;
    border: 1px solid black;
  }
</style>

<canvas id='canvas'></canvas>

<script>
var test = 0
console.log ("test;"+test)

// while (true)
// {test++
// console.log (test)}

   // Create empty canvas
    let canvas = document.getElementById('canvas');
    let c = canvas.getContext('2d');
    // Set the canvas dimensions
    canvas.width = 700;
    canvas.height = 450;
    // Define gravity value
    let gravity = 1.5;
    // Define the Player class
    class Player {
        constructor() {
            // Initial position and velocity of the player
            this.position = {
                x: 100,
                y: 200
            };
            this.velocity = {
                x: 0,
                y: 0
            };
            // Dimensions of the player
            this.width = 30;
            this.height = 30;
            // Track the number of jumps
            this.jumps = 0;
            // Maximum allowed jumps
            this.maxJumps = 1;
        }
        // Method to draw the player on the canvas
        draw() {
            c.fillStyle = 'orange';
            c.fillRect(this.position.x, this.position.y, this.width, this.height);
        }
        // Method to update the player's position and velocity
        update() {
            this.draw();
            this.position.y += this.velocity.y;
            this.position.x += this.velocity.x;
            if (this.position.y + this.height + this.velocity.y <= canvas.height)
                this.velocity.y += gravity;
            else {
                this.velocity.y = 0;
                this.jumps = 0; 
            }
        }
        jump() {
            if (this.jumps < this.maxJumps) {
                this.velocity.y -= 20;
                this.jumps++;
            }
        }
    }
        class Platform {
        constructor(image) {
            // Initial position of the platform
            this.position = {
                x: 0,
                y: 400
            }
            this.image = image;
            this.width = 650;
            this.height = 100;
        }
        // Method to draw the platform on the canvas
        draw() {
            c.drawImage(this.image, this.position.x, this.position.y, this.width, this.height);
        }
    }
    //--
    // NEW CODE - CREATE PLATFORM OBJECT WITH IMAGE
    //--
    // Load platform image
    let image = new Image();
    image.src = 'https://samayass.github.io/samayaCSA/images/platform.png'
    // Create a platform object
    let platform = new Platform(image);
    // Create a player object
    player = new Player();
    // Define keyboard keys and their states
    let keys = {
        right: {
            pressed: false
        },
        left: {
            pressed: false
        }
    };
    // Animation function to continuously update and render the canvas
    function animate() {
        requestAnimationFrame(animate);
        c.clearRect(0, 0, canvas.width, canvas.height);
        player.update();
        if (keys.right.pressed && player.position.x + player.width <= canvas.width - 50) {
            player.velocity.x = 15;
        } else if (keys.left.pressed && player.position.x >= 50) {
            player.velocity.x = -15;
        } else {
            player.velocity.x = 0;
        }
    
    platform.draw();
        player.update();
        // Control players horizontal movement
        if (keys.right.pressed && player.position.x + player.width <= canvas.width - 50) {
            player.velocity.x = 15;
        } else if (keys.left.pressed && player.position.x >= 50) {
            player.velocity.x = -15;
        } else {
            player.velocity.x = 0;
        }
        //--
        // NEW CODE  - PLATFORM COLLISIONS
        //--
        // Check for collision between player and platform
        if (
    player.position.y + player.height >= platform.position.y &&
    player.position.y <= platform.position.y + platform.height &&
    player.position.x + player.width >= platform.position.x &&
    player.position.x <= platform.position.x + platform.width
) {
    player.position.y = platform.position.y - player.height;
    player.velocity.y = 0;
    player.jumps = 0;
}
    }

    animate();
    // Event listener for keydown events
    addEventListener('keydown', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = true;
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                keys.right.pressed = true;
                break;
            case 87:
                console.log('up');
                player.jump(); // Call jump method on keypress
                break;
        }
    });
    // Event listener for keyup events
    addEventListener('keyup', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = false;
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                keys.right.pressed = false;
                break;
            case 87:
                console.log('up');
                // You can optionally handle key release for jumping, but it's not necessary for this example.
                break;
        }
    });
</script>