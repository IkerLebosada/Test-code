<!DOCTYPE html>
<head>
<title>Led Control-Iker-Test</title>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link rel="stylesheet" href="main.css">
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">

</head>
<body>
<font id="title">Led Control-Iker-Test</font><p><br>

<div id="content" class="row">
	<div class="col-sm-12  col-xs-12">
		<div id="analog" class="col-sm-6 col-xs-12">
			<div class="analogclass">
				<img class="analogsensor" src="Led.png" />
				<span class="analogvalue" id="lightvalue">none</span><br>
			</div>
			<p>
			<div class="analogclass">
				<img class="analogsensor" src="temp.png" />
				<span class="analogvalue" id="temperaturevalue">none</span>
			</div>
		</div>
		<div id="rgb" class="col-sm-6 col-xs-12">
			<img class="rgbled" src="img/rgb.png" />
			<table style="margin: 0 auto; margin-top: 10px;">
				<tr style="text-align: center;">
					<td><font color="red"><b>Red</b></font></td>
					<td><font color="green"><b>green</b></font></td>
					<td><font color="blue"><b>blue</b></font></td>
				</tr>
				<tr style="text-align: center;">
					<td id="redvalue">0</td>
					<td id="greenvalue">0</td>
					<td id="bluevalue">0</td>
				</tr>
				<tr>
					<td><input Onchange="changeColor();" type="range" id="rled" class="ledbar" value="0" min="0" max="255"></td>
					<td><input Onchange="changeColor();" type="range" id="gled" class="ledbar" value="0" min="0" max="255"></td>
					<td><input Onchange="changeColor();" type="range" id="bled" class="ledbar" value="0" min="0" max="255"></td>
				</tr>
			</table>
			<span id="showcolor"></span>
		</div>
	</div>
</div>

<script>
	
	var lightdata;
	var tempdata;
	var celsius;
	setup();
	// Analog
	function loop() {
		if(cpf){
	
			lightdata = cpf.get("light sensor");
			tempdata = cpf.get("temperature sensor");
			celsius = toCelsius(tempdata);	
			document.getElementById("lightvalue").innerHTML = lightdata;
			document.getElementById("temperaturevalue").innerHTML = celsius;
			
		}
		setTimeout(loop, 1000);
	}
	loop();
	
	// RGB
	function changeColor() {
		var Rled = document.getElementById("rled").value;
		var Gled = document.getElementById("gled").value;
		var Bled = document.getElementById("bled").value;
		
		document.getElementById("redvalue").innerHTML = Rled;
		document.getElementById("greenvalue").innerHTML = Gled;
		document.getElementById("bluevalue").innerHTML = Bled;
		
		document.getElementById("showcolor").style.backgroundColor = 'rgb(' + Rled + ',' + Gled + ',' + Bled + ')';
		
		if(cpf){
			cpf.setChainableLed("0," + Rled + "," + Gled + "," + Bled + ";");
		}
		
	}
	
	// Temperature
	function toCelsius(value) {
		var resistance = parseFloat((1023-value) * 10000 / value);
		var temperature = 1 / (Math.log(resistance / 10000) / 3975+1 / 298.15) - 273.15;
		
		return temperature.toFixed(2);
	}
	
	// cpf setup
	function setup(){
		if(cpf)
			cpf.setPinMode('["resetPin"],["setPinMode", "analog", 0, "INPUT"],["setPinMode", "analog", 1,"INPUT"],["grove_newChainableLED", 7, 8, 1]'); 
			//document.getElementById("demo").innerHTML += ret + "<br>";
	}
	
</script>
</body>
</html>
