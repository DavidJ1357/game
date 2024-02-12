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
    #canvas {
        margin: 0;
        border: 1px solid white;
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
    canvas.width = 850;
    canvas.height = 850;
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
                this.velocity.y -= 30;
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
                y: 800
            }
            this.image = image;
            this.width = 850;
            this.height = 200;
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
                y: 250
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
    class GenericObject {
        constructor({ x, y, image }) {
            this.position = {
                x,
                y
            };
            this.image = image;
            this.width = 760;
            this.height = 200;
        }
        // Method to draw the generic object on the canvas
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
    let imageBackground = new Image();
    let imageHills = new Image();
    
    image.src = '{{site.baseurl}}/images/platform.png'
    imageBlock.src = '{{site.baseurl}}/images/lava.png';
    imageBackground.src = '{{site.baseurl}}/images/brickwall.webp';
    let playerImage = new Image();
    playerImage.src = '{{site.baseurl}}/images/Andrew_anime_Animation.png'

    // Create a platform object
    let platform = new Platform(image);
    // Load player image
    let genericObjects = [
        new GenericObject({
            x:0, y:0, image: imageBackground
        }),
        new GenericObject({
            x:0, y:70, image: imageHills
        }),
    ];
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
        genericObjects.forEach(genericObject => {
            genericObject.draw()
        });
        platform.draw();
        player.update();
        enemy.update();
        blockObject.draw();
        //--
        // COLLISIONS BETWEEN BLOCK OBJECT AND PLAYER
        //--
        // Check for collision between player and block object
// Check for collision between player and block object
// Check for collision between player and block object
if (
    player.position.y + player.height >= blockObject.position.y &&
    player.position.y <= blockObject.position.y + blockObject.height &&
    player.position.x + player.width >= blockObject.position.x &&
    player.position.x <= blockObject.position.x + blockObject.width
) {
    if (player.position.y + player.height <= blockObject.position.y + blockObject.height / 4) {
        // Stop player from falling through the block
        player.velocity.y = 0;
        player.position.y = blockObject.position.y - player.height; // Align player's position with top of block
        player.jumps = 0; // Reset jumps
    } else if (player.position.y >= blockObject.position.y + blockObject.height / 4) {
        // Check if player is colliding with the bottom half of the block
        // Reset player's vertical velocity to simulate falling back down
        player.velocity.y = 0;
    }

    // Check if player is colliding with the left side of the block
    if (
        player.position.y + player.height > blockObject.position.y &&
        player.position.y < blockObject.position.y + blockObject.height &&
        player.position.x + player.width / 2 > blockObject.position.x &&
        player.position.x < blockObject.position.x + blockObject.width / 2
    ) {
        player.position.x = blockObject.position.x - player.width; // Align player's position with left side of block
    }
}

        //--
        //Enemy Movement
        if(enemy.position.x > player.position.x){
            enemy.velocity.x = -3;
        }else if(enemy.position.x < player.position.x){
            enemy.velocity.x = 3;
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
            case 37:
                console.log('left');
                keys.left.pressed = true;
                break;
            case 40:
                console.log('down');
                break;
            case 39:
                console.log('right');
                keys.right.pressed = true;
                break;
            case 38:
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
                break;
            
        }
    });
</script>