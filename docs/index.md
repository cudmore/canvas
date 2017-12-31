Canvas is software to acquire and visualize spatially organized video and image volumes acquired with a scanning microscope.

This is most useful when performing repeated time-lapse imaging as it allows disjoint images to be acquired and then easily and rapidly returned to on subsequent imaging sessions across timescale of minutes, days, and months.

## Example 1: In vivo canvas

<IMG SRC="img/canvas-example-1.png" width=600>

In this example screen-shot, the gray images are an automatically created mosaic of surface vasculature images from a video camera looking down into a cranial window. The green squares are the position of image stacks acquired with a scanning microscope. The Canvas software seamlessly integrates with [Map Manager][map manager] providing point and click navigation, browsing, and annotations of all image stacks.

## Example 2: Ex vivo canvas

<IMG SRC="img/canvas-example-slice.png" width=475 align=left>

<IMG SRC="img/canvas-example-slice-2.png" width=300 align=right>

<div class="print-page-break"></div>

This is an example of a Canvas acquired during two-photon imaging and electrophysiology in a cerebellar slice. The first image is an overview of the canvas and the second is a zoomed in view of the recording and imaging areas.

## Overview
 - Create a Canvas that visually places each video image and scanned images into a unified ‘motor space’.
 - Previous canvases can be loaded and displayed using the Map Manager Stack Browser.
 - Quickly and easily acquire the same fields-of-view both within and between time-points spanning minutes, hours, days and months.

This is designed to work on any scope that has a computer controlled objective or stage. It has been tested using x/y/z objective motors using a Sutter mp285 controller and a Bruker Scope using Prairie View software.


## Hardware and Software Requirements

 - Windows 7 or Windows XP
 - Wavemetrics Igor Pro 6 or 7, [link][wavemetrics]
 - Motor controllers (to get/set the position of the objective
    - Sutter Instruments mp285, [link][mp285]
    - Bruker Prairie view software 
 - Scanning microscope software (to import images)
    - ScanImage, [link][scanimage]
    - Prairie View
 - Video acquisition
    - Any USB video camera

<IMG SRC="img/canvas-scope-panel.png" width=350 align=right>

## Interface

**X/Y/Z**. Gives the current position of the motor.

**Read Position**. Reads the current motor position.

**Zero X/Y** and **Zero Z**. Zero the current position in the Canvas software. This does not move the onjective but is useful to be able to read absolute distances moved.

**Arrow Buttons**. Move the motor the specified distance. All units are in um. The four arrows in the left pane move within the image plane (left, right, front, back). The two arrows in the right plane move in depth/z (up and down).

**X, Y, and Z Step (um)**. Set the distance (in um) taken when each of the arrow buttons are used to move the motor.

**Set Path**. Set the save path of the current canvas. This can automatically be set in a scripted user file.

**Initialize Session**. Initialize a new session with specified ‘Session ID’. Each new session will have its own canvas and save folder.

**HDD**. Show the hard-disk-drive folder for the current session.

**Display Canvas**. Display the current canvas. The current canvas is created when a session is initialized with ‘Initialize Session’.

**Finalize and clear canvas**. Used at the end of an imaging session to close the current canvas.

<div class="print-page-break"></div>

<IMG SRC="img/canvas-scope-options.png" width=350 align=right>

## Options Panel

### Video Group.

Specifies the size of a video image in both pixels and um.

### Refresh 2P Squares Group.

**Display This Channel**. The default color channel to display in the canvas.

**Auto Save Canvas and Notebook**. The canvas and notebook will be saved each time ‘Import From scope’ button is pressed.

### Serial Port (mp285) group.

Options to control the serial port for communication with a Sutter MP285 motor controller.

### TCP/IP (bruker) group

Options to control communication with the Bruker Prairie view software.

### Objective Group.

Each row is a different objective with:

 - Name. Name of the objective. Usually 20x, 40x, 5x, etc.
 - Width. Width of field of view of objective (um)
 - Height. Height of field of view of objective (um)
 - Stepx and Stepy. Distance the motor will move when a movement arrow button is pressed. These values also appear in the canvas motor bar.
 - voxelsize. For importing .tif files. This is the x/y voxel size (um) of a stack taken at 1x magnification and 1024 ‘pixels per line’, 1024 ‘lines per frame’. This value is used to linearly scale all other stack imports to arbitrary magnification and ‘lines per frame’/’pixels per line’.


## Acquiring video

If the video camera on your scope has an analog signal, there are very inexpensive video to usb converters. We regularly use EasyCap DC60 converters ($10-$20). Converters like this take an analog video signal, convert it to USB and then single frames or video is captured within Igor Pro using their free driver.

High end USB2/3 cameras will generally work but the size of the frame acquired by Igor may be smaller than the sensor. To circumvent this, we have a video viewer/capture script written in Matlab that saves full FOV images from a usb camera that is then automatically loaded into Igor.

## Controlling the objective

### Sutter mp285

Both ScanImage and Igor need to read the position and move the motorized objective (usually a Sutter mp285). This is done by communicating with one physical serial-port, a COM port. Microsoft Windows only allows one program to communicate with each physical serial-port but we need both Matlab and Igor to communicate with the same COM port. Thus ,we use third-party software to split the physical COM port into two virtual ports, one for Matlab and one for Igor.

There are a few different programs to create virtual COM ports from one physical COM port.

 - Windows 7: Serial Splitter 5.0 (Eltima Software), $100. As of Aug 2014 this is what we are using and it is working well.
 - Windows 7: Serial Port Splitter from www.fabulatech.com
 - Windows XP : Etima Software has a 32bit version, FREE
 - com0com,  an open source project. If anyone gets this working, please let us know. FREE

### Prairie View

The Canvas software communicates with Prairie View over a TCP/IP (localhost) connection using Igor's SOCKIT library.


## Known Problems

 - If you enter a space in your scanimage file name, your mp285 canvas will not work properly. If you realize this after you have already created a canvas with ‘Import Scanimage’, manually change the space in each scan image .tif file (on the hard-drive) to an underscore (_). Do NOT simply delete the space, you must change it to an underscore.

[map manager]: http://blog.cudmore.io/mapmanager
[wavemetrics]: www.wavemetrics.com
[mp285]: https://www.sutter.com/MICROMANIPULATION/mp285.html
[scanimage]: http://scanimage.vidriotechnologies.com/display/SIH/ScanImage+Home