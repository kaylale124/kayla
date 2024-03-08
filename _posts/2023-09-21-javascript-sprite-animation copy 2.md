---
title: Sprite Animation
comments: true
hide: true
layout: default
description: Use JavaScript without external libraries to animate Mario moving across screen, OOP style.
courses: { csse: {week: }}
type: hacks
---
<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="dogSprite" src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/21542/DemoRpgCharacter.png" alt="Character"></img>
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animation works -->
            <input type="radio" name="animation" id="down" checked>
            <label for="down">Down</label><br>
            <input type="radio" name="animation" id="right" checked>
            <label for="right">Right</label><br>
            <input type="radio" name="animation" id="up" checked>
            <label for="up">Up</label><br>
            <input type="radio" name="animation" id="left">
            <label for="Left">Left</label><br>
        </div>
    </div>
</body>

<script>
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const SPRITE_WIDTH = 32px;
        const SPRITE_HEIGHT = 32px;
        const FRAME_LIMIT = 48;

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

        // Add event listener to the parent container for event delegation
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

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            dog.draw(ctx);
            dog.update();
            requestAnimationFrame(animate);
        }

        animate();
    });
</script>