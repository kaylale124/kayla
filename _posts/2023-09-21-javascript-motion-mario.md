---
title: JS Mario Animation
comments: true
hide: true
layout: default
description: Use JavaScript without external libraries to animate Mario moving across screen, OOP style.
courses: { csse: {week: 5} }
type: hacks
---

  <!--- Liquid concatentation --->
  <!--- Liquid list variable created from file containing mario metatdata for sprite --->
 <!--- Liquid integer assignment --->

<!--- HTML for page contains <p> tag named "mario" and class properties for a "sprite"  -->

<p id="mario" class="sprite"></p>
  
<!--- Embedded Cascading Style Sheet (CSS) rules, defines how HTML elements look --->
<style>

  /*CSS style rules for the elements id and class above...
  */
  .sprite {
    height: 256px;
    width: 256px;
    background-image: url('/{{site.baseurl}}/images/mario_animation.png');
    background-repeat: no-repeat;
  }

  /*background position of sprite element
  */
  #mario {
    background-position: calc( * 256 * -1px) calc( * 256* -1px);
  }
</style>

<!--- Embedded executable code--->
<script>
  ////////// convert yml hash to javascript key value objects /////////

  var mario_metadata = {}; //key, value object
    
  
  var key = "Rest"  //key
  var values = {} //values object
  values["row"] = 0
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

    
  
  var key = "RestL"  //key
  var values = {} //values object
  values["row"] = 1
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

    
  
  var key = "Walk"  //key
  var values = {} //values object
  values["row"] = 2
  values["col"] = 0
  values["frames"] = 8
  mario_metadata[key] = values; //key with values added

    
  
  var key = "Tada"  //key
  var values = {} //values object
  values["row"] = 2
  values["col"] = 11
  values["frames"] = 3
  mario_metadata[key] = values; //key with values added

    
  
  var key = "WalkL"  //key
  var values = {} //values object
  values["row"] = 3
  values["col"] = 0
  values["frames"] = 8
  mario_metadata[key] = values; //key with values added

    
  
  var key = "TadaL"  //key
  var values = {} //values object
  values["row"] = 3
  values["col"] = 11
  values["frames"] = 3
  mario_metadata[key] = values; //key with values added

    
  
  var key = "Run1"  //key
  var values = {} //values object
  values["row"] = 4
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

    
  
  var key = "Run1L"  //key
  var values = {} //values object
  values["row"] = 5
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

    
  
  var key = "Run2"  //key
  var values = {} //values object
  values["row"] = 6
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

    
  
  var key = "Run2L"  //key
  var values = {} //values object
  values["row"] = 7
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

    
  
  var key = "Puff"  //key
  var values = {} //values object
  values["row"] = 8
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

    
  
  var key = "PuffL"  //key
  var values = {} //values object
  values["row"] = 9
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

    
  
  var key = "Cheer"  //key
  var values = {} //values object
  values["row"] = 10
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

    
  
  var key = "CheerL"  //key
  var values = {} //values object
  values["row"] = 11
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

    
  
  var key = "Flip"  //key
  var values = {} //values object
  values["row"] = 12
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

    
  
  var key = "FlipL"  //key
  var values = {} //values object
  values["row"] = 13
  values["col"] = 0
  values["frames"] = 15
  mario_metadata[key] = values; //key with values added

  

  ////////// animation control object /////////

  class Mario {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario"); //HTML element of sprite
      this.pixels = 256; //pixel offset of images in the sprite, set by liquid constant
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

    startRunning() {
      this.stopAnimate();
      this.animate(this.obj["Run1"], 6);
    }

    startPuffing() {
      this.stopAnimate();
      this.animate(this.obj["Puff"], 0);
    }

    startCheering() {
      this.stopAnimate();
      this.animate(this.obj["Cheer"], 0);
    }

    startFlipping() {
      this.stopAnimate();
      this.animate(this.obj["Flip"], 0);
    }

    startResting() {
      this.stopAnimate();
      this.animate(this.obj["Rest"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario = new Mario(mario_metadata);

  ////////// event control /////////

  window.addEventListener("keydown", (event) => {
    if (event.key === "ArrowRight") {
      event.preventDefault();
      if (event.repeat) {
        mario.startCheering();
      } else {
        if (mario.currentSpeed === 0) {
          mario.startWalking();
        } else if (mario.currentSpeed === 3) {
          mario.startRunning();
        }
      }
    } else if (event.key === "ArrowLeft") {
      event.preventDefault();
      if (event.repeat) {
        mario.stopAnimate();
      } else {
        mario.startPuffing();
      }
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