---
title: JS Sprite Animation
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
            <img id="dogSprite" src="{{site.baseurl}}/images/dogSprites.png">
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animation works -->
            <input type="radio" name="animation" id="idle" checked>
            <label for="idle">Idle</label><br>
            <input type="radio" name="animation" id="barking">
            <label for="barking">Barking</label><br>
            <input type="radio" name="animation" id="walking">
            <label for="walking">Walking</label><br>
        </div>
    </div>
</body>

<script>
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 160;
        const SPRITE_HEIGHT = 144;
        const SCALE_FACTOR = 2;
        const FRAME_LIMIT = 48;
        const FRAME_RATE = 15;

        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Dog {
            constructor() {
                this.image = document.getElementById("dogSprite");
                this.spriteWidth = SPRITE_WIDTH;
                this.spriteHeight = SPRITE_HEIGHT;
                this.width = this.spriteWidth;
                this.height = this.spriteHeight;
                this.x = 0;
                this.y = 0;
                this.scale = 1;
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT;
                this.frameX = 0;
                this.frameY = 0;
            }

            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * this.spriteWidth,
                    this.frameY * this.spriteHeight,
                    this.spriteWidth,
                    this.spriteHeight,
                    this.x,
                    this.y,
                    this.width * this.scale,
                    this.height * this.scale
                );
            }

            update() {
                if (this.frameX < this.maxFrame) {
                    this.frameX++;
                } else {
                    this.frameX = 0;
                }
            }
        }

        const dog = new Dog();

window.addEventListener('keydown', function (event) {
    if (event.key === 'ArrowLeft' && document.getElementById('walking').checked) {
        // Check if the "Walking" radio button is selected and the left arrow key is pressed
        dog.x -= 5; // Update the dog's x-coordinate to move it to the left
    }
});
        // Add event listener to the parent container for event delegation
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'idle':
                        dog.frameY = 0;
                        break;
                    case 'barking':
                        dog.frameY = 1;
                        break;
                    case 'walking':
                        dog.frameY = 2;
                        break;
                    default:
                        break;
                }
            }
        });

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            dog.draw(ctx);
            dog.update();
            requestAnimationFrame(animate);
        }

        animate();
    });
</script>