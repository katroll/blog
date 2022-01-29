---
title: "Three.js From A Beginner"
date: 2022-01-28T17:57:25-05:00
draft: false
---

I did not expect to spend my day today the way that I did. 

It started this morning when I began to think about what I want my portfolio web page to look like. I googled something along the lines of “software developer portfolio sites” and a few links came up with titles like “Best 20 Software Portfolio Examples”. I clicked on one and started scrolling,  saw a site that looked cool, so I followed the link. 

The link took me to this really cool site that you can find [here](https://bruno-simon.com/), built by Bruno Simon. My first thought was that I wanted to be able to build a site like this. On driving the truck around on the site a bit more (if you follow the link you will understand), I passed the mention of a course that the creator of the site teaches, called Three.js Journey. After a bit more reading, I decided that I was going to enroll in this course on the side to the Flatiron School to just keep learning new and cool things, and hopefully make a cool site using Three.js in the future.

Five hours later I am about a third of the way through the first chunk of the basics. So far I have learned about creating scenes, meshes, cameras, transforming objects, animating objects, and more that I’m likely forgetting because it is so much to take in. 

***

**WHAT I HAVE DONE SO FAR**

The main take aways from the many hours of videos I watched about creating a scene is this:

1. Before anything, you want to make sure you are **importing three.js** to your js file with with:

        import * as THREE from 'three'


2.	**Create the scene:**

    A scene requires specific pieces to function: a scene, an object, a camera, and a renderer.

- **A scene**

    A scene is a three.js obect that can be created with just one line of code:

        const scene = new THREE.Scene()     

- **An Object**

    A mesh is a three.js 3D object that requires two parameters -- the type of object(geometry), and the material of the object. There are a number of existing geometries already in the three.js framework such as cubes, speres, cones, and many more complex 3D shapes. 

    There won't be anything to see in your scene unless you create it, so here is a simple cube created:

        const geometry = new THREE.BoxGeometry(1, 1, 1)
        const material = new THREE.MeshBasicMaterial({ color: 0xff0000 })
        const mesh = new THREE.Mesh(geometry, material)
        scene.add(mesh)

    
    The geometry object is created using the first line of the above code with the material object created on the following line. The material in this example only has a color passed to it. 

    The mesh itself is created on the third line and then added into the scene object on the forth line.

    Now that we have a scene with a mesh placed in the scene, we need a way and a perspective to look at it. This is where the camera comes in.

- **A Camera**

        const sizes = {
            width: 800,
            height: 600
        }

        // Camera
        const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height)
        camera.position.z = 3
        scene.add(camera)

    The code above first sets a size variable that is used in multiple places. It is used to set the size of the renderer that is used to render the scene. We also need to use it to set the aspect ratio of the camera to make sure things are displayed correctly. 

    On the first line where we create a new camera, we pass it two variables:

    1. A field of view. The field of view is a number that specifies the vertical degrees that the camera can view.

    2. The aspect ratio. this has a default value of 1, which would be a square view. If viewing in a none square canvas, it would be canvas width / canvas height.

    The line following creating the camera is positioning the camera. Everything is created by default at the center of a 3 dimensional graph with x, y, and z axis. So right now the camera and cube are right on top of each other. We want to move the camera back away from the cube we created so we can view it.

    Moving the camera along the positive z-axis is backing the camera up away from the cube while still pointing at it. moving it along the positive y-axis will raise the camera's "height" and the x-axis will move it left and right. 

    The last step is to add the camera to the scene with `scene.add(camera)`.

    - **A Renderer**

    The final step is the render the scene to the the html file that is loaded to the page.

        const renderer = new THREE.WebGLRenderer({
            canvas: document.querySelector('canvas.webgl')
        })


    First, we create a new three.js renderer object. It needs a `<canvas>` html tag to render within, so we use `document.querySelector('canvas.webgl')` to grab the `<canvas>` with a class of "webgl" and pass that into the constructor.

        renderer.setSize(sizes.width, sizes.height)
        renderer.render(scene, camera)

    As mentioned before, the sizes `const` was also created to set the size of the renderer, which is done on the first line of code above.

    Then, finally, the scene and camera are rendered to the page with `renderer.render(scene, camera)`.

    ***

    With that not so many lines of code, you will have a 3D cube on the screen! I'm having alot of fun learning how to build things and move them around. The next topic of  animations was more interesting and more fun, and I will add that into my next post! 





