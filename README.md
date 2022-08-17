# MAKING GAMES WITH JAVASCRIPT

Applying some patterns to write games with Javascript

## Platform type

    PC
    Console
    Mobile

## Game type

    Action
    Adventure
    Arcade
        space invaders
            - https://codepen.io/jonslater204/pen/LYWQbMa
    Educational
    Fighting
    Idle
    Music
    Platformers
    Puzzle
        https://codepen.io/t_afif/pen/JjLWpOJ
    Racing
    Role playing
    Shooter
    Simulation
    Sports
    Strategy

## FAMOUS GAME

    Pong
    Tic-tac-toe
    Snake
    Mario
    Space Invaders
    Asteroids
    Space Shooter
    Flappy Bird

## GAME CORE PRINCIPLE

## 1. Drawing somewhere: the canvas HtmlElement

Draw on the screen with the html canvas

```html
<canvas id="gc" width="400" height="400"></canvas>
```

- https://riptutorial.com/html5-canvas ★★★

## 2. Animations

            1. Init objects & positions
    ┌────▶ 2. Clear canvas                     ctx.clearRect(x,y,w,h);
    │      3. Draw shapes                      ctx.fillStyle = "gold"; ctx.fillRect(0, 0, 50, 80);
    │      4. Compute next positions
    └───── Goto step 2

```jsx
<body>
    <canvas id="canvas" width="512" height="512"></canvas>
</body>
<script>
window.onload=(function(){
    var canvas=document.getElementById("canvas");
    animate();

    function animate(time) {
        ctx.clearRect(0,0,cw,ch);        // 1. clear the canvas
        x++; y++;                        // 2. update animation rules 
        ctx.drawImage(img,x,y);          // 3. draw on the screen
        requestAnimationFrame(animate);  // 4. When you need something to draw: request another loop of animation
    }

}); 
</script>
```

### Animation Framerate: How often do we need to paint?

The temporal sensitivity and resolution of human vision varies between individuals.  
The human visual system can process 10 to 12 images per second and perceive them individually, while higher rates are perceived as motion.

Animation rule is to repeatedly clearing the canvas and repainting it faster than human eye detection
The eye can see a difference between a delay of 40ms (brain image process delay). 

## 3. THE GAME LOOP

- Draw the scene  
- Detect user interaction: keyboars/mouse events…
- Update objects positions ( AI…)
- Check game rules: stop the game, limit objects movements, start adding elements... 

❌ ANTI-PATTERN

Avoid JavaScript timer functions setTimeout() and setInterval(), they are not adequate for animation
- window.setTimeout()       let you run some code after a certain delay 
- window.setTimeinterval()    let you run some code every so often 

```js
function game() {
    player.x +=1;
    …
}
window.onload=function() {	
	document.addEventListener("keydown",keyPush);
	setInterval(game,1000/15); // 15x/sec
}
```

✔️ DESIGN PATTERN

Use the requestAnimationFrame API (rAFs) instead of setInterval(action, freq). 
It tell your system to wait for the next occasion to draw to execute a given function

It's a way of rate-limiting the execution of a function.
Start/Cancelation of rAFs is our responsibility  
If the browser tab is not active, it would not execute. Although for scroll, mouse or keyboard events this doesn't matter.
Rule of thumb: use requestAnimationFrame if your js function is "painting" or animating directly properties, use it at everything that involves re-calculating element positions.

```js
function game() {
    player.x +=1;
    …
}
window.onload=function() {	
	document.addEventListener("keydown",keyPush);
	window.requestAnimFrame(function() { draw(angle) });
}
```

Another way

```js
window.onload=(function(){

    // animation interval variables
    var nextTime=0;      // the next animation begins at "nextTime"
    var duration=1000;   // run animation every 1000ms

    requestAnimationFrame(animate);

    function animate(currentTime){

        // wait for nextTime to occur
        if(currentTime<nextTime){
            // request another loop of animation
            requestAnimationFrame(animate);
            // time hasn't elapsed so just return
            return;
        }
        // set nextTime
        nextTime=currentTime+duration;

        // add another rectangle every 1000ms
        ctx.fillStyle='#'+Math.floor(Math.random()*16777215).toString(16);
        ctx.fillRect(x,30,30,30);

        // request another loop of animation
        requestAnimationFrame(animate);
    }

}); 
```

```js
<script>
		(function() {
			
			// Get a regular interval for drawing to the screen
			window.requestAnimFrame = (function (callback) {
				return window.requestAnimationFrame || 
							window.webkitRequestAnimationFrame ||
							window.mozRequestAnimationFrame ||
							window.oRequestAnimationFrame ||
							window.msRequestAnimaitonFrame ||
							function (callback) {
							 	window.setTimeout(callback, 1000/60);
							};
			})();
            
            // Allow for animation
			(function drawLoop () {
				requestAnimFrame(drawLoop);
				renderCanvas();
			})();

		})();
	</script>```

To make Ajax requests, or deciding if adding/removing a class (that could trigger a CSS animation), I would consider _.debounce or _.throttle, where you can set up lower executing rates (200ms for example, instead of 16ms)

## 4. USER INTERACTIVITY
### CONTROLLERS

    keyboard
    mouse
    game controller 
    webcam-gesture (AI)

## 5. GAME ENGINES

    Unity

### LIBS

## 6. PHYSICS ENGINE


### MORE


- https://phaser.io/
- https://makejsgames.com/

- https://arcade.makecode.com
- https://www.noesisengine.com/
- https://github.com/microsoft/gdk
- https://store.steampowered.com/about/
- https://godotengine.org/features
- https://trackingjs.com/examples/color_hexgl.html
- https://www.etr.fr/app/space-battle-vr/
- https://www.codeproject.com/Articles/5252990/Space-Invaders-in-Csharp-WinForm
- https://rupie.io/
- https://blogs.unity3d.com/2017/07/11/introducing-unity-2017/
- https://microsoft.github.io/Web-Dev-For-Beginners/pdf/readme.pdf

- https://blogs.msdn.com/b/davrous/archive/2013/11/19/using-webgl-to-create-games-for-the-windows-store.aspx
- https://blogs.msdn.com/b/davrous/archive/2012/03/16/html5-gaming-animating-sprites-in-canvas-with-easeljs.aspx
- https://blogs.msdn.com/b/davrous/archive/2013/07/23/introducing-babylon-gamefx-a-framework-to-build-html5-webgl-games-in-a-few-lines-of-code.aspx

- https://www.smashingmagazine.com/2021/10/real-time-multi-user-game/
