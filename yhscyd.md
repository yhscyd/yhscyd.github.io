<html>
  <head>
    <meta charset="utf-8">
  </head>
  <body>
    <br>遗憾是常有的
    <br>沉思往事立斜阳，当时只道是寻常。<br>谁念西风独自凉，萧萧黄叶闭疏窗。
  </body>
  </html>
  
  
  <!DOCTYPE html>
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html;charset=utf-8">
		<br><title>Chapter 1. First WebGL Demo</title>
		<!-- 顶点着色器和片元着色器代码 -->
		<!-- 获取顶点位置 -->
		<script id="vertex-shader" type="x-shader/x-vertex">
			attribute vec4 vPosition;
		
			void main(){
				gl_Position = vPosition;
			}
		</script>
		<script id="fragment-shader" type="x-shader/x-fragment">
			precision mediump float;
			varying vec4 fColor;
			void main(){
				gl_FragColor = vec4( -1.0, -1.0, 0.0, 1.0 );
			}
		</script>
		<script id="vertex-shader2" type="x-shader/x-vertex">
			attribute vec4 vPosition2;
		
			void main(){
				gl_Position = vPosition2;
			}
		</script>
		<script id="fragment-shader2" type="x-shader/x-fragment">
			precision mediump float;
			varying vec4 fColor2;
			void main(){
				gl_FragColor = vec4( 1.0, 1.0, 0.0, 1.0 );
			}
		</script>
		<!-- 一组相关的JS库 -->
		<script type="text/javascript" src="js/common/webgl-utils.js"></script>
		<script type="text/javascript" src="js/common/initShaders.js"></script>
		<script type="text/javascript" src="js/common/glMatrix-0.9.5.min.js"></script>
		<!-- 绘制三角形的JS代码 -->
		<script type="text/javascript" src="js/ch01/triangle.js"></script>
	</head>
	<body>
		<canvas id="triangle-canvas" style="border:none;" width="500" height="500"></canvas>
	</body>
</html>

"use strict";

var gl;
var points;

window.onload = function init(){
	var canvas = document.getElementById( "triangle-canvas" );
	gl = WebGLUtils.setupWebGL( canvas );
	if( !gl ){
		alert( "WebGL isn't available" );
	}

	// Three Vertices
	var vertices = [
		-1.0, -1.0, 
		 -1.0,  0.0, 
		 0.0, 0.0, 
		 -1.0, -1.0,
		 0.0, -1.0,
		 0.0,  0.0,
		 // 0.0, 1.0,
		 // 1.0,  1.0,
		 // 0.0,  -1.0
		/* -0.5, -0.5,
		 0.0, 0.5,
		 0.5, -0.5*/
	];
	
	
	
	// Configure WebGL
	gl.viewport( 0, 0, canvas.width, canvas.height );
	gl.clearColor( 1.0, 1.0, 1.0, 1.0 );

	// Load shaders and initialize attribute buffers
	var program = initShaders( gl, "vertex-shader", "fragment-shader" );
	gl.useProgram( program );
	
	// Load the data into the GPU
	var bufferId = gl.createBuffer();
	gl.bindBuffer( gl.ARRAY_BUFFER, bufferId );
	gl.bufferData( gl.ARRAY_BUFFER, new Float32Array( vertices ), gl.STATIC_DRAW );



	// Associate external shader variables with data buffer
	var vPosition = gl.getAttribLocation( program, "vPosition" );
	gl.vertexAttribPointer( vPosition, 2, gl.FLOAT, false, 0, 0 );
	gl.enableVertexAttribArray( vPosition );
	
	render();
	var vertices2 = [
		// -1.0, -1.0, 
		//  -1.0,  1.0, 
		//  0.0, 1.0, 
		//  -1.0, -1.0,
		//  0.0, -1.0,
		//  0.0,  1.0,
		 0.0, 1.0,
		 1.0,  1.0,
		 0.0,  0.0
		/* -0.5, -0.5,
		 0.0, 0.5,
		 0.5, -0.5*/
	];
	var program2 = initShaders( gl, "vertex-shader2", "fragment-shader2" );
	gl.useProgram( program2 );
	var bufferId = gl.createBuffer();
	gl.bindBuffer( gl.ARRAY_BUFFER, bufferId );
	gl.bufferData( gl.ARRAY_BUFFER, new Float32Array( vertices2 ), gl.STATIC_DRAW );
	
	var vPosition2 = gl.getAttribLocation( program2, "vPosition2" );
	gl.vertexAttribPointer( vPosition2, 2, gl.FLOAT, false, 0, 0 );
	gl.enableVertexAttribArray( vPosition2 );
	render2();
}

function render(){
	gl.clear( gl.COLOR_BUFFER_BIT );
	//gl.drawArrays( gl.TRIANGLE_FAN, 0, 4 );
	gl.drawArrays( gl.TRIANGLES, 0, 6 );

	//gl.drawArrays( gl.TRIANGLE_FANS, 3, 6 );
}
function render2(){
	// gl.clear( gl.COLOR_BUFFER_BIT );
	//gl.drawArrays( gl.TRIANGLE_FAN, 0, 4 );
	gl.drawArrays( gl.TRIANGLES, 0, 3 );
	//gl.drawArrays( gl.TRIANGLE_FANS, 3, 6 );
}
