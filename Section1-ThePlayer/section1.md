## Section 1: Player and Player Movement

### Section Overview

To start off, lets create the player game object. Right-click some empty space within the hierarchy and select *3D Object -> Capsule*. Rename the capsule object to *Player*. 

Select your newly created *Player* in the hierarchy, and look over to the inspector window, there will be three components: 

- Transform

    - Keeps track of the game object's location, orientation, and size.

- Capsule (Mesh filter)
    - Holds a reference to the shape/mesh of the game object.

- Mesh Renderer
    - Displays the object onto the screen by shooting rays and stuff (take cs184 if you want to learn more).

- Capsule Collider
    - Collide with objects   

### Input Manager



### Setting velocity
![alt text](image.png)

Now, open up the *PlayerController.cs* script located in *Assets -> Scripts -> PlayerController.cs*. 

- `p_Velocity` The current direction the player is moving

- `cc_rb` is a reference to the attached rigidbody component, where we can manipulate the velocity and move the player.

We initialize the player's `p_Velocity` to `Vector2.zero` in the `Awake()` function. 

{:note}
>`Awake()` is called once immediately after the GameObject to which the script is attached to is instantiated. It is also called before any `Start()` function, making it ideal for initial setup tasks.

In the `Update()` function, we are first reading the keyboard input from the user.

- `float forward = Input.GetAxis("Vertical")` returns -1 or 1 while the `W` or `S` keys are held down, respectively. 

- `float right= Input.GetAxis("Horizontal")` returns -1 or 1 while the `A` or `D` keys are held down, respectively.

### Moving the player's position (fixed update)

#### Task 1.1 (Move Position)

Now, lets move the player based off the user input. In order to do this, we need to find the vector that points the direction the player is facing and is scaled by the distance the player moves per frame, but first need to understand a bit of vector math.

To start off, we can use the player's position and add the direction vector the player is facing. 

Next, we need to normalize and scale this direction vector by the distance it needs to move. This is given by the speed, framerate, and velocity.

Here are some important functions and proerties you'll need to use

- `cc_Rb.MovePosition(Vector3 direction)` takes in a vector and moves the player towards that direction.

- `cc_Rb.position` the current position of the player

- `transform.forward` a vector pointing at the direction the player is fixing

- `

- `cc_Rb` holds the reference to the rigidbody component of the host game object.

{:note}
> `Update()` runs every frame. For example, a system running 60 fps will call `Update()` 60 times per second. The framerate varies between systems, so the amount of times `Update()` is called will also vary between systems. `FixedUpdate()` negates this by running a constant (50) amount of times independent of the host system.

### Fixing up the physics



### Setting up the environment

### Rotating the player

### Making the player into a prefab

