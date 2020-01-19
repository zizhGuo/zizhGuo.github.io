---
layout: post
title:  "Computer Graphics Labs (algorithms & rendering techniques)"
date:   2019-12-21 01:11:13
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---
### **Lab6: Textures**

#### **Description**
1. What is this lab about?

This lab is a demonstration of texture mapping. We also applied shading model on the basis of textures attached on the objects. The way to implement this is to replace the color of material by the texture color calculated by the OpenGL pipeline. 

There are multiple ways to parse the image passed into the pipeline as the texture. The easiest way is to implement a library called **SOIL** which provides many useful methods to help do it. One key process for apply texture on the object is to **manually reverse Y coordinates** since the OpenGL UV coordinates sets the origins at the lower left corner instead what it is usually presented by top left corner.

The process to parse and load image to shader is a bit complex, for it involves generate the texture in GLint, and active which texture unit for shader to choose, and sets the texture parameters. The order for doing the whole process is important, so it has to be careful.

2. Why do we do this?

Texture mapping is a method for defining high frequency detail, surface texture, or color information on a computer-generated graphic or 3D model.  - Wikipedia "Texture mapping"

[picture]

#### **Prerequisites**
1. Ubuntu machine.
2. GLFW ( http://www.glfw.org/) - a windowing/application framework used by the assignments.
3. GLEW ( http://glew.sourceforge.net/) - a support library that allows the use of all OpenGL. extensions supported by your graphics hardware (not needed for Apple platforms).

#### **Installation**
1. Download the codes.
2. Use terminal & "cd" to current directery.
3. Run "make" to link and compile
4. Run "./main"

#### **Functions**
Partial codes: ([see complete module in GitHub](https://github.com/zizhGuo/CSCI610-Foundations-of-Computer-Graphics/blob/master/shading/phong.vert))

Two Approaches to load the images into the shader using SOIL API.
```c++
///
// This function initializes all texture-related data structures for
// the program.  This is where texture buffers should be created, where
// texture images should be loaded, etc.
//
// You will need to write this function, and maintain all of the values
// needed to be sent to the various shaders.
///
void initTextures( void )
{   
    //
    // For "Quad", using SOIL_load_OGL_texture() to directly generate texutres.
    //
    GLuint tex_happy = SOIL_load_OGL_texture(
        "happy.png",
        SOIL_LOAD_AUTO,
        SOIL_CREATE_NEW_ID,
        SOIL_FLAG_MIPMAPS | SOIL_FLAG_INVERT_Y | // set mipmaps; set inversion on Y coordinates
            SOIL_FLAG_TEXTURE_REPEATS); // set wrapping mode to repeat

    glActiveTexture(GL_TEXTURE0);  // active the texutre unit 0
    glBindTexture(GL_TEXTURE_2D, tex_happy);  // bind the happy face texture to current unit

    //
    // For "Sphere" and "Cylinder", using more complicated way by loading the image, sets the data,
    // sets the parameters independently, and loading the data to texture.
    //
    int width, height;
    unsigned char *image;
    GLuint tex_jupiter, tex_wall, tex_disc;
    glGenTextures(1, &tex_jupiter); // generate texture for sphere
    glGenTextures(1, &tex_wall); // generate texture for cylinder side
    glGenTextures(1, &tex_disc); // generate texture for cylinder disc

    // "Sphere"
    glActiveTexture(GL_TEXTURE2); // activate texture unit 2
    glBindTexture(GL_TEXTURE_2D, tex_jupiter); // bind the jupiter texture to current unit
    image = SOIL_load_image("jupiter.jpg", &width, &height, 0, SOIL_LOAD_RGB ); // loading the image
    if( image == 0 ) {     
        printf( "SOIL loading error: '%s'\n", SOIL_last_result() ); 
    }
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT); // sets S-axis wrapping mode as repeat
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT); // sets T-axis wrapping mode as repeat
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_NEAREST); // sets filtering mode for magnification as nearest
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR); // sets filtering mode for minification as linear
    glTexParameteri(GL_TEXTURE_2D, GL_GENERATE_MIPMAP, GL_TRUE); // enable mipmap generation
    glTexImage2D( GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, image); // load the image to texture
    SOIL_free_image_data(image); // free space
```

### **Lab5: Shading**

#### **Description**
1. What is this lab about?

This lab is a demonstration of shading models (phong and flat) implemented on OpenGL using self-written shaders. The project uses two pair of shaders, for flat model, the shading color is calculated in vertex shader and passed to fragment shader for final assignment, whereas for phong model, the normals has to be processed by OpenGL pipeline, so the final colors are calculated in fragment shader.

2. Why do we do this?

Shading is one most important step for sythesize a image, for it demonstrate how objects are shaded by the light. The light for illumination is categorized into three types: ambient, diffuse and specular. Each type of light works as parameters to define the light impacted on the object. Of course for objects for rendering, they have their own material property to match light coeffcients, which are used for the model formula. Everything is done in the shader, so in program, we need to send all those parameters by uniform variables defined in shader.

Since the model uses normals for each traiangle and vertices's coordinates as well, the buffers stores both information as the objects are initially created. 

[picture]

#### **Prerequisites**
1. Ubuntu machine.
2. GLFW ( http://www.glfw.org/) - a windowing/application framework used by the assignments.
3. GLEW ( http://glew.sourceforge.net/) - a support library that allows the use of all OpenGL. extensions supported by your graphics hardware (not needed for Apple platforms).

#### **Installation**
1. Download the codes.
2. Use terminal & "cd" to current directery.
3. Run "make" to link and compile
4. Run "./main"

#### **Functions**
Partial codes: ([see complete module in GitHub](https://github.com/zizhGuo/CSCI610-Foundations-of-Computer-Graphics/blob/master/shading/phong.vert))
Since the synthex of GLSL is similar to C++, so I use c++ format to demonstrate.
```c++
void main()
{
    // Add all illumination and shading code you need here

    vec4 normals = normalize(N); 
    vec4 L = normalize(Lpos - vpos); // calculate light vector
    vec4 R = reflect(L, normals); // calculate the reflective vector
    vec4 V = normalize(vpos);

    fragmentColor = IA * KA * OA + IS * KD * OD * max(dot(L.xyz, normals.xyz), 0) + OS *IS * KS * pow(max(dot(R.xyz, V.xyz), 0), SE); // apply phong model equation to get final color

    // by default, just use 80% gray for everything
    //fragmentColor = vec4( 0.8, 0.8, 0.8, 1.0 );
}

```

### **Lab4: Tessellation**

#### **Description**
1. What is this lab about?

This lab is a demonstration how tessellation algo is performed.

2. Why do we do this?

Tessellation is used for dividing the polygons into suitable structures for rendering by creating more traiangles with program generated vertices. Familiarly, some basic primitive shapes utilize this algorithm, such as cone, cylinder, sphere and cube, etc.

[picture]

#### **Prerequisites**
1. Ubuntu machine.
2. GLFW ( http://www.glfw.org/) - a windowing/application framework used by the assignments.
3. GLEW ( http://glew.sourceforge.net/) - a support library that allows the use of all OpenGL. extensions supported by your graphics hardware (not needed for Apple platforms).

#### **Installation**
1. Download the codes.
2. Use terminal & "cd" to current directery.
3. Run "make" to link and compile
4. Run "./main"

#### **Functions**
Partial codes: ([see complete module in GitHub](https://github.com/zizhGuo/CSCI610-Foundations-of-Computer-Graphics/blob/master/tessellation/Shapes.cpp))
```c++
///
// makeCylinder - Create polygons for a cylinder with unit height, centered at
// the origin, with separate number of radial subdivisions and height 
// subdivisions.
//
// @param radius - Radius of the base of the cylinder
// @param discFactor - number of subdivisions on the radial base
// @param heightFactor - number of subdivisions along the height
//
// Can only use calls to C.addTriangle()
///
void makeCylinder( Canvas &C, float radius,
                   int discFactor, int heightFactor )
{
    if( discFactor < 3 )
        discFactor = 3;

    if( heightFactor < 1 )
        heightFactor = 1;

    // YOUR IMPLEMENTATION HERE
    // offset on given discFactor and stride for current location on the same tier
    float strideDisk = 0.0f; 
    float offsetA = 2.0f * 3.14159f / discFactor;

    // offset on given heightFactor and stride for current height
    float strideSide = 0.0f;
    float offsetB = 1.0f / heightFactor;


    for (int i {0}; i < discFactor; ++i)
    {
        //Tessellation on disk top
        Vertex v1 = makeDiskVertex(strideDisk, radius, 0.5f);
        Vertex v2 = makeDiskVertex(strideDisk + offsetA, radius, 0.5f);
        C.addTriangle(Vertex{0.0f, 0.0f, 0.5f, 1.0f},v1, v2);

        //Tessellation on disk bottom
        Vertex v3 = makeDiskVertex(strideDisk, radius, -0.5f);
        Vertex v4 = makeDiskVertex(strideDisk + offsetA, radius, -0.5f);
        C.addTriangle(Vertex{0.0f, 0.0f, -0.5f, 1.0f},v4, v3);
        strideDisk += offsetA;

        //Tessellation on Cylinder side
        for(int j {0}; j < heightFactor; ++j)
        {
            Vertex vv1 = makeRecVertex(0.0f, strideSide, v1, v2, v3, v4);
            Vertex vv2 = makeRecVertex(1.0f, strideSide + offsetB, v1, v2, v3, v4);
            Vertex vv3 = makeRecVertex(0.0f, strideSide + offsetB, v1, v2, v3, v4);
            Vertex vv4 = makeRecVertex(1.0f, strideSide, v1, v2, v3, v4);
            C.addTriangle(vv1, vv2, vv3);
            C.addTriangle(vv2, vv1, vv4);
            strideSide += offsetB;
        }
        strideSide = 0.0f;
    }
}
```


### **Lab3: Sutherland-Hodgman Polygon Clipper**

#### **Description**
1. What is this lab about?

This lab is a demonstration of Sutherland-Hodgman Polygon Clipper.

2. Why do we do this?

In pipeline, after do Model Transform, the objects should be transformed to viewport, so it is necessary to clip the objects. In 3D, it depends on which perspective you choose, either the frustum or orthographic perspective will apply this step in order to eliminate the parts that is no need to demonstrate.

[picture]

#### **Prerequisites**
1. Ubuntu machine.
2. GLFW ( http://www.glfw.org/) - a windowing/application framework used by the assignments.
3. GLEW ( http://glew.sourceforge.net/) - a support library that allows the use of all OpenGL. extensions supported by your graphics hardware (not needed for Apple platforms).

#### **Installation**
1. Download the codes.
2. Use terminal & "cd" to current directery.
3. Run "make" to link and compile
4. Run "./main"

#### **Functions**
Partial codes: ([see complete module in GitHub](https://github.com/zizhGuo/CSCI610-Foundations-of-Computer-Graphics/blob/master/clip/c%2B%2B/Clipper.cpp))





### **Lab2: Sutherland-Hodgman Polygon Clipper**

#### **Description**
1. What is this lab about?

This lab is a demonstration of Sutherland-Hodgman Polygon Clipper.

2. Why do we do this?

In pipeline, after do Model Transform, the objects should be transformed to viewport, so it is necessary to clip the objects. In 3D, it depends on which perspective you choose, either the frustum or orthographic perspective will apply this step in order to eliminate the parts that is no need to demonstrate.

[picture]

#### **Prerequisites**
1. Ubuntu machine.
2. GLFW ( http://www.glfw.org/) - a windowing/application framework used by the assignments.
3. GLEW ( http://glew.sourceforge.net/) - a support library that allows the use of all OpenGL. extensions supported by your graphics hardware (not needed for Apple platforms).

#### **Installation**
1. Download the codes.
2. Use terminal & "cd" to current directery.
3. Run "make" to link and compile
4. Run "./main"

#### **Functions**
Partial codes: ([see complete module in GitHub](https://github.com/zizhGuo/CSCI610-Foundations-of-Computer-Graphics/blob/master/clip/c%2B%2B/Clipper.cpp))

```c++
///
// clipPolygon
//
// Clip the polygon with vertex count in and vertices inV against the
// rectangular clipping region specified by lower-left corner ll and
// upper-right corner ur. The resulting vertices are placed in outV.
//
// The routine should return the with the vertex count of polygon
// resulting from the clipping.
//
// @param in    the number of vertices in the polygon to be clipped
// @param inV   the incoming vertex list
// @param outV  the outgoing vertex list
// @param ll    the lower-left corner of the clipping rectangle
// @param ur    the upper-right corner of the clipping rectangle
//
// @return number of vertices in the polygon resulting after clipping
///
int clipPolygon( int in, const Vertex inV[], Vertex outV[],
         Vertex ll, Vertex ur ) </code>
</pre> </summary>
    <pre>
<code>
int clipPolygon( int in, const Vertex inV[], Vertex outV[],
         Vertex ll, Vertex ur )
{
    // YOUR CODE GOES HERE
    list<Vertex> out1 {list<Vertex>()};
    list<Vertex> out2 {list<Vertex>()};
    list<Vertex> out3 {list<Vertex>()};
    list<Vertex> out4 {list<Vertex>()};
    int out1_length {0};
    int out2_length {0};
    int out3_length {0};
    int out4_length {0};        
    Vertex lr {ur.x, ll.y};
    Vertex ul {ll.x, ur.y};

    // clipping with lower boundary
    SHPC(inV, out1, in, out1_length, ll, lr);
    Vertex out1_arr[out1_length];
    int count{0};
    for (auto i {out1.begin()}; i != out1.end(); ++i)
    {
        out1_arr[count] = *i;
        ++count;
    }

    // clipping with right boundary
    SHPC(out1_arr, out2, out1_length, out2_length, lr, ur);
    Vertex out2_arr[out2_length];
    count = 0;;
    for (auto i {out2.begin()}; i != out2.end(); ++i)
    {
        out2_arr[count] = *i;
        ++count;
    }

    // clipping with upper boundary
    SHPC(out2_arr, out3, out2_length, out3_length, ur, ul);
    Vertex out3_arr[out3_length];
    count = 0;;
    for (auto i {out3.begin()}; i != out3.end(); ++i)
    {
        out3_arr[count] = *i;
        ++count;
    }

    /// clipping with left boundary
    SHPC(out3_arr, out4, out3_length, out4_length, ul, ll);
    Vertex out4_arr[out4_length];
    count = 0;;
    for (auto i {out4.begin()}; i != out4.end(); ++i)
    {
        out4_arr[count] = *i;
        ++count;
    }

    // deep copy the vertices list to outV array
    for (int i{0}; i < out4_length; ++i)
    {
        outV[i] = out4_arr[i];
    }

    return out4_length; // return the number of vertices from clipped polygon
}
```
    



### **Lab1: Polygon Filling Algorithm**

#### **Description**
1. What is this lab about?

This lab is a demonstration of polygon filling algorithm. The frame buffers stores the color of each pixel. In order to physically make it see-able, the program will use the polygon filling algorithm to fill the pixels that inside of the polygon with the pixel color in buffers. This lab is a simulation to implement this process.

[picture]

#### **Prerequisites**
1. Ubuntu machine.
2. GLFW ( http://www.glfw.org/) - a windowing/application framework used by the assignments.
3. GLEW ( http://glew.sourceforge.net/) - a support library that allows the use of all OpenGL. extensions supported by your graphics hardware (not needed for Apple platforms).

#### **Installation**
1. Download the codes.
2. Use terminal & "cd" to current directery.
3. Run "make" to link and compile
4. Run "./main"

#### **Functions**

```c++
///
// Draw a filled polygon.
//
// Implementation should use the scan-line polygon fill algorithm
// discussed in class.
//
// The polygon has n distinct vertices.  The coordinates of the vertices
// making up the polygon are supplied in the 'v' array parameter, such
// that the ith vertex is in v[i].
//
// You are to add the implementation here using only calls to the
// setPixel() function.
//
// @param n - number of vertices
// @param v - array of vertices
///
void Rasterizer::drawPolygon( int n, const Vertex v[] )
{
    // YOUR IMPLEMENTATION HERE
    int WINDOWS_HEIGHT {600};

    size_t maxY = 0;

  // Simple optimization: Get the highest y coordinate among all edges
    for (size_t i{0}; i < n; ++i)
    {
    if (v[i].y > maxY)
    {
      maxY = v[i].y;
    }
    }

  // Initialize the ET and AT
    list<Edge> ET[WINDOWS_HEIGHT];
    for (size_t i{0}; i < maxY; ++i)
    {
    ET[i] = list<Edge>() ;
    } 
  list <Edge> AT {list<Edge>()};

    
  // Add edges to ET based on ymin
    for (size_t i {0}; i != n; ++i)
    {
        int x1 = v[i].x;
        int x2 = v[(i + 1) % n].x;
        int y1 = v[i].y;
        int y2 = v[(i + 1) % n].y;

      // Igonore horizontal line
        if (y1 == y2) continue;

      // Info of current edge <(x1, y1), (x2, y2)>
        int ymin = y1 > y2 ? y2 : y1;
        int ymax = y1 > y2 ? y1 : y2;
        float x = y1 > y2 ? x2 : x1;
        float dx = (x1 - x2) * 1.0f / (y1 - y2);

      // Initialize current edge and ET
        Edge edge = Edge(ymax, x, dx);
        ET[ymin].push_back(edge);

    }

    // Scan-line drawing from y = 0 to y = 600
    for (size_t i {0}; i != WINDOWS_HEIGHT; ++i)
    {
        //drop tables
        for (auto j {AT.begin()}; j != AT.end();)
        {
            if (j -> ymax == i)
            {
                AT.erase(j++);
            }
            else
            {
                ++j;
            }
        }

    // Update AT by copying edges from ET
        for (auto j {ET[i].begin()}; j != ET[i].end(); ++j)
        {
            AT.push_back(*j);
        }

    //Sort edges in AT

      // Sort based on x
        AT.sort([](const Edge& a, const Edge& b){return a.x < b.x;});

      // For each pair, if x equals, sorting based on dx
      for (auto j {AT.begin()}; j != AT.end(); advance(j, 2))
      {
        auto nxt = next(j, 1);
        if (j -> x == nxt -> x)
        {
          if (j -> dx > nxt -> dx)
          {
            iter_swap(j, nxt);
          }          
        }
      }

    // Fill polygon by pairs in each scan-line
      for (auto j {AT.begin()}; j != AT.end(); advance(j, 2))
      {
        auto nxt = next(j, 1);
        float x_left = j -> x;
        int x1 {0};

        // left intersected point, if x is on the edge, draw it. Or round it up.
        if (x_left - (int)x_left > 0) {
          x1 = (int)x_left + 1;
        }
        else
        {
          x1 = (int)x_left;
        }

        // right intersected point, round it down.
        int x2 = (int)(nxt -> x);

        // For each line segment by paired intersected points, set pixels.
        for (int k {x1}; k != x2; ++k)
        {
          Vertex v {k, i};
          C.setPixel(v);
        }
      }

    // Update x in AT by adding dx.
      for (auto j {AT.begin()}; j != AT.end(); advance(j, 1))
      {
        j -> x += j -> dx;
      }
    }
```












#### **Contribution**

#### **License**

#### **Citation**
https://www.jianshu.com/p/d9be99077c2b
#### **Contact**
