# AR editor target for Microsoft MakeCode

AR editor target for
[Microsoft MakeCode](https://github.com/Microsoft/pxt)

## Running locally

These instructions allow to run locally to modify the sample.

### Setup

The following commands are a 1-time setup after synching the repo on your machine.

* install the PXT command line
```
npm install -g pxt
```
* install typings
```
npm install -g typings
```
* install the dependencies
```
npm install
```
* install typings
```
typings install
```

### Running the local server

After you're done, simple run this command to open a local web server:
```
pxt serve
```

After making a change in the source, refresh the page in the browser.

## Updating the tools

If you would like to pick up the latest PXT build, simply run
```
pxt update
```

More instructions at https://github.com/Microsoft/pxt#running-a-target-from-localhost 

## Creating & Initializing an AR marker
In order to have multiple trackable markers in a single scene, we will want to use barcodes. ARToolkit has support for various other marker systems, but 2D barcodes give us a way to uniquely identify different objects. For now, using a 3x3 matrix for the barcodes seems to give us the best resolution for detecting markers while also giving us up to 64 unique barcode patterns. If you would like to change the marker detection mode, simply change the parameters for when we initialize the a-scene in simulator.html.

```
<a-scene embedded artoolkit='sourceType: webcam; detectionMode: mono_and_matrix; matrixCodeType: 3x3;' id='a-scene'>
    <a-entity camera></a-entity>
</a-scene>
```    

The easiest way to generate a marker is to use ARToolKit's 2D barcode marker generator: http://www.artoolworks.com/support/applications/marker/  
The most important settings to keep the same are border size, border color, barcode dimensions, and error checking/correction type.  
Border size: 0.25  
Border color: "Markers have black borders"  
Barcode dimensions: 3x3  
Error checking and correction type: None  

Once you have fixed your settings, choose a number between 0 and 63 for the barcode. For this example, let's do 20. When you've generated your picture you should have a black square with a few white squares inside of it. Either print it out or draw it using a ruler and a black sharpie.

Now that you've generated and have a marker, we need to write some code so that we can track it.

First, create a marker element.  

```
var markerEl = document.createElement('a-marker');
```

Then, set the marker element's attributes type and value. Type should be barcode, and value will be 20 for this example. If you used a different value in the previous step when you generated your marker, use that value instead.
```
markerEl.setAttribute('type', 'barcode');
markerEl.setAttribute('value', '20');
```

Next, we'll add some code for a nice purple box that will show up on top of the marker when it's detected.

```
var boxEl = document.createElement('a-box');
boxEl.setAttribute('material', 'opacity: 0.75; side: double; color:purple;');
markerEl.appendChild(boxEl);
```

Finally, we need to add our marker to the scene.

```
var sceneEl = document.querySelector('a-scene');
sceneEl.appendChild(markerEl);
```

