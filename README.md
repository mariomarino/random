<html>
<head></head>
<body>
<style>
td{
	width:50px;
	height:50px;
	border: solid black 1px;
}
.d{
	background-color:lightgray;
}
.a{
	background-color:green;
}
</style>
<div id="qui"></div>
<table id="table"></table>
<script>
	/*
	function regole(x, y, z){
		var val = x * 4 + y * 2+z;
		switch(val){
			case 3: case 4: case 5: case 7: return 1;
			default: return 0;
		}
	}
	
		  var array = new Array();
		  var traffico = "";
		  var div = document.getElementById("qui");
		  for(var x = 0;x<60;x++){
			  array[x] = Math.floor(Math.random()*2);
			  console.log(array[x]);
			  traffico = traffico + array[x]==0?" ":"\u2588";
		  }
			div.innerHTML=traffico;
			
		  var tmp = new Array();
		  var toprint = "";
		  
		  setInterval(function(){
			traffico = "";
			 
			  for(var y = 0;y<array.length;y++){
				tmp[y] = regole(array[(array.length+y-1)%array.length],array[(y)%array.length],array[(y+1)%array.length]);
				toprint = tmp[y] == 0 ? " ":"\u2588";
				traffico = traffico + toprint;
				
			}
			console.log(traffico);
				for(var x = 0;x<array.length;x++)
					array[x] = tmp[x];
				div.innerHTML=traffico;
		  },50);
		*/
	function stampa(g){
		var cella;
		for(var x = 0;x<g.length;x++)
			for(var y = 0;y<g[x].length;y++){
				cella = document.getElementById('r'+x+'c'+y);
				if(g[x][y]==1)
					cella.setAttribute('class','a');
				else 
					cella.setAttribute('class','d');
			}
			
	}
	function creaGriglia(g){
		var table = document.getElementById("table");
		var riga;
		var cella;
		
		for(var x = 0;x<g.length;x++){
			riga = document.createElement("tr");
			riga.setAttribute('id','row'+x)
			for(var y = 0;y<g[x].length;y++){
				cella = document.createElement("td");
				cella.setAttribute('class','d');
				cella.setAttribute('id','r'+x+'c'+y)
				riga.appendChild(cella);
			}
			table.appendChild(riga);
		}
	}
	var griglia = new Array(10);
	
	var tmp = new Array(griglia.length);
	for(var x = 0;x<griglia.length;x++)
		griglia[x]=new Array(griglia.length);
	for(var x = 0;x<griglia.length;x++){
		tmp[x] = new Array(griglia[x].length);
		for(var y = 0;y<griglia[x].length;y++){
			griglia[x][y] = Math.floor(Math.random()*2);
			tmp[x][y] = griglia[x][y];
		}
	}
	creaGriglia(griglia);
	stampa(griglia);
	var sum = 0;
	function regole(x, y, g){
		sum = 0;
		switch(x){
			case 0: 
				if(y==0)
					sum+=g[x][y+1]+g[x+1][y]+g[x+1][y+1];
				else if(y==g[x].length-1)
					sum+=g[x][y-1]+g[x+1][y]+g[x+1][y-1];
				else 
					sum+=g[x][y-1]+g[x][y+1]+g[x+1][y+1]+g[x+1][y]+g[x+1][y-1];
				break;
			case g.length-1: 
				if(y==0)
					sum+=g[x-1][y]+g[x-1][y+1]+g[x][y+1];
				else if(y==g[x].length-1)
					sum+=g[x][y-1]+g[x-1][y]+g[x-1][y-1];
				else
					sum+=g[x][y-1]+g[x][y+1]+g[x-1][y]+g[x-1][y-1]+g[x-1][y+1];
				break;
			default: 
				switch(y){
					case 0:
						sum+=g[x][y+1]+g[x-1][y]+g[x-1][y+1]+g[x+1][y]+g[x+1][y+1];
						break;
					case g[x].length-1: 
						sum+=g[x][y-1]+g[x-1][y]+g[x-1][y-1]+g[x+1][y]+g[x+1][y-1];
						break;
					default:
						sum+=g[x][y-1]+g[x][y+1]+g[x-1][y-1]+g[x-1][y]+g[x-1][y+1]+g[x+1][y-1]+g[x+1][y]+g[x+1][y+1];
						break;
				}
				break;
		}
		if(griglia[x][y]==1){
			if(sum == 2|| sum == 3) return 1;
			return 0;
		}
		if(sum==3) return 1;
		return 0;
	}
	var eh = 0;
	setInterval(function(){
	
		for(var x = 0;x<griglia.length;x++)
			for(var y = 0;y<griglia[x].length;y++){
				eh = regole(x,y, griglia);
				tmp[x][y] = eh;
			}
			for(var q = 0;q<griglia.length;q++)
				for(var w = 0;w<griglia[q].length;w++)
					griglia[q][w]=tmp[q][w];
		stampa(griglia);
	},1000);
</script>
</body>
</html>
