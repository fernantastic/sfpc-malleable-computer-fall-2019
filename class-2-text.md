# Class 2: Text

# References

- [TEXTREME](https://le-von.itch.io/textreme) -  ****juicy text editor
- Creative Writer 2 (can be found on internet archive)
- WordPalette - corpus-based suggestions

## Homework

create a one page text (short poem, story, etc.) made in your own tool. Add brushes or editor features to create the writing or visual style you want.

# Goals

Let's add text to our drawing tool

How can we make an interactive poetry tool?

Can it be used for performance?

Can it actually help you write?

# Code

**Go to** 

> [**https://glitch.com/~sfpc-malleable-computer**](https://glitch.com/~sfpc-malleable-computer)

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

### Pixel copying / modifying

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

## Language

### add a form and a toolbar option

    <textarea id="corpus" cols="70" rows="10" type="text">Your text</textarea>
    
    brushButtons.push(createButton("Text").parent(toolbar).mousePressed(() => setBrush(4) ));

### go to gutenberg, find a corpus

### add a text brush

    var lastTextChar = 0;
    var corpus;
    
    // draw
    else if (currentBrush == 4) { // Text
      corpus = document.getElementById("corpus")
    	strokeWeight(0);
      fill(brushColor);
      textSize(15);
      if (lastTextPoint == undefined) {
        text(corpus[lastTextChar], drawPoint.x, drawPoint.y);
        lastTextChar = (lastTextChar + 1) % corpus.length;
      }
    }

### add characters when far from the last point

    var lastTextPoint;
    var lastTextChar = 0;
    var corpus;
    
    // draw
    else if (currentBrush == 4) { // Text
      corpus = document.getElementById("corpus");
    	strokeWeight(0);
      fill(brushColor);
      textSize(brushSize * 2);
      if (lastTextPoint == undefined || 
          lastTextPoint.dist(drawPoint) > brushSize * 2) {
        lastTextPoint = drawPoint;
        text(corpus[lastTextChar], drawPoint.x, drawPoint.y);
        lastTextChar = (lastTextChar + 1) % corpus.length;
      }
    }

### choose word by word

    corpus = document.getElementById("corpus").value.split(' ');
    //...
    text(corpus[lastTextChar], drawPoint.x, drawPoint.y);

### make text rotate too

    // on draw, pressed
    translate(drawPoint.x, drawPoint.y);
    rotate(radians(random(-sliderRotation.value(), sliderRotation.value())));
    lastTextPoint = drawPoint;
    text(corpus[lastTextChar], 0, 0);
