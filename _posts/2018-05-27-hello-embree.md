---
title:  "Hello Embree"
date:   2018-05-27 10:30
categories: objects
---

I've been slowly developing a raytracer which has been a lot of fun and a great learning experience both from a technical standpoint and the theoretical side of physically-based rendering.

I'm at a point now where I am importing meshes of a larger nature, and I was not content with my BVH implementation. As much as I would like to read the state of the art papers on BVH construction and traversal, I've devided to opt to use embree instead. I would rather focus on the light transport part and focus on getting prettier images and getting them to converge quicker from an algorithmic standpoint. As a friend of mine said, I have to pick my battles, and embree is going to do a lot of heavy lifting for me so that I can focus more on what I'd like to write.

Setting up embree with CMake is quite easy (I've got the full source code of this post available here), and I won't go into details into how to set it up - the CMakeLists.txt is quite self explanatory and I've added as much comments as I can.

As great and as fleshed out the samples are, there is a lot of boilerplate code, written in different header files. This put me off initially but with a bit of digging, and building the samples with a powerful IDE went long way in help way in helping me zip around the API and the headers. 

I thought it would be interesting to write the simplest thing I can think of with embree, A hello world ray tracing example. I'm a big fan of Dr. Peter Shirley's Ray Tracing in One Weekend Book series, so I thought I'd redo the first 2 chapters (with some changes) using embree. Let's get started.


### What is embree?

Embree is a high-performance ray tracing kernel library written by Intel. They implement state-of-the-art implementations of accelleration structure and intersection methods, and provide a plethora of features to write a performant ray tracer. More details can be found here, and in their white paper.


### Hello Embree!

My toy example shoots a ray per pixel and returns white if nothing is hit and black if the triangle places in the centre of the image is hit. This is the main function: 

{% highlight c %}
int main()
{
    // Inititiate device and scene
    // Add geometry to scene
    // Initiate image parameters, image and camera
    // Render image
    // Output image
}
{% endhighlight %}

We'll work through each of these with a short explanation for everything. The full source code listing can be found here. It is self contained, with embree being part of the sources. If you have any issues with compilation, feel free to contact me.

If you want to skip ahead:
1. [Device initiation](#device-initiation)
2. [Adding geometry to the scene](#adding-geometry-to-the-scene)
3. [Creating the triangle geometry and defining the vertices](#creating-the-triangle-geometry-and-defining-the-vertices)
4. [Render image](#render-image)
5. [Output image](#output-image)


### Device initiation 

{% highlight c %}
// Inititiate device and scene
RTCDevice device = rtcNewDevice("");
RTCScene scene = rtcNewScene(device);
{% endhighlight %}

The device object is defined to be a class factory for all the other objects that we'll be creating such as the scene and the geometry, using the [rtcNewDevice](https://embree.github.io/api.html#rtcnewdevice){:target="_blank"} call. The device handle is not destroyed until all objects bound with the device are released. Embree uses reference counting to keep track of the lifetime of all the objects you create - functions with the word release decrease the reference counter.

The new scene is created and bound to the device previously created.


### Adding geometry to the scene

We now need to define and add the geometry to our scene. I'm just going to create a triangle in the middle of the screen.
{% highlight c %}
// Add geometry to scene
    // Create a new geometry for the triangle
    // Define the vertices
    // Assign the vertices to the geometry buffer
    // Commit geometry to the scene
{% endhighlight %}


#### Creating the triangle geometry and defining the vertices

We'll need to allocate memory for our geometry and this some special functions.
{% highlight c %}
// Create a new geometry for the triangle
RTCGeometry mesh = rtcNewGeometry(device, RTC_GEOMETRY_TYPE_TRIANGLE);
Vertex* vertices = (Vertex*) rtcSetNewGeometryBuffer(mesh, RTC_BUFFER_TYPE_VERTEX, 0, RTC_FORMAT_FLOAT3, sizeof(Vertex), 3);
{% endhighlight %}

We create a geometry object using [rtcNewGeometry](https://embree.github.io/api.html#rtcnewgeometry), which is attached to our device and we've picked a triangle to be our geometry of choice. Embree has a selection of different geometry types available. Once we've created our geometry object, we create a buffer for the vertices, using [rtcSetNewGeometryBuffer](https://embree.github.io/api.html#rtcsetnewgeometrybuffer). We'll use this function to set the indices later on. The key here is the second parameter, RTC_BUFFER_TYPE_VERTEX. 

Next is the vertices:

{% highlight c %}
// Define the vertices
vertices[0].x = -1; vertices[0].y =  0; vertices[0].z = -3;
vertices[1].x =  0; vertices[1].y = -1; vertices[1].z = -3;
vertices[2].x =  1; vertices[2].y =  0; vertices[2].z = -3;
{% endhighlight %}

Nothing extraordinary here - just defining the positions of the vertices. We'll now assign these indices to our buffer:

{% highlight c %}
// Assign the vertices to the geometry buffer
Triangle* triangles = (Triangle*) rtcSetNewGeometryBuffer(mesh, RTC_BUFFER_TYPE_INDEX, 0, RTC_FORMAT_UINT3, sizeof(Triangle), 1);
triangles[0].v0 = 0; triangles[0].v1 = 1; triangles[0].v2 = 2;
{% endhighlight %}

We're now setting up an index buffer for the vertices. Looking at this from a birds eye view, it's quite similar to how things are done in OpenGL. We're not done yet though. We need to commit the geometry and the scene.

{% highlight c %}
// Commit geometry to the scene
rtcCommitGeometry(mesh);
unsigned int geomID = rtcAttachGeometry(scene,mesh);
rtcReleaseGeometry(mesh);
rtcCommitScene(scene);
{% endhighlight %}

So what does this all mean? The [rtcCommitGeometry](https://embree.github.io/api.html#rtccommitgeometry){:target="_blank"} call commits all the modifications to the geometry and has to be called each time we modify the geometry.

We then attach the geometry to the scene using [rtcAttachGeometry](https://embree.github.io/api.html#rtcattachgeometry){:target="_blank"}. The releaseGeometry[rtcReleaseGeometry](https://embree.github.io/api.html#rtcreleasegeometry){:target="_blank"} call decreases the reference count of each pice of geometry. Curious because why are we releasing just when we committed. I saw it in the examples and something I'm still looking into. To commit our scene we'll need to call rtcCommitScene which builds our spatial data structure. Everytime we modify the geometry, we need to go through this step.

We'll initiate the image and the camera parameters.
{% highlight c %}
// Initiate image parameters, image and camera
const int width = 600;
const int height = 300;
unsigned char *image = new unsigned char[width * height * 4];

embree::Vec3fa lower_left_corner(-2.0f, -1.0f, -1.0f);
embree::Vec3fa horizontal(4.0f, 0.0f, 0.0f);
embree::Vec3fa vertical(0.0f, 2.0f, 0.0f);
embree::Vec3fa origin(0.0f, 0.0f, 0.0f);
{% endhighlight %}

If you're not familiar with regards to the parameters of the camera, I'd suggest reading up on primary ray generation and the camera model. I might do a post about this later as I used to struggle with this a while back. Now that I've said this in public I'll be forced to do it :)

Nothing too fancy here, just define the image parameters and the camera parameters for the primary ray generation.

The next part is where we get to test the ray intersection!


### Render image 

{% highlight c %}
// Render image
for(int y = height-1; y >= 0; --y)
{
    for(int x = 0; x < width; ++x)
    {
        // Initiate pixel colour
        embree::Vec3fa color = embree::Vec3fa(0.0f);
        // Create ray intersection context
        // Generate ray
        // Perform ray intersection
        // Check if ray intersected any geometry
        // Set pixel colour
    }
}
{% endhighlight %}

{% highlight c %}
// Create ray intersection context
RTCIntersectContext context;
rtcInitIntersectContext(&context);
{% endhighlight %}

This is the primary ray generation stolen from Dr. Shirley's book.
{% highlight c %}
// Generate ray
float u = float (x) / float (width);
float v = float (y) / float (height);

embree::Vec3fa direction(lower_left_corner + horizontal*u + vertical*v);
Ray r(origin, normalize(direction), 0.0f, embree::inf);
{% endhighlight %}

Once we have the ray set up, all we need to do is perform a ray intersection test using an embree function suited for [single ray intersection tests](https://embree.github.io/api.html#rtcintersect1){:target="_blank"}.

{% highlight c %}
// Perform ray intersection
rtcIntersect1(scene, &context, RTCRayHit_(r));
{% endhighlight %}

We need to pass in the scene and the context. The RTCRayHit_ function constructs the embree ray-type from the one that type that we defined ourselves. In reality our ray is very similar to what's the defined in embree. The intersection information is stored in the ray, and checked:

{% highlight c %}
// Check if ray intersected any geometry
if(r.geomID != RTC_INVALID_GEOMETRY_ID)
{
    color = embree::Vec3fa(1.0f, 1.0f, 1.0f);
}
{% endhighlight %}

and that's it! The embree specific stuff ends here. I haven't included the ray definition and the auxiliary functions to convert because they're one liners and you can have a poke around in the repo. We then set the colour of the pixel:

{% highlight c %}
// Set pixel colour
image[(x + width * y)*4 + 0] = (unsigned char)(color[0] * 255.0);
image[(x + width * y)*4 + 1] = (unsigned char)(color[1] * 255.0);
image[(x + width * y)*4 + 2] = (unsigned char)(color[2] * 255.0);
image[(x + width * y)*4 + 3] = (unsigned char)(255);
{% endhighlight %}

and repeat the whole process for each pixel. When that's done, we spit out our png:


### Output image
{% highlight c %}
// Output image
unsigned error = lodepng::encode("hello.png", image, width, height);
if (error) std::cout << "encoder error " << error << ": " << lodepng_error_text(error) << std::endl;
{% endhighlight %}

I'm using lodepng to write the image to file, nothing too complex going on here.

I hope this wasn't too verbose. Embree's extensive features means that it's now significantly easier to write a high-performant ray tracer. I'm very excited in doing more fleshed out examples and melding it with my own ray tracer. I will have to probably change a lot from an architecture standpoint but the performance gains warrant it.
