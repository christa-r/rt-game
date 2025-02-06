<img src="https://www.bu.edu/autism/files/2022/03/GEODE_Final_Logo_FullColor.jpg" alt="GEODE Logo" width="300"/>

## **Create a GitHub Account**
**1. Visit GitHub:**
Go to https://github.com.

**2. Sign Up:**
Click on the **Sign up** button and follow the on-screen instructions. You will be asked to choose a username, enter your email address, and set a password.

**3. Verify Your Email:**
GitHub will send you a verification email. Click the link in that email to verify your account.

## **Create a MongoDB Account**
MongoDB is a popular NoSQL database, and MongoDB Atlas offers a free cloud-hosted database service. MongoDB is where data from the website app hosted on Heroku is stored.

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
  Download the installer from the Heroku CLI download page (https://devcenter.heroku.com/articles/heroku-cli) and run it.
- **For macOS:**
  If you have Homebrew installed, open Terminal and run:
```bash
brew tap heroku/brew && brew install heroku
```
- **For Linux:**
  Follow the installation instructions for your Linux distribution on the Heroku      CLI documentation page (https://devcenter.heroku.com/articles/heroku-cli#install-for-linux).

**2. Verify the Installation:**

Open your terminal (or Command Prompt on Windows) and run:
```bash
heroku --version
```

You should see output with the Heroku CLI version, confirming that it is installed.

## **Download or Clone the Game Repository**
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

**Lines 1–9:**
These lines create shortcut variables for various PIXI.js classes and objects. For example:
- `Application` is used to create the main PIXI application.
- `Container` will be used to group display objects.
- `loader` and `resources` help with asset loading.
- `Graphics`, `Sprite`, `Text`, and `TextStyle` are used for drawing shapes and displaying images or text.
