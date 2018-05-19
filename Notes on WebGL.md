# WebGL


WebGL only cares about 2 things: **clipspace coordinates** and **colors**. Your job as a programmer using WebGL is to provide WebGL with those 2 things.

- Clipspace coordinates always go from `-1` to `+1` no matter what size your canvas is. 
- Colors in WebGL go from `0` to `1`.



## Shaders 

Code that runs in GPU. Written in GLSL. 

Two functions: 

- vertex shader
- fragment shader

Together they are called a **program**.


### Vertex shader

`gl_Position`: A variable the vertex shader is responsible for setting

Computes vertex positions. 

Based on these positions WebGL can then rasterize various kinds of primitives.

### Fragment shader


`gl_FragColor`: A variable the fragment shader is responsible for setting


When rasterizing, it calls a second user supplied function called a fragment shader. The fragment shader's job is to **compute a color for each pixel** of the primitive currently being drawn.





### Receiving data

Any data you want those functions to have access to must be provided to the GPU.

There are **4 ways** a shader can receive data:

1. **Attributes and buffers**

	**Buffers** are _arrays of binary data_ that you upload to the GPU. Usually buffers contain things like **positions, normals, texture coordinates, vertex colors**, etc.

	**Attributes** are used to specify _how to pull data out of your buffers_ and provide them to your vertex shader. For example you might put positions in a buffer as three 32bit floats per position. You would tell a particular attribute 
		- which buffer to pull the positions out of, 
		- what type of data it should pull out (3 component 32 bit floating point numbers), 
		- what offset in the buffer the positions start, 
		- and how many bytes to get from one position to the next.

	Buffers are not random access. Instead a vertex shaders is executed a specified number of times. Each time it's executed the next value from each specified buffer is pulled out assigned to an attribute.

2. **Uniforms**
	Uniforms are effectively global variables you set before you execute your shader program.

3. **Textures**
	Textures are arrays of data you can randomly access in your shader program. The most common thing to put in a texture is image data but textures are just data and can just as easily contain something other than colors.

4. **Varyings**
	Varyings are a way for a vertex shader to pass data to a fragment shader. Depending on what is being rendered, points, lines, or triangles, the values set on a varying by a vertex shader will be interpolated while executing the fragment shader.



## Bind points

WebGL lets us manipulate many WebGL resources on global bind points. You can think of bind points as internal global variables inside WebGL. First you bind a resource to a bind point. Then, all other functions refer to the resource through the bind point.


## Initialization Code

```JS
var positionAttributeLocation = gl.getAttribLocation(program, "a_position");
```

Create a buffer that our shader is expecting and bind it to a bind point

```JS
var positionBuffer = gl.createBuffer();
gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);
```

Now we can put data in the buffer, referencing it through the bind point:

Next we fill the data:

```JS
var positions = [
  0, 0,
  0, 0.5,
  0.7, 0,
];
gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(positions), gl.STATIC_DRAW);
```

## Render code


This code should get executed each time we want to render/draw.



