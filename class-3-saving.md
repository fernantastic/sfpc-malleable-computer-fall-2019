# Class 3: Tools and Community

you should be able to quickly see what someone else made, maybe even remix it?

Load and save functionality: get a code to share

# Project page

[https://glitch.com/~sfpc-malleable-computer-saving](https://glitch.com/~sfpc-malleable-computer-saving)

# Code

## Create our scene description

Decide what we're going to store, how can we store and load later?

we'll use javascript object

this will be the ground truth for our scene

    var scene = {
      "backgroundImage": "",
      "fontColor": "#ff0000",
      "textSize": 20,
      "topText": "what",
      "bottomText": "is love"
    };

## Create a simple image, text editor

Add text size slider

draw text in the top and bottom from the input

    var colorPicker;
    
    // Toolbar
    createButton("Get random background").parent(toolbar).mousePressed(getRandomBackground);
    
    colorPicker = createColorPicker(scene.fontColor).parent("toolbar").input((i) => scene.fontColor = i.target.value);
    createDiv("Font Size").parent(toolbar);
    createSlider(12, 70, scene.textSize).parent(toolbar).input((i) => scene.textSize = int(i.target.value));
    
    createDiv("Top text").parent(toolbar);
    createInput(scene.topText).parent(toolbar).input((i) => scene.topText = i.target.value);
    createDiv("Bottom text").parent(toolbar);
    createInput(scene.bottomText).parent(toolbar).input((i) => scene.bottomText = i.target.value);

## Get a random image

Take an image from [https://source.unsplash.com/](https://source.unsplash.com/)

    // setup
    createButton("Get random background").parent(toolbar).mousePressed(getRandomBackground);
    
    function getRandomBackground() {
      fetch("https://source.unsplash.com/random/300x300")
        .then((response) => {
          scene.backgroundImage = encodeURIComponent(response.url);
          loadBackground();
      });
    }
    
    function loadBackground() {
      if (scene.backgroundImage.length = 0)
          return;
      var url = decodeURIComponent(scene.backgroundImage)
      if (url != undefined) 
        imageObj = loadImage(url);
    }

## Draw our scene

Our drawing code will just draw all the elements available to the screen

    function draw() {
      background(255);
      
      if (scene != undefined) {
        if (scene.backgroundImage.length > 0) {
          push();
          translate(width / 2, height / 2);
          angleMode(DEGREES);
          rotate(sin(millis() * 0.2) * 20);
          scale(1.25);
          translate(-width / 2, -height / 2);
          image(imageObj, 0, 0, width, height);
          pop();
        }
        
        fill(scene.fontColor);
        stroke(255);
        strokeWeight(4);
        textSize(scene.textSize + sin(millis()) * 2);
        textAlign(CENTER, TOP);
        text(scene.topText, 0, 5, width, scene.textSize);
        textAlign(CENTER, BOTTOM);
        text(scene.bottomText, 0, 0, width, height - 5);
      }
    }

Now we have to turn our scene into something we can save

and load that back into a regular scene

## Save our scene into JSON and turn into URL

javascript makes it easy to save an object into a JSON format

what is JSON

    // setup
    createButton("Get Link").parent(toolbar).mousePressed(saveLink);
    
    function saveLink() {
      var jsonString = JSON.stringify(scene);
      
      // get url from Share / Live App
      var url = PROJECT_URL;
      url += "?scene=" + encodeURIComponent(jsonString);
      
      document.getElementById("URL").value = url;
    }

Note: max is 4096 bytes?

## Load our JSON from the URL

    function preload() {
      loadScene();
    }
    
    function loadScene() {
      var params = new URLSearchParams(new URL(document.URL).search);
      if (params.has("scene")) {
        var uri = params.get("scene");
        var sceneObj = JSON.parse(uri);
        if (sceneObj != undefined) {
          scene = sceneObj;
          
          loadBackground();
        }
      }
    }
