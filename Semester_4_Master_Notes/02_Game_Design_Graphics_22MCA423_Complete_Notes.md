# Master Study Notes: Fundamentals of Game Design & Graphics
## Course Code: 22MCA423 / Professional Elective | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: Game Design Foundations & MDA Framework (Game Architecture, Mechanics, Dynamics, Aesthetics - MDA Framework, Core Game Loops, Bartle's Player Taxonomy, Game Design Document - GDD).
* **Module 2**: 2D & 3D Mathematical Foundations for Graphics (Vector Math, Dot & Cross Products, Homogeneous Coordinates, 2D & 3D Affine Transformations: Translation, Scaling, Rotation Matrices, View Frustum Projection).
* **Module 3**: The Graphics Rendering Pipeline & Shading (Programmable Graphics Pipeline: Vertex Processing, Primitive Assembly, Rasterization, Fragment Shader, Depth Testing, Phong Illumination Model: Ambient, Diffuse, Specular).
* **Module 4**: Game Engine Architecture & Physics Simulation (Entity Component System - ECS vs Object-Oriented Component Architecture, Game Loops & Delta Time, Rigid Body Dynamics, Collision Detection - AABB, Sphere, OBB, Raycasting).
* **Module 5**: Game AI, Audio & Production Lifecycle (AI Steering Behaviors, Finite State Machines - FSM, Behavior Trees, A* Pathfinding Algorithm with Manhattan/Euclidean Heuristics, Spatial 3D Audio, Production Stages).

---

# MODULE 1: GAME DESIGN & THE MDA FRAMEWORK

## 1.1 The MDA Framework (Hunicke, LeBlanc, Zubek)
```
  [ DESIGNER ]                                                 [ PLAYER ]
       |                                                           |
  [ MECHANICS ]  -------->  [ DYNAMICS ]  -------->  [ AESTHETICS ]
 (Rules, Data structures)  (Run-time behavior)      (Emotional response)
```
* **Mechanics**: The fundamental rules, algorithms, data structures, and actions provided to the player (e.g., jump height, inventory capacity, shooting cooldown).
* **Dynamics**: The run-time behavior that emerges when the player interacts with the mechanics (e.g., emergent economy, camping strategies, aggressive vs defensive play).
* **Aesthetics**: The desirable emotional responses evoked in the player during gameplay (e.g., Sensation, Fantasy, Narrative, Challenge, Fellowship, Discovery, Expression, Submission).

## 1.2 The Standard Game Loop
```python
while is_game_running:
    delta_time = clock.get_elapsed_time()
    process_player_inputs()        # Keyboard, mouse, gamepad polling
    update_game_physics(delta_time)# Kinematics, collisions, AI state
    render_graphics_frame()        # Draw sprites/polygons to back-buffer
    display.flip_buffers()         # Double-buffering V-Sync swap
```
* **Delta Time ($\Delta t$)**: Elapsed time between consecutive frames. Multiplying movement vectors by $\Delta t$ ensures frame-rate independence (game runs at the same physical speed whether at 30 FPS or 144 FPS):
  $$\text{position} := \text{position} + \text{velocity} \times \Delta t$$

---

# MODULE 2: MATHEMATICAL FOUNDATIONS FOR GRAPHICS

## 2.1 2D Affine Transformation Matrices (Homogeneous Coordinates)
Points are represented as column vectors $\begin{bmatrix} x & y & 1 \end{bmatrix}^T$.

1. **Translation Matrix**:
   $$T(t_x, t_y) = \begin{bmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{bmatrix}$$
2. **Scaling Matrix**:
   $$S(s_x, s_y) = \begin{bmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & 1 \end{bmatrix}$$
3. **Counter-Clockwise Rotation Matrix (by angle $\theta$)**:
   $$R(\theta) = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

---

# MODULE 3: THE GRAPHICS PIPELINE & PHONG LIGHTING

## 3.1 Programmable Rendering Pipeline
```
[ 3D Model Vertices ]
         |
[ Vertex Shader ]       ---> Transforms local 3D coordinates into Clip Space (MVP Matrix)
         |
[ Primitive Assembly ]  ---> Connects vertices into primitives (Triangles, Lines)
         |
[ Rasterization ]       ---> Interpolates triangles into 2D screen Fragments (Pixels)
         |
[ Fragment Shader ]     ---> Computes pixel color, textures, normal mapping, and lighting
         |
[ Tests & Blending ]    ---> Depth (Z-buffer) test, Stencil test, Alpha Blending
         |
[ Framebuffer Screen ]
```

## 3.2 Phong Reflection Model
The total illumination $I$ at a surface point is the superposition of three distinct components:
$$I = I_{\text{ambient}} + I_{\text{diffuse}} + I_{\text{specular}}$$
$$I = k_a I_a + k_d I_l (\vec{N} \cdot \vec{L}) + k_s I_l (\vec{R} \cdot \vec{V})^\alpha$$
* $k_a, k_d, k_s$: Ambient, diffuse, and specular reflection coefficients.
* $\vec{N}$: Surface normal unit vector.
* $\vec{L}$: Unit vector pointing toward the light source.
* $\vec{R}$: Reflected light direction vector ($2(\vec{N} \cdot \vec{L})\vec{N} - \vec{L}$).
* $\vec{V}$: Unit vector pointing toward the camera/viewer.
* $\alpha$: Shininess exponent (higher $\alpha$ yields a sharper, tighter highlight).

---

# MODULE 4: GAME ENGINES & COLLISION DETECTION

## 4.1 Collision Detection: Axis-Aligned Bounding Box (AABB)
Two boxes $A$ and $B$ collide if and only if they overlap along **every axis simultaneously**:
```cpp
bool CheckAABBCollision(const Box& A, const Box& B) {
    return (A.minX <= B.maxX && A.maxX >= B.minX) &&
           (A.minY <= B.maxY && A.maxY >= B.minY) &&
           (A.minZ <= B.maxZ && A.maxZ >= B.minZ);
}
```
* **Complexity**: $O(1)$ scalar checks; used for coarse broad-phase collision filtering before detailed polygon-level narrow-phase checks.

---

# MODULE 5: GAME AI & PATHFINDING

## 5.1 The A* Search Algorithm
A* finds the shortest path through a navigation mesh or grid graph by scoring candidate nodes using evaluation function:
$$f(n) = g(n) + h(n)$$
* $g(n)$: Exact path cost accumulated from start node to current node $n$.
* $h(n)$: Admissible heuristic estimating cost from node $n$ to the goal.
  * **Manhattan Distance** (Grid 4-directional): $h(n) = |x_n - x_{\text{goal}}| + |y_n - y_{\text{goal}}|$
  * **Euclidean Distance** (Continuous 8-directional): $h(n) = \sqrt{(x_n - x_{\text{goal}})^2 + (y_n - y_{\text{goal}})^2}$

---

# 🎯 High-Yield VTU Exam Solved Questions (10-Mark Model Answers)

### Question 1: 2D Composite Transformation Numerical (VTU Dec 2023 - 10 Marks)
**A triangle with vertices $A(0, 0)$, $B(2, 0)$, and $C(1, 2)$ is rotated by $90^\circ$ counter-clockwise about the point $P(1, 1)$. Determine the coordinates of the transformed triangle.**

**Solution:**
1. **Strategy (Composite Transformation)**:
   * Step 1: Translate point $P(1, 1)$ to the origin ($T_{-1, -1}$).
   * Step 2: Rotate by $90^\circ$ about origin ($R_{90^\circ}$).
   * Step 3: Translate back from origin to $P(1, 1)$ ($T_{1, 1}$).
2. **Formulate Matrices**:
   $$T(-1, -1) = \begin{bmatrix} 1 & 0 & -1 \\ 0 & 1 & -1 \\ 0 & 0 & 1 \end{bmatrix}, \quad R(90^\circ) = \begin{bmatrix} \cos 90^\circ & -\sin 90^\circ & 0 \\ \sin 90^\circ & \cos 90^\circ & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$
   $$T(1, 1) = \begin{bmatrix} 1 & 0 & 1 \\ 0 & 1 & 1 \\ 0 & 0 & 1 \end{bmatrix}$$
3. **Composite Matrix $M = T(1, 1) \cdot R(90^\circ) \cdot T(-1, -1)$**:
   $$R \cdot T(-1, -1) = \begin{bmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & -1 \\ 0 & 1 & -1 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 0 & -1 & 1 \\ 1 & 0 & -1 \\ 0 & 0 & 1 \end{bmatrix}$$
   $$M = \begin{bmatrix} 1 & 0 & 1 \\ 0 & 1 & 1 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 0 & -1 & 1 \\ 1 & 0 & -1 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} \mathbf{0} & \mathbf{-1} & \mathbf{2} \\ \mathbf{1} & \mathbf{0} & \mathbf{0} \\ \mathbf{0} & \mathbf{0} & \mathbf{1} \end{bmatrix}$$
4. **Transform Each Vertex**:
   * **Vertex A $(0, 0)$**:
     $$\begin{bmatrix} 0 & -1 & 2 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix} = \begin{bmatrix} 2 \\ 0 \\ 1 \end{bmatrix} \implies \mathbf{A'(2, 0)}$$
   * **Vertex B $(2, 0)$**:
     $$\begin{bmatrix} 0 & -1 & 2 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 2 \\ 0 \\ 1 \end{bmatrix} = \begin{bmatrix} 2 \\ 2 \\ 1 \end{bmatrix} \implies \mathbf{B'(2, 2)}$$
   * **Vertex C $(1, 2)$**:
     $$\begin{bmatrix} 0 & -1 & 2 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 \\ 2 \\ 1 \end{bmatrix} = \begin{bmatrix} 0 \\ 1 \\ 1 \end{bmatrix} \implies \mathbf{C'(0, 1)}$$
* **Final Coordinates**: $\mathbf{A'(2, 0), B'(2, 2), C'(0, 1)}$.

---

### Question 2: Phong Illumination Components & Calculation (10 Marks)
* Explain Ambient, Diffuse, and Specular reflection with neat diagrams and derive the Phong lighting equation.

**Model Answer**:
1. **Ambient Reflection**: Represents uniform background light that has been scattered so many times in the scene that its source direction is indeterminable ($I_{\text{amb}} = k_a I_a$).
2. **Diffuse Reflection (Lambert's Cosine Law)**: Light scattered uniformly in all directions when striking a rough surface. The intensity observed is directly proportional to the cosine of the angle between surface normal $\vec{N}$ and incident light vector $\vec{L}$ ($I_{\text{diff}} = k_d I_l \cos\theta = k_d I_l (\vec{N} \cdot \vec{L})$).
3. **Specular Reflection**: Responsible for the bright shiny highlight on polished or metallic surfaces. The reflected ray $\vec{R}$ is concentrated in a tight cone around the angle of reflection ($I_{\text{spec}} = k_s I_l (\vec{R} \cdot \vec{V})^\alpha$).
