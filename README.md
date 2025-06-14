# Canvas2APNG for Module JS

Canvas2APNG lets you make APNG animations with pure Javascript.
One JS file is needed to create animations of your changing canvas.
You can play the animation directly on your page or save it as a local
file (extension .png).  <br/>
For more info about APNG see: https://en.wikipedia.org/wiki/APNG

# Demo
Animation examples generated by Canvas2APNG.

![Basic animation.](Demo/demo_animation_basics.png)&nbsp;&nbsp;&nbsp;&nbsp;![Clock animation.](Demo/demo_animation_clock.png)
<br/>
Most browsers support the playing of APNG animations.  <br/>
If these images are not animating, the animation is stopped (after 7 times) or your browser does not support APNG.

# News
**06 aug 2017**: <br/>
First publish of my CANVAS to APNG encoder to generate animations of a changing Html5 canvas element. <br/> <br/>
**13 aug 2017**: <br/>
I have found a bug. The bug occurs when the canvas image is too complex. Then more than one so called IDAT and fdAT chunks are present. This causes an error while reading the APNG file. I hope I will soon have a solution for it. <br/> <br/>
**14 aug 2017**: <br/>
Solution implemented for the bug of yesterday. <br/> <br/>
**18 aug 2017**: <br/>
An application has been realised in my [Chess Diagram Maker](http://svg_experimenten.deds.nl/chessboard/chess_diagram_maker.html) where online an animation of chess can be created (GIF or PNG). 
A similar application is realised in my [Draughts Diagram maker](http://svg_experimenten.deds.nl/draughtboard/draughts_diagram_maker.html). <br/>
![Example of a chess animation.](Demo/chess_scholars_mate3.png)

# Basic Usage

1. First you need to import the JS file in your js code: <br/>
      ``import { APNGencoder } from './canvas2apng.js';``

3. To create an APNG animation you need a canvas element in your html: <br/> 
   ``<canvas id="myCanvas" >  </canvas>`` <br/>
   In your script define: <br/>
   ``const canvas = document.getElementById("myCanvas");``

4. Create an encoder object in your script: <br/>
      ``const encoder = new APNGencoder(canvas);``

5. The main structure of your script should be: <br/>
      * Set encoder in start modus: ``encoder.start();``
      * For each change of the canvas add a frame to the animation: ``encoder.addFrame();``
      * Stop encoder, animation ready: ``encoder.finish();``

6. While writing your canvas changes, you specify moments to write the canvas data to the encoder with the ``addFrame()`` function. Each time you add a frame to the encoder, the canvas image will be added to the animation.

7. After finishing the encoder you can save the animation data to an image element.  <br/>
   Or you can download the animation as a local file (extension .png).  <br/>
   See the demo application how to do that.

# Encoder commands
* Define an instance of the APNGencoder object. Parameter is the canvas element. <br/>
  ``const encoder = new APNGencoder(canvas);``

* Set properties of the animation. At start or somewhere between adding frames.
  * Number of animation loops. Infinite loop is 0. <br/>
    ``encoder.setRepeat(2);``
  * Delay between frame in multiples of 1/100 sec. <br/>
    ``encoder.setDelay(80);``
  * Set dispose attribute (values 0,1,2). See APNG specs.  <br/>
    ``encoder.setDispose(0);``
  * Set blend attribute (values 0,1). See APNG specs.  <br/>
    ``encoder.setBlend(1);``

* Processing commands.
  * Set encoder in start modus. Ready for adding frames to the animation.  <br/>
    ``encoder.start();``
  * Add frames to the animation.  <br/>
    ``encoder.addFrame();``
  * Stop encoder, animation ready.  <br/>
    ``encoder.finish();``
  * Get the result of the animation. The result is an array of unsigned integers representing a APNG animation. <br/>
    With ``encoder.stream()`` you get an instance of the ByteArray object with some useful methods. <br/>
    The ``encoder.stream().bin`` command gives the array of the animation. 

# ByteArray
To facilitate processing, a special object ByteArray is defined. The animation is stored in an instance of ByteArray, part of the application. It hold an array of unsigned integers with some specific methods to process PNG data. <br/>
The resulting ByteArray can be asked by ``encoder.stream()`` and the array by ``encoder.stream().bin``  <br/>
Some useful methods facilitates debugging and converting the instance of a ByteArray object.
* ``bin``
* ``toStrHex()``
* ``toStrBase64()``
* ``toStrAscii()``
* ``toStrDec()``

And some useful functions: 
* ``decimalToHex()``
* ``bytesToBase64()``
* ``base64ToBytes()``

# Display the animation
To display the animation at your page use an image element and define the ".src" attribute as:
  * ``const base64Out = bytesToBase64(encoder.stream().bin);``
  * ``img.src = "data:image/png;base64," + base64Out;``   <br/>

You can also download (save) the animation as local PNG file by using a link element (\<a\>) with
download attribute. See the function ``downloadAPNG(iLink)`` in the demo application.

# CRC32 Calculation
A function is included to calculate CRC32 values (see code).
