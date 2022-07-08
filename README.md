# umbrella-like-pendulum

##PREVIEW

<!DOCTYPE html>
<html>
	<head>
		<title>Page Title</title>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1, shrink-to-fit=no"/>
	</head>
	<body>
<style>
body {
 margin:0;
 padding:0;   
}
#canvas{
 position:absolute;
 background-color:#000;
}
</style>
	 <canvas id = "canvas"></canvas>
<script>

const nums = 70;
 const canvas = document.querySelector('canvas');
 const ctx = canvas.getContext('2d'); 
 
 let {PI,sin,cos,atan2,sqrt,pow,random} = Math;
 
 canvas.height = window.innerHeight;
 canvas.width = window.innerWidth;
 
 class Pendulum {
 
  constructor(params){
   this.px = params.px;
   this.py = params.py;
   this.prad = params.prad;
   this.pcol = params.pcol;
   this.len = params.len;
   this.theta = params.theta;
   this.bx = sin(this.theta)*this.len + this.px;
   this.by = cos(this.theta)*this.len + this.py;
   this.brad = params.brad;
   this.bcol = params.bcol;
   this.v = 0;
   this.draw();
  }
  
  draw(){
	ctx.beginPath();
	ctx.moveTo(this.px,this.py);
	ctx.lineTo(this.bx,this.by);
	ctx.lineWidth = 1.0;
	ctx.strokeStyle = `red`;
	ctx.stroke();
	ctx.beginPath();
	ctx.arc(this.px,this.py,this.prad,0,PI*2);
	ctx.fillStyle = this.pcol;
	ctx.fill();
	ctx.beginPath();
	ctx.arc(this.bx,this.by,this.brad,0,PI*2);
	ctx.fillStyle = this.bcol;
	ctx.fill();
  }
  
  update(){
  this.bx = sin(this.theta)*this.len + this.px;
  this.by = cos(this.theta)*this.len + this.py; 
  this.v += -(gravity * sin(this.theta)) /this.len;
   this.theta += this.v;
  //this.v /= resistance;
   this.draw();
  }  
}

 let params = {
	 px:canvas.width/2,
	 py:150,
	 prad:2,
	 pcol:`green`,
	 len:700,
	 brad:18,
	 bcol:`red`,
	 theta:PI/6
  };
  
  const pendulums = [];
  const gravity = 0.5;
  const resistance = 1.004;
  const lengths = [];
  const maxL = 500;
  const sync = 65;
  
  init();
  
  
  function init(){
  
  let hue = 160; 
  for(let i = sync;i<sync+nums;i++){
  let l = maxL*pow((sync/i),2);
  lengths.push(l); } 
  
  for(let i = 0;i<nums;i++){
 // params.brad += 0.4;
  params.len = lengths[i];
  params.bcol = `hsl(${hue+=-4},70%,50%)`;
  pendulums.push(new Pendulum(params));  }
  
  requestAnimationFrame(animate);
  }
  
  function animate(){
   ctx.clearRect(0,0,canvas.width,canvas.height);
   pendulums.forEach((pendulum)=>{
		pendulum.update();});
   
   requestAnimationFrame(animate);
  } 
</script>
	</body>
</html>
