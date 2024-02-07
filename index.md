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
    background-color: #007FFF;
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
        constructor(image) {
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

            this.image = image;
        }
        // Method to draw the player on the canvas
        draw() {
            c.fillStyle = 'cyan';
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
<<<<<<< HEAD
                this.velocity.y -= 20;
=======
                this.velocity.y -= 30;
>>>>>>> 60a726d4f41fe2be0211ef72760c9451194b1dab
                this.jumps++;
            }
        }
    } 
    class Enemy {
        constructor(image) {
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

            this.image = image;
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
     class BlockObject {
        constructor(image) {
            // Initial position of the block object
            this.position = {
                x: 200,
                y: 100
            };
            this.image = image;
            this.width = 158;
            this.height = 79;
        }
        // Method to draw the block object on the canvas
        draw() {
            c.drawImage(this.image, this.position.x, this.position.y);
        }
    }
    //--
    // NEW CODE - CREATE PLATFORM OBJECT WITH IMAGE
    //--
    // Load platform image
    let image = new Image();
    let imageBlock = new Image();
    let blockObject = new BlockObject(imageBlock);
    image.src = 'https://samayass.github.io/samayaCSA/images/platform.png'
    imageBlock.src = 'https://samayass.github.io/samayaCSA/images/box.png';

    // Create a platform object
    let platform = new Platform(image);
    // Load player image
    let playerImage = new Image();
    playerImage.src = '{{site.baseurl}}/images/Andrew_anime_Animation'
    // Create a player object
    player = new Player(playerImage);
    enemy = new Enemy();
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
            player.velocity.x = 5;
        } else if (keys.left.pressed && player.position.x >= 50) {
            player.velocity.x = -5;
        } else {
            player.velocity.x = 0;
        }
    
        platform.draw();
        player.update();
        enemy.update();
        blockObject.draw();
        //--
        // COLLISIONS BETWEEN BLOCK OBJECT AND PLAYER
        //--
        if (
            player.position.y + player.height <= blockObject.position.y &&
            player.position.y + player.height + player.velocity.y >= blockObject.position.y &&
            player.position.x + player.width >= blockObject.position.x &&
            player.position.x <= blockObject.position.x + blockObject.width
        )
        {
            player.velocity.y = 0;
        }
       
        // Control players horizontal movement
        if (keys.right.pressed && player.position.x + player.width <= canvas.width - 50) {
            player.velocity.x = 5;
            console.log("Move");
        } else if (keys.left.pressed && player.position.x >= 50) {
            player.velocity.x = -5;
        } else {
            player.velocity.x = 0;
        }
        //--
        //Enemy Movement
        if(enemy.position.x > player.position.x){
            enemy.velocity.x = -5;
        }else if(enemy.position.x < player.position.x){
            enemy.velocity.x = 5;
        }
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

    if (
    enemy.position.y + player.height >= platform.position.y &&
    enemy.position.y <= platform.position.y + platform.height &&
    enemy.position.x + player.width >= platform.position.x &&
    enemy.position.x <= platform.position.x + platform.width
) {
    enemy.position.y = platform.position.y - enemy.height;
    enemy.velocity.y = 0;
    enemy.jumps = 0;
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