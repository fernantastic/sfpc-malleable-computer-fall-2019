# Class 1: Drawing

# References

- P5 JS reference for an Image object: [https://p5js.org/reference/#group-Image](https://p5js.org/reference/#group-Image)
- Dan Schiffmann [Painting with Pixels](Daniel Schiffman)
- P5 JS reference [https://p5js.org/reference/](https://p5js.org/reference/)

# Start

Optional: Make groups of two. We'll pair program: if one of you knows how to code let the other person be in charge of writing it.

**Go to** 

> [**https://glitch.com/~sfpc-malleable-computer**](https://glitch.com/~sfpc-malleable-computer)

### draw

    // draw
    if (mousePressed)
    	line(mouseX, mouseY, pmouseX, pmouseY);

### add clear button

    // setup
    createButton("Clear").parent(toolbar).mousePressed(clear);
    
    function clear() {
    	background(255);
    }

### add save button

    
    function save() {
      saveCanvas('myCanvas', 'png');
    }

### Adding a slider for radius

    var sizeSlider;
    
    // setup
    createDiv("Brush Size:").parent(toolbar);
    sizeSlider = createSlider(1, 50, 1).parent(toolbar);
    
    //draw
    strokeWeight(sizeSlider.value());

### add randomness to brush position

    var drawPoint = createVector(mouseX, mouseY);
    drawPoint.x += random(-1,1) * randomnessSlider.value();
    drawPoint.y += random(-1,1) * randomnessSlider.value();

### add color picker

    // setup
    colorPicker = createColorPicker([128]).parent(toolbar);
    
    // draw
    stroke(colorPicker.color());

### add brush option toolbar

    var currentBrush;
    var brushButtons = [];
    
    // setup
    createDiv("Brush:").parent(toolbar);
    
    var buttons = ["line", "circle"]
    brushButtons.push(createButton("Line").parent(toolbar).mousePressed(() => setBrush(0) ));
    brushButtons.push(createButton("Circle").parent(toolbar).mousePressed(() => setBrush(1) ));
    brushButtons.push(createButton("Square").parent(toolbar).mousePressed(() => setBrush(2) ));
    setBrush(0);
    
    // draw
    if (currentBrush == 0) {
      line(pmouseX, pmouseY, drawPoint.x, drawPoint.y);  
    } else if (currentBrush == 1) { // circle
      strokeWeight(1);
      ellipse(drawPoint.x, drawPoint.y, brushSize);
    }
    
    function setBrush(brush) {
      currentBrush = brush;
      for(var i = 0; i < brushButtons.length; i++) {
        brushButtons[i].style("opacity: " + (currentBrush == i ? 1 : 0.5));
      }
    }

## Fun stuff

### randomize color

    var brushColor = colorPicker.color();
    var v = random(-30, 30);
    brushColor.setRed(red(brushColor) + v);
    brushColor.setGreen(green(brushColor) + v);
    brushColor.setBlue(blue(brushColor) + v);
    stroke(brushColor);

### add a stamp

    // search for stamp on google images
    // add to public/stamp.png
    
    // setup
    stamp = loadImage('https://cdn.glitch.com/89a8eb0f-e2ce-4acc-9618-514e19833c6b%2Fstamp.jpg?v=1572821537755');
    
    // draw
    } else if (currentBrush == 2) { // stamp
    	var w = brushSize * 10;
    	var h = brushSize * 10 / (stamp.width / stamp.height);
    	translate(drawPoint.x, drawPoint.y);
    	rotate(radians(random(-sliderRotation.value(), sliderRotation.value())));
    	tint(brushColor);
    	image(stamp, -w / 2, -h / 2, w, h);
    }

### add filter buttons

Reference: [https://p5js.org/reference/#/p5/filter](https://p5js.org/reference/#/p5/filter)

    //setup
    createDiv("Filters:").parent(toolbar);
    createButton("Blur").parent(toolbar).mousePressed(() => filter(BLUR, 2));
    createButton("Erode").parent(toolbar).mousePressed(() => filter(ERODE));
    createButton("Threshold").parent(toolbar).mousePressed(() => filter(THRESHOLD));

### custom filters

    // steup
    createButton("Randomize").parent(toolbar).mousePressed(() => {
      var img = get();
      img.loadPixels();
      
      for(var x = 0; x < width; x++) {
        for(var y = 0; y < height; y++) {
          var pixelColor = getColor(img, x, y);
          if (alpha(pixelColor) == 0)
              continue;
          
          pixelColor.setRed(red(pixelColor) + random(-100,100));
          pixelColor.setGreen(green(pixelColor) + random(-100,100));
          pixelColor.setBlue(blue(pixelColor) + random(-100,100));
          
          set(x, y, pixelColor);
        }
      }
      updatePixels();
    });

### clone pixels

    // draw
    else if (currentBrush == 3) { // Clone
      var chunk = get(
        drawPoint.x + random(-brushSize * 2, brushSize * 2), 
        drawPoint.y + random(-brushSize * 2, brushSize * 2), 
        brushSize,
        brushSize
      );
      set(
        drawPoint.x - brushSize / 2, 
        drawPoint.y - brushSize / 2,
        chunk
      );
      updatePixels();
    }

# Etc

More fun things to do:

- Reflect the drawing point around the center to create a kaleidoscope

### Drag and drop an image to load

    // setup
    canvas.drop(onDropFile);
    
    function onDropFile(file) {
      if (file.type == 'image') {
        var img = createImg(file.data, '').hide();
        // draw once
        image(img, 0, 0, img.width, img.height);
      }
    }
