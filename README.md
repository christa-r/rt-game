<img src="https://www.bu.edu/autism/files/2022/03/GEODE_Final_Logo_FullColor.jpg" alt="GEODE Logo" width="300"/>

## **Create a GitHub Account**
**1. Visit GitHub:**
Go to https://github.com.

**2. Sign Up:**
Click on the **Sign up** button and follow the on-screen instructions. You will be asked to choose a username, enter your email address, and set a password.

**3. Verify Your Email:**
GitHub will send you a verification email. Click the link in that email to verify your account.

## **Create a MongoDB Account**
MongoDB is a popular NoSQL database, and MongoDB Atlas offers a free cloud-hosted database service. MongoDB is where data from the website app (game) hosted on Heroku is stored.

**1. Visit MongoDB Atlas:**
Go to https://www.mongodb.com/cloud/atlas.

**2. Sign Up:**
Click on **Start Free** and fill in the required details to create your account.

## **Create a Heroku Account and Install the Heroku CLI**

Heroku is a cloud platform that lets you deploy, manage, and scale modern apps. This is where the game is hosted.

### **Create a Heroku Account**
**1. Visit Heroku:**
Go to https://www.heroku.com.

**2. Sign Up:**
Click on **Sign Up for Free** and complete the registration process with your details.

**3. Verify Your Email:**
Heroku will send you a verification email. Click the verification link to activate your account.

### **Install the Heroku CLI**
**1. Download the CLI:**
- **For Windows:**
  Download the installer from the [Heroku CLI download page](https://devcenter.heroku.com/articles/heroku-cli) and run it.
- **For macOS:**
  If you have Homebrew installed, open Terminal and run:
```bash
brew tap heroku/brew && brew install heroku
```
- **For Linux:**
  Follow the installation instructions for your Linux distribution on the [Heroku CLI documentation page](https://devcenter.heroku.com/articles/heroku-cli#install-for-linux).

**2. Verify the Installation:**

Open your terminal (or Command Prompt on Windows) and run:
```bash
heroku --version
```

You should see output with the Heroku CLI version, confirming that it is installed.

## **Clone Your Repository and Deploy to Heroku Using Git**
**1. Open your terminal** (Command Prompt on Windows, Terminal on macOS/Linux).

**2. Log into Heroku:**
```bash
heroku login
```

Follow the prompts in your browser to complete the login.

**3. Clone the Repository:**

```bash
heroku git:clone -a coindm
cd coindm
```

This command creates a new folder named coindm and places the source code there.

**3. Deploy Your Changes to Heroku**
```bash
git add .
```
```bash
git commit -am "make it better"
```
```bash
git push heroku main
```
## **Detailed Code Walkthrough**

**Lines 1–9: PIXI.js Setup and Aliases**

These lines create aliases for various PIXI.js classes and objects. For example:
- `Application` is used to create the main PIXI application.
- `Container` will be used to group display objects.
- `loader` and `resources` help with asset loading.
- `Graphics`, `Sprite`, `Text`, and `TextStyle` are used for drawing shapes and displaying images or text.

```javascript
let Application = PIXI.Application,
    Container = PIXI.Container,
    loader = PIXI.loader,
    resources = PIXI.loader.resources,
    Graphics = PIXI.Graphics,
    TextureCache = PIXI.utils.TextureCache,
    Sprite = PIXI.Sprite,
    Text = PIXI.Text,
    TextStyle = PIXI.TextStyle;
```

**Lines 11–16: Application Initialization**

These lines create a new PIXI Application with antialiasing (smoother graphics), transparency, canvas dimensions (1000×600 pixels) and a resolution of 1.

```javascript
let app = new Application({
    antialiasing: true,
    transparent: true,
    width: 1000,
    height: 600,
    resolution: 1
});
```

**Lines 18–19: Attaching the PIXI Canvas to the HTML Document**

These lines retrieve the HTML element with the ID `gameWindow` and append the PIXI canvas `(app.view)` to it so that the game renders on your webpage.

```javascript
var gameWindow = document.getElementById("gameWindow"); 
gameWindow.appendChild(app.view);
```

**Lines 21–22: Defining Size and Aspect Ratio for Resizing**

An array `size` is defined to hold the width and height. The `ratio` variable stores the aspect ratio (width divided by height) for later use in resizing.

```javascript
var size = [1000, 600];
var ratio = size[0] / size[1];
```

**Lines 24–40: Responsive Resizing**

- The `resize()` function calculates the appropriate canvas width `(w)` and height `(h)` based on the window dimensions to maintain the aspect ratio. This ensures the game always displays properly regardless of the screen size.
- It then applies CSS styles (position, width, height, left, top) to the canvas.
- Finally, `window.onresize` is set so that every time the window is resized, the canvas adjusts accordingly.

```javascript
resize()
function resize() {
    if (window.innerWidth / window.innerHeight >= ratio) {
        var w = window.innerHeight * ratio;
        var h = window.innerHeight;
    } else {
        var w = window.innerWidth;
        var h = window.innerWidth / ratio;
    }
    app.renderer.view.style.position = 'absolute';
    app.renderer.view.style.width = w + 'px';
    app.renderer.view.style.height = h + 'px';
    app.renderer.view.style.left = '25%';
    app.renderer.view.style.top = '1%'; 
}
window.onresize = resize;
```

**Lines 42–51: Smoothie and Flash Stage Setup**

- **Line 42:** Creates a new PIXI container `(flashStage)` for elements that flash on the screen during gameplay.
- **Lines 43–48:** Initializes Smoothie (an animation engine) that calls the `gameLoop` function at 50 frames per second. Smoothie ensures the game loop runs at a consistent speed.
- **Lines 49–51:** Initializes empty arrays for explosion and level transition animations. These arrays will later be populated with image URLs used for animated effects.

```javascript
var flashStage = new PIXI.Container(); // Container for flashing elements
var smoothie = new Smoothie({
  engine: PIXI,
  renderer: app,
  root: flashStage,
  update: gameLoop.bind(this),
  fps: 50,
});
var EXPLOSION_FRAMES = []; // Frames for explosion animation
var LIGHTSPEED_FRAMES = []; // Frames for level transition animation
```

**Lines 71–80: Asset Loading**

This block preloads multiple assets (images and JSON files) required by the game. Once loading is complete, the `setup()` function is automatically called to initialize game objects.

```javascript
PIXI.loader
  .add("images/galaguh.json")
  .add("images/enemy_inv.png")
  .add("images/close.png")
  .add("images/bg-control-pad.svg")
  .add("images/bg-control-angle-indicator.svg")
  .add("images/spaceship-body.png")
  .add("images/space-background.svg")
  .add("images/asteroid.png")
  .load(setup);
```

**Lines 82–83: Additional Asset Preparation**

These function calls prepare animation frames. In other words, they build arrays containing URLs to individual images that will be used to create frame-by-frame animations later in the game.
- `addExplosionFrames()` populates the array of explosion animation frames.
- `addLightSpeedFrames()` populates the array for level transition animations.

```javascript
addExplosionFrames(); // Populate explosion frame URLs
addLightSpeedFrames(); // Populate lightspeed frame URLs
```

**Lines 85–120: Global Variables and Input Setup**

Declares many global variables that will be used across different parts of the game.
- Variables like `hero,` `aliens,` and `spaceBackground` will hold PIXI sprites and containers.
The `keyboard()` function is called with key codes (65 for A, 68 for D, and 32 for Space) to set up event listeners for user input.

```javascript
let state, explosion, exit, player,
    door, healthBar, message, scoreMessage, gameOverScene, enemies, id;
let hero, aliens, spaceBackground, playerBullets, enemyBullets;

let left = keyboard(65); 
let right = keyboard(68); 
let spacebar = keyboard(32); 
```

**Lines 121–131: User Prompt with SweetAlert**

Uses the SweetAlert library to display a modal dialog that prompts the user for an ID. The entered ID is stored in the `userinput` variable and may be used for user tracking or data export later.

```javascript
Swal.fire({
    title: "Hello!",
    text: "Please enter the ID here:",
    input: 'text',
    showCancelButton: true,
    allowOutsideClick: false,
    allowEscapeKey: false      
}).then((result) => {
    if (result.value) {
        console.log("Result: " + result.value);
        userinput = result.value;
    }
});
```

**Lines 134–167: The `setup()` Function**

This function is called once all assets are loaded and is responsible for getting the game ready to run. It initializes data, sounds, UI elements, and starts the main loop.
- **Lines 134–136:** Generates a unique user ID by combining two random base‑36 strings.
- **Lines 138–141:** Sets up the `jsondata` object with the username, user input, and current time.
- **Lines 142–155:** Converts the `jsondata` object into a JSON string, sets the server endpoint `(/rt_shank3)`, and creates a CORS-enabled POST request. It verifies that the request is properly created, sets the request header to indicate JSON content, defines callbacks for handling both successful responses and errors, and finally sends the JSON data to the server.
- **Lines 157–161:** Creates sound objects for positive `(goodsound)` and negative `(badsound)` feedback and initializes counters like `trialCounter` and `DestroyedAlienCounter` to zero.
- **Lines 162–167:** Adds the `flashStage` to the main stage, sets the initial game state (`getHC`), and starts the Smoothie game loop.

```javascript
- function setup() {
    // Generate a unique user ID
    user = Math.random().toString(36).substring(2, 7) +
           Math.random().toString(36).substring(2, 7);
    
    // Initialize JSON data with user info and send it to the server
    jsondata.username = user;
    jsondata.uid = userinput;
    jsondata.start_time = Date();
    var data = JSON.stringify(jsondata);
    var url = '/rt_shank3';
    var xhr = createCORSRequest('POST', url);
    if (!xhr) {
        throw new Error('CORS not supported');
    }
    xhr.setRequestHeader('Content-Type','application/json');
    xhr.onload = function(){
        var text = xhr.responseText;
    };
    xhr.onerror = function(){
        alert("Error sending data to server");
    };
    xhr.send(data);
    
    trialArray = [];
    goodsound = new sound('spacelaser_trimmed.wav');
    badsound = new sound('failshoot.wav');
    trialCounter = 0;
    DestroyedAlienCounter = 0;
    app.stage.addChild(flashStage);
    
    // (Additional initialization for sprites, event listeners, and UI elements goes here.)
    
    state = getHC;
    smoothie.start();
}
```

**Game Loop Overview**

- **Game Loop Function:**
Called repeatedly by Smoothie at 50 FPS, this function updates the game by invoking the current state function (stored in `state`). The `delta` parameter helps ensure smooth animations.

```javascript
function gameLoop(delta){
    state(delta);
}
```

- **State Functions:**
Various functions (e.g., `play()`, `trial_init()`, `test_flash()`, `decision()`, `noresp()`, `wrongresp()`, `rightresp()`, `transition()`, `nextLevel()`) manage different phases of gameplay, such as updating positions, handling user responses, and transitioning between levels.

**Helper Functions Summary**
- `coinFlip(prob)`: This function returns `1` with probability `prob` and `0` otherwise. It is used to introduce randomness into the game (e.g., deciding which side to flash).
- `contain(sprite, container)`: Keeps a sprite within the boundaries of a container.
- `hitTestRectangle(r1, r2)`: Checks for collisions between two rectangular objects.
- `randomInt(min, max)`: Generates a random integer between `min` and `max`.
- `keyboard(keyCode)`: Sets up keyboard event listeners for a given key code.
- `addExplosionFrames()` and `addLightSpeedFrames()`: Populate arrays with URLs for frame-by-frame animations.
- AJAX Functions (`createCORSRequest` and `sendData`): Enable communication with a backend server to send and receive game data.
- `getHC()`: Retrieves high score data or performs other initial data requests from the server.
