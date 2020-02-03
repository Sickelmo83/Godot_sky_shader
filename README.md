# Godot_sky_shader
This project adds a dynamic sky shader to your project. It is based on https://github.com/danilw/godot-utils-and-other.

Complete feature list
* day-night-cycle
* Sun
* Moon
* Stars
* Clouds
* `work in progress` God Rays (via [God Rays Plugin](https://github.com/SIsilicon/Godot-God-Rays-Plugin) by @SIsilicon)
  * the combination of both has a noticable performance impact

[![sky](https://github.com/Lexpartizan/Godot_sky_shader/blob/master/preview1.jpg)](https://www.youtube.com/watch?v=fzUHa1BbOd4) 

The Third Person Controller used in the demo scene uses the code from https://github.com/NIK0666/GodotThirdPersonController, as featured in the following video: https://youtu.be/jxtUtUo4aEI

# Support

The shader and demo scenes target Godot 3.2.  
There is a *.zip file which contains a subset of this project's content and works in Godot 3.1.

# Demo

The demo scene Sky.tscn showcases some of the abilities of this dynamic sky.

Download the project (explained in the next chapter) and open it via Godot Editor.

Parameters in the Sky scene:
* Moon Phase: Covers the moon with a shadow from top-left of the moon to the bottom-right (and vice-verca)
* Coverage: Specifies how much the sky is covered by clouds
* Height: How close the clouds are to the viewer
* Quality Steps: If the steps are < 20, the 3D clouds are exchanged with 2D clouds
* Wind Strength: Speed of cloud movement
* Lightning Strike: brightens the sky for less than a second

Additional changes can be made which are not currently exposed in the GUI of the demo scene, such as
* traversal route (speed & direction) of sun and moon
* wind direction, ie. direction the clouds move

# How-to use it in your project

Downloading for Godot 3.2
1. Download the project using a) Git via command line or b) via the projects Code webpage on Github using the "Clone or download" button and then "Download ZIP"

Downloading for Godot 3.1
1. Download the zip called "Godot_sky_shader-3.1.2.zip" by clicking on it on the projects Code webpage
2. After the download, navigate in your File Manager to the downloaded zip file and extract the zip file to get all the project files you need

Preparation
1. Open your Godot project files using your file manager
2. move the files Sky.tscn as well as the complete folder called "shaders" into your project via the file manager
3. when selecting the Godot Editor again (or when opening your project with the Godot Editor), the files will be imported automatically

Using as it is
* you can use the Sky.tscn scene itself: It has a GDscript attached, which gives you controls to adjust the settings. When you are satisfied, you can hide the node called `Control` (it contains the GUI elements for adjusting your settings for the dynamic sky shader)
* you need to keep the same file structure as on Github, if you want to use the Sky.tscn scene
* your environment file (when creating a new project, the Godot Editor creates a new `default_env.tres` file for you) needs to have in its Background section the value `Sky` for `Mode` (this is currently the default setting) and you need to create a new `PanoramaSky` in the dropdown menu next to `Sky`
  * this is a bit confusing, because Mode has the value Sky and Sky has the value PanoramaSky. But it should look like this in your Inspector
    * `Background`
      * `Mode: Sky`
      * `Sky: PanoramaSky`
* you need to press on the button for "Play Scene" in the Godot Editor, as this scene is not your main scene!

required File structure in your project (so that the file Sky.tscn can find its dependencies):  
```
/shaders (folder with shaders)
  /CloudlessSky.shader
  /Clouds.shader
  /Sky.shader
  /fire.shader
  /noise.png
  /noise.png.import
Sky.tscn
default_env.tres
icon.png
```

The other files are for demonstration purposes.

Changing the file structure will break the dependencies for the file Sky.tscn! You will have to fix this yourself, then.


Example on how to set up the shaders in your project if you don't want to use the given `Sky.tscn` file:
1. You need a scene where you want to use the dynamic sky shaders
2. add a WorldEnvironment node to your root node, if you don't already have one
   1. select this node and set the reference to the `Environment` field: This should be your `default_env.tres` file (or however you renamed it)
   2. click on the *.tres file you just referenced and do the following changes to your settings in the `Background` section:
      1. set `Mode` to `Sky` via the dropdown menu
      2. for the field below `Mode` called `Sky`, create a new to `PanoramaSky` via the dropdown menu
3. add a light node to your root node, if you don't already have one, e.g. a directional light
4. add one Viewport node to your root node, and add a Sprite node as a child to this Viewport -> This setup is for the sky
   1. select the Sprite node you just created
   2. navigate to the CanvasItem section
   3. in the Material submenu you need to create a new ShaderMaterial
   4. open the newly created ShaderMaterial, click on the dropdown menu next to the field called `Shader` and load the CloudlessSky.shader file
5. add another Viewport node to your root node, and add a Sprite node as a child to this Viewport -> This setup is for the clouds
   1. select the Sprite node you just created
   2. navigate to the CanvasItem section
   3. in the Material submenu you need to create a new ShaderMaterial
   4. open the newly created ShaderMaterial, click on the dropdown menu next to the field called `Shader` and load the Clouds.shader file
6. Take the GDScript code from the `Sky.tscn` file and create a new script for your root node
7. testing: either press the button for "Play Scene" in the Godot Editor, or set this scene as as your main scene!

# Known Issues

* Stretched sun and moon (see issue #5)
* Flickering on the cloud edges
* A vertical seam on the sphere of the PanoramaSky is visible when the clouds move over it (see issue #2)