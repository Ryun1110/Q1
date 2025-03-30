# Q1

# Ray Tracer (Computer Graphics Assignment)

This project is a simple ray tracer that renders a scene containing a plane and three spheres using the Phong illumination model. The code also includes functionality for computing shadows and supports basic perspective projection from a user-defined camera.

---

## Table of Contents
1. [Overview](#overview)  
2. [Project Structure](#project-structure)  
3. [Dependencies](#dependencies)  
4. [Building and Running](#building-and-running)  
5. [Implementation Details](#implementation-details)  
6. [Usage Notes](#usage-notes) 

---

## Overview

**Key Features:**
- Perspective camera setup using eye, look-at, and up vectors.
- Support for a plane (`y = -2`) and three spheres with different radii and centers:
  - Sphere S1 at `(-4, 0, -7)` with radius `1`.
  - Sphere S2 at `(0, 0, -7)` with radius `2`.
  - Sphere S3 at `(4, 0, -7)` with radius `1`.
- Phong shading model:
  - Ambient, diffuse, and specular terms.
  - Shadows calculated via shadow rays.
- Configurable image plane bounds (`l, r, b, t, d`).
- Image resolution defaults to `512 x 512`.
- Renders the final image to an OpenGL window.

---

## Project Structure

Below is a brief description of each file:

- **Main_Q1.cpp**  
  Contains the `main()` function. Creates a window using GLFW, sets up an OpenGL context, and calls the ray-tracing routine to fill a pixel buffer, which is then drawn to the screen.

- **Camera.h**  
  Defines a `Camera` class that generates rays for each pixel. It takes the standard parameters: eye position, look-at, up vector, and viewport parameters (`l, r, b, t, d`).

- **Ray.h**  
  Defines a simple `Ray` class with origin and direction.

- **Plane.h**  
  Represents the plane in the scene, here defined by the equation `y = -2`. Implements an `intersect` method to compute ray-plane intersection.

- **Sphere.h**  
  Represents a sphere in the scene, defined by its center and radius. Implements an `intersect` method (solves the quadratic equation) and a `getNormal` method for normals at hit points.

- **Material.h**  
  Stores material properties (`ka`, `kd`, `ks`, `shininess`) for use in the Phong illumination model.

- **Scene.h**  
  Holds all scene objects (the plane and three spheres, plus their materials). Implements:
  - **findClosestIntersection**: Loops through all objects to find the nearest intersection.
  - **inShadow**: Checks whether a hit point is in shadow relative to the light source.
  - **computePhong**: Evaluates the Phong lighting equation (ambient + diffuse + specular).
  - **intersect**: High-level method that returns whether any object was hit and also provides the resulting color.

---

## Dependencies

1. **OpenGL**  
   The project uses OpenGL to render the pixel buffer to the screen.  
2. **GLFW**  
   Handles window creation and input events on Windows (or other OS).  
3. **GLEW**  
   Manages OpenGL extensions.  
4. **GLM**  
   A C++ math library specifically designed for graphics software (vector and matrix operations).  
5. **Visual Studio 2022 (Windows 11)**  
   Recommended environment as described in the assignment instructions.  

---

## Building and Running

### Windows (Visual Studio 2022)

1. **Clone or Download** this repository (or extract the ZIP).
2. **Open the Project**  
   - Open the `.sln` (solution) file in Visual Studio **or** create a new Visual Studio project and add the `.cpp`/`.h` files.
3. **Configure Dependencies**  
   - Ensure that GLFW, GLEW, and GLM are installed or included.  
   - In Visual Studio, go to **Project Properties** → **VC++ Directories** → **Include Directories** and **Library Directories** to point to your local copies of GLFW, GLEW, and GLM.
4. **Build**  
   - Select **Build → Build Solution** (F7).
5. **Run**  
   - Press **Local Windows Debugger** (F5).  
   - A window should appear with the rendered image.

### Other Platforms

- The code can be adapted to other operating systems or build systems (e.g., CMake). In that case:
  - Make sure you have the OpenGL, GLFW (or another windowing library), and GLM development libraries installed.
  - Adjust your build scripts accordingly (e.g., `CMakeLists.txt`).

---

## Implementation Details

- **Ray Generation**  
  A primary (eye) ray is generated for each pixel, passing through the center of that pixel in camera space.
- **Intersection**  
  - **Plane** uses the analytical solution for a ray-plane intersection (solving for `t` in `y = -2`).  
  - **Sphere** uses the quadratic equation to find intersections.
- **Shading (Phong)**  
  - Ambient: `ka * lightColor`  
  - Diffuse: `kd * max(dot(N, L), 0)`  
  - Specular: `ks * max(dot(R, V), 0)^shininess`  
  - Shadows: If a point is in shadow, we only apply ambient.
- **Light Source**  
  A point light at `(-4, 4, -3)` emitting white light. No falloff is applied; it uses constant intensity in the provided code.
- **Output**  
  Renders to a `std::vector<float>` (`OutputImage`), which is then drawn via `glDrawPixels`.

---

## Usage Notes

- **Camera and Scene Configuration**  
  - By default, the camera is at `(0, 0, 0)`, looking in the negative `z` direction.  
  - The default resolution is `512 x 512`.  
  - The field of view and near plane dimensions are set by `[l, r, b, t, d] = [-0.1, 0.1, -0.1, 0.1, 0.1]`.

- **Modifications**  
  - You can freely edit camera parameters, field of view, or resolution.  
  - If you use alternative libraries for the windowing system, ensure to modify the main loop and image-display routine accordingly.

---

