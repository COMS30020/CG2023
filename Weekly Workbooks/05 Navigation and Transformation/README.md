## Navigation and Transformation
### <a href='https://www.ole.bris.ac.uk/webapps/blackboard/content/contentWrapper.jsp?course_id=_257203_1&displayName=Re%2FPlay%20Collections&href=%2Fwebapps%2Fblackboard%2Fexecute%2Fblti%2FlaunchPlacement%3Fblti_placement_id%3D_3169_1%26course_id%3D_257203_1%26mode%3Dcpview%26wrapped%3Dtrue' target='_blank'> Weekly Briefing ![](../../resources/icons/briefing.png) </a>
### Task 1: Introduction


This workbook continues from where we left off last week - implementing additional features for the rasterising renderer. We start by adding texture mapping to the existing flat-colour renderer. Following on from that, we make the renderer interactive, so that the user can navigate the camera around the model. Note that if you didn't complete all of the tasks from last week, you should finish these first before starting on this week's workbook.  


# 
### Task 2: Camera Position
 <a href='02%20Camera%20Position/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='02%20Camera%20Position/slides/segment-2.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='02%20Camera%20Position/slides/segment-3.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='02%20Camera%20Position/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='02%20Camera%20Position/audio/segment-2.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='02%20Camera%20Position/audio/segment-3.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a>

Your render of the Cornell Box is probably looking quite good by now. The problem is that in order to _really_ test out your rendering code, you need to view the model from a number of different angles. Take a look and the slides and audio narration above relating to translation and rotation (there looks like a lot of material, but each file is only short !). Once you have an understanding of the principles, add some key event handlers to your code that allow a user to interactively:

- _translate_ the camera position in 3 dimensions (up/down, left/right, forwards/backwards)
- _rotate_ the camera position about the centre of the scene (about-the-X-axis and about-the-Y-axis)

Providing translations and rotations of the camera allows you to see how your render looks from different points of view. Note that it is essential your code only manipulates the **position of the camera** (i.e. you do not alter the vertices of the model). There is no need to implement rotation about the Z axis for this exercise - this just makes navigation messy and complex for the user.

Translating the position of the camera is relatively easy - it's just a case of adding (or subtracting) a suitable amount to the x, y, or z coordinate of the camera position `vec3`. Rotation on the other hand is a little more complex - you will need to create a 3x3 rotation matrix (using the `mat3` class from GLM) to apply to the camera position. Take a look at <a href="https://en.wikibooks.org/wiki/GLSL_Programming/Vector_and_Matrix_Operations" target="_blank">this documentation on matrices</a> to find out how to create and manipulate matrices in GLM.  


**Hints & Tips:**  
When instantiating a `mat3`, remember that GLM is _column major_ (i.e. the order the parameters are passed in should be first column, then second column, then third).

GLM vectors and matrices both override the `*` operator, so we can simply multiply the rotation `mat3` and the camera position `vec3` together to get the new camera position `vec3` !

Note that every time you change the position of the camera, the appearance of the render will change. As a result, you will need to re-render the entire scene (i.e. clear the pixels and redraw all of the triangles) every time the camera is moved. Rather than the event handler containing the re-drawing code, standard practice is for all drawing code to appear inside the `draw` function (the clue is in the name !). Note that there is no need for your event handler to explicitly call this drawing function - it is already called inside the main loop (assuming you haven't changed the code in the template). This may seem inefficient - since the whole scene is frequently redrawn, irrespective of whether or not the camera has moved. However (as we shall see later) various different processes can cause the camera to move (in addition to just key presses from the user) so it is useful to have the render refreshed on a regular basis.  


# 
### Task 3: Camera Orientation
 <a href='03%20Camera%20Orientation/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='03%20Camera%20Orientation/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='03%20Camera%20Orientation/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

When you start to move the camera around using the features added in the previous task, you quickly get to the point where the model is no longer visible from the camera position. What we really want is to be able to rotate the _orientation_ of the camera (i.e. panning and tilting) so that we can keep the model in view.

View the slides, audio narration and animation linked to above relating to camera orientation. Then add a new `mat3` matrix to your code to hold the current right/up/forward orientation of the camera. Use this orientation matrix to adjust the camera-to-vertex direction vectors when projecting vertices onto the image plane.

In order to fully test out these new features, add event handlers to your code so that the user can alter the orientation of the camera using suitable keys on the keyboard: rotating the camera in the Y axis (panning) and X axis (tilting). Again, rotation about the Z axis makes navigation messy and complex for the user, so there is no need to implement this in your code.

  


**Hints & Tips:**  
When altering the camera orientation, we again need to create a 3x3 rotation matrix (just as we did for manipulating the camera position). Similarly, we can make use of the `*` operator to multiply together the rotation matrix and the camera orientation matrix to calculate the new camera orientation.  


# 
### Task 4: Orbit
 <a href='04%20Orbit/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

It soon becomes a pain to have to manually move the camera around the scene using the keyboard each time we want to get a different perspective on a model. To ease this problem (and to make testing of your renderer easier) we will now include some additional code that automatically animates the position of the camera.

More specifically, add some code to your project that rotates ("orbits") the position on the camera (about the Y axis) around the centre of the Cornell Box. See the animation linked to above for an illustration of what you are aiming to achieve - this is a view from above showing the orbiting of the camera around the model (note that there is no audio narration in this video).

Implementing the "orbit" feature will involve applying a very small rotational transformation to the _camera position_ every time your code iterates through the main loop. As such, you _could_ put the additional "orbit" code inside the loop in the `main` method. _However_ it is standard practice (and arguably tidier) to put the orbit code inside the `draw` function (which already gets called during each iteration of the main loop).  


# 
### Task 5: LookAt
 <a href='05%20LookAt/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='05%20LookAt/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='05%20LookAt/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

The problem with the "orbit" feature above is that you will soon lose sight of the model. This is because, although the camera _position_ is orbiting, the camera _orientation_ remains the same (so the camera will always be looking off in the same direction). To solve this problem, you should implement a `lookAt` function as described in the above slides and audio narration. This function should take a single parameter - a point in 3D space that the camera should "look at" (point towards).

You should call your new `lookAt` function each time your "orbit" code has changed the position of the camera. By passing in the centre of the scene as a parameter, the `lookAt` function will ensure that the camera is _always_ oriented towards the centre of the scene (so that the Cornell Box model will always remain visible). See the animation linked to above for a demonstration of the effect that you should be trying to achieve from the point of view of the camera (note: there is no audio narration for this particular animation !)

  


**Hints & Tips:**  
You might also like to include an event handler which allows the user to toggle (switch on and off) the above animation - sometimes they aren't going to want to be constantly orbiting !  


# 
### Task 6: Texture Mapping in 3D (optional)
 <a href='06%20Texture%20Mapping%20in%203D%20%28optional%29/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='06%20Texture%20Mapping%20in%203D%20%28optional%29/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a>

The aim of this optional task is to integrate the texture mapping function that you _might_ have implemented in a previous workbook into your 3D rasteriser. If you didn't implement the texture mapping function previously, or are behind with the workbook tasks, then you might like to skip this current task. Don't worry - if you are doing the coursework for this unit, you will have the opportunity to revisit texture mapping at a later date.

Take a look at the slides and audio narration above for more information on how OBJ files handle textures. Use this knowledge to extend your file parsing functions to read and then store the additional texture information. Note that the `ModelTriangle` class contains an array of `TexturePoints` to help you maintain the relationship between triangle vertices and texture point coordinates.

Rather than drawing the floor of the Cornell Box model as two plain green triangles, you should texture these triangles using our usual <a href="models/texture.ppm" target="_blank">texture image</a>. To help you in this task, we have provided an <a href="models/textured-cornell-box.mtl" target="_blank">updated OBJ materials file</a> and an <a href="models/textured-cornell-box.obj" target="_blank">updated OBJ geometry file</a> both of which contain additional texture information (namely: a new textured material called "Cobbles", texture mapping points for some of the vertices in the model and a reference to the texture image filename).

Try to get your render looking _similar_ to that shown in the diagram below. Don't spend too long trying to get the textured floor looking _exactly_ right (because you'll never actually manage to do it ;o) You might have an intuition that something isn't _quite_ right with this approach to texture mapping in 3D. Try not to worry too much about this for the moment - we will return to the topic of texture mapping in a later workbook (spoiler: the problem relates to depth and perspective !)  


![](06%20Texture%20Mapping%20in%203D%20%28optional%29/images/textured-floor.png)

# 
### Task 7: Homogeneous Coordinates (optional)
 <a href='07%20Homogeneous%20Coordinates%20%28optional%29/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='07%20Homogeneous%20Coordinates%20%28optional%29/slides/segment-2.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='07%20Homogeneous%20Coordinates%20%28optional%29/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='07%20Homogeneous%20Coordinates%20%28optional%29/audio/segment-2.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='07%20Homogeneous%20Coordinates%20%28optional%29/audio/segment-3.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='07%20Homogeneous%20Coordinates%20%28optional%29/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

This is an optional task that you may (or may not ?) wish to implement. It is useful to know about homogenous coordinates - so _do_ view the slides, audio narration and animation above. However, it is up to you whether you choose to implement these ideas in your rendering code. These features are not essential to complete later exercises and they offer only very marginal benefit for the kinds of rendering that we will be attempting in the remainder of this unit.

If you have the time (and the desire to do so) refactor your code to operate with homogenous coordinates. This will require you to replace `vec3` variables with `vec4` and `mat3` with `mat4`. You may also find that you get some interesting "bonus features" and "side effects" with features such as `lookAt` !  


# 
### End of workbook

Make sure you push all your work to your GitHub repository !<br>
Use of GitHub is a key development skill.<br>
It will allow us to keep track of your progress (and we will use it in the final marking process).