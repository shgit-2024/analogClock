# analogClock
//Create analog clock
<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <script type="text/javascript" charset="utf8" src="https://code.jquery.com/jquery-3.7.0.js"></script>
    <script type="text/javascript" charset="utf8" src="https://cdn.datatables.net/1.13.7/js/jquery.dataTables.min.js"></script>
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.13.7/css/jquery.dataTables.min.css">
    <style>
      html, body {
        margin: 0;
		padding: 0;
      }
	 #analog-clock{
		width: 500px;
		height: 500px;
		margin: auto;
		border: 5px solid black;
	}
    </style>
  </head>
  <body>
      <div id="analog-clock" class="analog-clock">
		<canvas id="myCanvas" width="500" height="500"></canvas>
		<p> Developed by Shimon Hansda, 15-03-2024</p>
	  </div>
    <script>
      $(document).ready(function(){
        const canvas = document.getElementById('myCanvas');
		const context = canvas.getContext('2d');
		//translate the (0,0) position in center of the canvas
		context.translate(canvas.width/2,canvas.height/2);
		
		//rotate canvas -90 degree to position 0 degree in north;
		context.rotate(-Math.PI/2);
		setInterval(function(){
			//crear the whole canvas to redraw update clock status
			context.clearRect(0, 0, canvas.width, canvas.height);
			
			const centerX = 0; 
			const centerY = 0; 
			radius = 210;
			//outer circle
			context.beginPath();
			context.arc(centerX, centerY, radius, 0, 2 * Math.PI, false);
			context.fillStyle = 'white';
			context.fill();
			context.lineWidth = 7;
			context.strokeStyle = '#003300';
			context.stroke();
			context.closePath();
			//draw clock body circle
			radius = 200;
			context.beginPath();
			context.arc(centerX, centerY, radius, 0, 2 * Math.PI, false);
			context.fillStyle = 'white';
			context.fill();
			context.lineWidth = 5;
			context.strokeStyle = '#ffae47';
			context.stroke();
			context.closePath();
			//draw circle in center
			radius = 6;
			context.beginPath();
			context.arc(centerX, centerY, radius, 0, 2 * Math.PI, false);
			context.fillStyle = 'black';
			context.fill();
			context.lineWidth = 10;
			context.strokeStyle = '#000000';
			context.stroke();
			context.closePath();
			//draw minutes points
			context.lineWidth = 1;
			degreeMinute = 360/60;
			rMP =  190;
			thetaMinute = 0;
			for(var i = 0; i < 60; i++){
				thetaMinute = thetaMinute + degreeMinute;
				x1 = rMP * Math.cos(thetaMinute * Math.PI / 180);
				y1 = rMP * Math.sin(thetaMinute * Math.PI / 180);
				context.beginPath();
				context.arc(x1, y1, 3, 0, Math.PI*2);
				context.fillStyle = 'black';
				context.fill();
				context.stroke();
				context.closePath();
			}
			
			//draw hours points
			context.lineWidth = 1;
			degreeHour = 360/12;
			rHP =  190;
			thetaHour = 0;
			for(var i = 0; i < 12; i++){
				thetaHour = thetaHour + degreeHour;
				x1 = rHP * Math.cos(thetaHour * Math.PI / 180);
				y1 = rHP * Math.sin(thetaHour * Math.PI / 180);
				context.beginPath();
				context.arc(x1, y1, 7,0, Math.PI*2);
				context.fillStyle = 'black';
				context.fill();
				context.stroke();
				context.closePath();
			}
			//draw hours label text
			//save previous drawing to restore back 
			context.save();
			//rotate back to initial position for draw text
			context.rotate(Math.PI/2);
			degreeHour = 360/12;
			rHP =  160;
			thetaHour = 0;
			for(var i = 4; i < 16; i++){
				thetaHour = thetaHour + degreeHour;
				x1 = rHP * Math.cos(thetaHour * Math.PI / 180);
				y1 = rHP * Math.sin(thetaHour * Math.PI / 180);
				context.font = "30px Arial";
				context.fillStyle = "red";
				context.textAlign = "center";
				context.textBaseline = "middle";
				labelText = i;
				if(i == 13) labelText = 1;
				else if(i == 14) labelText = 2;
				else if(i == 15) labelText = 3;
				context.fillText(labelText, x1, y1);
			}
			//restore previous drawing saved by save() method
			context.restore();
						
			//get the system time	
			d = new Date();
			hours = d.getHours();
			if(hours > 12) hours = hours - 12;
			minutes = d.getMinutes();
			seconds = d.getSeconds();
			//draw hours hands
			context.lineWidth = 7;
			x1 = 0;
			y1 = 0;
			rH =  100;
			thetaHours = hours * 30;
			//moves the hours hand more 6 degree of completion of every 12 minutes
			if(minutes >= 12 && minutes < 24) thetaHours = thetaHours + 6;
			else if(minutes >= 24 && minutes < 36) thetaHours = thetaHours + 12;
			else if(minutes >= 36 && minutes < 48) thetaHours = thetaHours + 18;
			else if(minutes >= 48 && minutes < 60) thetaHours = thetaHours + 24;
			context.beginPath();
			context.moveTo(x1, y1);
			context.lineTo(x1 + rH * Math.cos(Math.PI * thetaHours / 180.0), y1 + rH * Math.sin(Math.PI * thetaHours / 180.0));
			context.strokeStyle = '#330000';
			context.stroke();
			context.closePath();
			// draw minute hands
			context.lineWidth = 3;
			x1 = 0;
			y1 = 0;
			rM =  140;
			
			thetaMimutes = minutes * 6;
			context.beginPath();
			context.moveTo(x1, y1);
			context.lineTo(x1 + rM * Math.cos(Math.PI * thetaMimutes / 180.0), y1 + rM * Math.sin(Math.PI * thetaMimutes / 180.0));
			context.strokeStyle = '#003300';
			context.stroke();
			context.closePath();
			//draw seconds hands
			context.lineWidth = 1;
			x1 = 0;
			y1 = 0;
			rS =  180;
			
			thetaSecond = seconds * 6;
			context.beginPath();
			context.moveTo(x1, y1);
			context.lineTo(x1 + rS * Math.cos(Math.PI * thetaSecond / 180.0), y1 + rS * Math.sin(Math.PI * thetaSecond / 180.0));
			context.strokeStyle = '#000040';
			context.stroke();
			context.closePath();
			//draw circle in center
			radius = 3;
			context.beginPath();
			context.arc(centerX, centerY, radius, 0, 2 * Math.PI, false);
			context.fillStyle = 'white';
			context.fill();
			context.lineWidth = 10;
			context.strokeStyle = '#000000';
			context.stroke();
			context.closePath();
			
		},1000);
		
      });
    </script>
  </body>
</html>

