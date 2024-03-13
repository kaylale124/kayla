---
layout: base
title: Mario Game
image: /images/mario_animation.png
hide: true
type: hacks
courses: { csse: {week: 13} }
---
<!-- Liquid:  statements -->

<!-- Include submenu from _includes to top of pages -->
{% include nav_home.html %}
<!--- Concatenation of site URL to frontmatter image  --->
{% assign sprite_file = site.baseurl | append: page.image %}
<!--- Has is a list variable containing mario metadata for sprite --->
{% assign hash = site.data.mario_metadata %}  
<!--- Size width/height of Sprit images --->
{% assign pixels = 256 %} 

<!--- HTML for page contains <p> tag named "Mario" and class properties for a "sprite"  -->

<p id="mario" class="sprite"></p>
  
<!--- Embedded Cascading Style Sheet (CSS) rules, 
        define how HTML elements look 
--->
<style>

  /*CSS style rules for the id and class of the sprite...
  */
  .sprite {
    height: {{pixels}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
  }

  /*background position of sprite element
  */
  #mario {
    background-position: calc({{animations[0].col}} * {{pixels}} * -1px) calc({{animations[0].row}} * {{pixels}}* -1px);
  }
</style>

<!--- Embedded executable code--->
<script>
  ////////// convert YML hash to javascript key:value objects /////////

  var mario_metadata = {}; //key, value object
  {% for key in hash %}  
  
  var key = "{{key | first}}"  //key
  var values = {} //values object
  values["row"] = {{key.row}}
  values["col"] = {{key.col}}
  values["frames"] = {{key.frames}}
  mario_metadata[key] = values; //key with values added

  {% endfor %}

  ////////// game object for player /////////

  class Mario {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario"); //HTML element of sprite
      this.pixels = {{pixels}}; //pixel offset of images in the sprite, set by liquid constant
      this.interval = 100; //animation time interval
      this.obj = meta_data;
      this.marioElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels;
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.pixels;
        this.marioElement.style.backgroundPosition = `-${col}px -${row}px`;
        this.marioElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames;

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) {
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels;
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      this.animate(this.obj["Walk"], 3);
    }

    startWalkingL() {
      this.stopAnimate();
      this.animate(this.obj["WalkL"], -3);
    }

    startRunning() {
      this.stopAnimate();
      this.animate(this.obj["Run1"], 6);
    }

    startRunningL(){
        this.stopAnimate();
        this.animate(this.obj["Run1L"], -6)
    }

    startPuffing() {
      this.stopAnimate();
      this.animate(this.obj["Puff"], 0);
    }

    startPuffingL() {
      this.stopAnimate();
      this.animate(this.obj["PuffL"], 0);
    }

    startCheering() {
      this.stopAnimate();
      this.animate(this.obj["Cheer"], 0);
    }

    startCheeringL() {
      this.stopAnimate();
      this.animate(this.obj["CheerL"], 0);
    }

    startFlipping() {
      this.stopAnimate();
      this.animate(this.obj["Flip"], 0);
    }
    startFlippingL() {
      this.stopAnimate();
      this.animate(this.obj["FlipL"], 0);
    }
    startResting() {
      this.stopAnimate();
      this.animate(this.obj["Rest"], 0);
    }

    startRestingL() {
      this.stopAnimate();
      this.animate(this.obj["RestL"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario = new Mario(mario_metadata);
  
  let dKeyPressed = false;
  let aKeyPressed = false;


  ////////// event control /////////

  window.addEventListener("keydown", (event) => {
    if (event.key === "d") {
      event.preventDefault();
        if (!dKeyPressed) {
        aKeyPressed = false;
        dKeyPressed = true;
        mario.startWalking();
        } 
    } else if (event.key === "a") {
      event.preventDefault();
      if (!aKeyPressed) {
        dKeyPressed = false;
        aKeyPressed = true;
        mario.startWalkingL();
    }
    } else if (event.key === "Shift" && dKeyPressed) {
      dKeyPressed = false;
      event.preventDefault();
      mario.startRunning();
    } else if (event.key === "Shift" && aKeyPressed) {
      aKeyPressed = false;
      event.preventDefault();
      mario.startRunningL(); 
    } else if (event.key === "w" && dKeyPressed) {
      dKeyPressed = false;
      event.preventDefault();
      mario.startCheering();
    } else if (event.key === "w" && aKeyPressed) {
      aKeyPressed = false;
      event.preventDefault();
      mario.startCheeringL();
    } else if (event.key === " " && dKeyPressed) {
      dKeyPressed = false;
      event.preventDefault();
      mario.startFlipping();
    } else if (event.key === " " && aKeyPressed) {
      aKeyPressed = false;
      event.preventDefault();
      mario.startFlippingL();
    } else if (event.key === "r" && dKeyPressed) {
      dKeyPressed = false;
      event.preventDefault();
      mario.startResting();
    } else if (event.key === "r" && aKeyPressed) {
      aKeyPressed = false;
      event.preventDefault();
      mario.startRestingL();
    } else if (event.key === "p" && dKeyPressed) {
      dKeyPressed = false;
      event.preventDefault();
      mario.startPuffing();
    } else if (event.key === "p" && aKeyPressed) {
      aKeyPressed = false; 
      event.preventDefault();
      mario.startPuffingL();
    }
});

window.addEventListener("keyup", (event) => {
    if (event.key === "d") {
        dKeyPressed = false;
        mario.startResting();
    }
});

window.addEventListener("keyup", (event) => {
    if (event.key === "a") {
        aKeyPressed = false;
        mario.startRestingL();
    }
});

    

  //touch events that enable animations
  window.addEventListener("touchstart", (event) => {
    event.preventDefault(); // prevent default browser action
    if (event.touches[0].clientX > window.innerWidth / 2) {
      // move right
      if (currentSpeed === 0) { // if at rest, go to walking
        mario.startWalking();
      } else if (currentSpeed === 3) { // if walking, go to running
        mario.startRunning();
      }
    } else {
      // move left
      mario.startPuffing();
    }
  });

  //stop animation on window blur
  window.addEventListener("blur", () => {
    mario.stopAnimate();
  });

  //start animation on window focus
  window.addEventListener("focus", () => {
     mario.startFlipping();
  });

  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite");
    sprite.style.transform = `scale(${0.2 * scale})`;
    mario.startResting();
  });

</script>

# Commands
- a: move left
- d: move right
- space: flip
- w: cheer
- r: rest
- p: puff
- shift: run