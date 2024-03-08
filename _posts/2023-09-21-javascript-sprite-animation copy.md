---
title: Sprite Animation
comments: true
hide: true
layout: default
description: Use JavaScript without external libraries to animate Mario moving across screen, OOP style.
courses: { csse: {week: 5} }
type: hacks
---

<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="dogSprite" src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/21542/DemoRpgCharacter.png">  // change sprite here
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="down">
            <label for="down">Down</label><br>
            <input type="radio" name="animation" id="right">
            <label for="right">Right</label><br>
            <input type="radio" name="animation" id="up">
            <label for="up">Up</label><br>
            <input type="radio" name="animation" id="left">
            <label for="left">Left</label><br>
        </div>
    </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 32;  // matches sprite pixel width
        const SPRITE_HEIGHT = 32; // matches sprite pixel height
        const FRAME_LIMIT = 4;  // matches number of frames per sprite row, this code assume each row is same

        const SCALE_FACTOR = 6;  // control size of sprite on canvas
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Dog {
            constructor() {
                this.image = document.getElementById("dogSprite");
                this.x = 0;
                this.y = 0;
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT;
                this.frameX = 0;
                this.frameY = 0;
            }

            // draw dog object
            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * SPRITE_WIDTH,
                    this.frameY * SPRITE_HEIGHT,
                    SPRITE_WIDTH,
                    SPRITE_HEIGHT,
                    this.x,
                    this.y,
                    canvas.width,
                    canvas.height
                );
            }

            // update frameX of object
            update() {
                if (this.frameX < this.maxFrame) {
                    this.frameX++;
                } else {
                    this.frameX = 0;
                }
            }
        }

        // dog object
        const dog = new Dog();

        // update frameY of dog object, action from idle, bark, walk radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'down':
                        dog.frameY = 0;
                        break;
                    case 'right':
                        dog.frameY = 1;
                        break;
                    case 'up':
                        dog.frameY = 2;
                        break;
                    case 'left':
                        dog.frameY = 3;
                        break;
                    default:
                        break;
                }
            }
        });

        // Animation recursive control function
        function animate() {
            // Clears the canvas to remove the previous frame.
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draws the current frame of the sprite.
            dog.draw(ctx);

            // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
            dog.update();

            setTimeout(() => {
        // Uses `requestAnimationFrame` to synchronize the animation loop with the display's refresh rate,
        // ensuring smooth visuals.
        requestAnimationFrame(animate);
    }, 200); // Adjust the delay in milliseconds (e.g., 100 milliseconds = 10 frames per second).
}

        // run 1st animate
        animate();
    });
</script>

