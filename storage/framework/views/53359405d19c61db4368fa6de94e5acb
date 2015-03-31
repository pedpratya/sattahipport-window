
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="initial-scale=1.0, user-scalable=no" />
<meta http-equiv="content-type" content="text/html; charset=UTF-8"/>
<title>fixed grid</title>
	<style>
		html, body, #map-canvas {
			height: 100%;
			margin: 0px;
			padding: 0px
		}
		.customBox {
			background: yellow;
			border: 1px solid red;
			position: absolute;

		}		
	</style>
<script type="text/javascript" src="https://maps.googleapis.com/maps/api/js?v=3.8&libraries=geometry&sensor=false"></script>
<script type="text/javascript">
var map;
var infowindow = new google.maps.InfoWindow();
var rectArr=[];
var cols=["yellow"];

function TxtOverlay(pos, txt, cls, map){

	// Now initialize all properties.
	this.pos = pos;
	this.txt_ = txt;
	this.cls_ = cls;
	this.map_ = map;

	// We define a property to hold the image's
	// div. We'll actually create this div
	// upon receipt of the add() method so we'll
	// leave it null for now.
	this.div_ = null;

	// Explicitly call setMap() on this overlay
	this.setMap(map);
}

TxtOverlay.prototype = new google.maps.OverlayView();

TxtOverlay.prototype.onAdd = function(){

	// Note: an overlay's receipt of onAdd() indicates that
	// the map's panes are now available for attaching
	// the overlay to the map via the DOM.

	// Create the DIV and set some basic attributes.
	var div = document.createElement('DIV');
	div.className = this.cls_;

	div.innerHTML = this.txt_;

	// Set the overlay's div_ property to this DIV
	this.div_ = div;
	var overlayProjection = this.getProjection();
	var position = overlayProjection.fromLatLngToDivPixel(this.pos);
	div.style.left = position.x + 'px';
	div.style.top = position.y + 'px';
	// We add an overlay to a map via one of the map's panes.

	var panes = this.getPanes();
	panes.floatPane.appendChild(div);
}

TxtOverlay.prototype.draw = function(){


	var overlayProjection = this.getProjection();

	// Retrieve the southwest and northeast coordinates of this overlay
	// in latlngs and convert them to pixels coordinates.
	// We'll use these coordinates to resize the DIV.
	var position = overlayProjection.fromLatLngToDivPixel(this.pos);


	var div = this.div_;
	div.style.left = position.x + 'px';
	div.style.top = position.y + 'px';

}

//Optional: helper methods for removing and toggling the text overlay.  
TxtOverlay.prototype.onRemove = function(){
	this.div_.parentNode.removeChild(this.div_);
	this.div_ = null;
}

TxtOverlay.prototype.hide = function(){
	if (this.div_) {
		this.div_.style.visibility = "hidden";
	}
}

TxtOverlay.prototype.show = function(){
	if (this.div_) {
		this.div_.style.visibility = "visible";
	}
}

TxtOverlay.prototype.toggle = function(){
	if (this.div_) {
		if (this.div_.style.visibility == "hidden") {
			this.show();
		}
		else {
			this.hide();
		}
	}
}

TxtOverlay.prototype.toggleDOM = function(){
	if (this.getMap()) {
		this.setMap(null);
	}
	else {
		this.setMap(this.map_);
	}
}

function initialize() {
	var rectangle;
	var coachella = new google.maps.LatLng(12.621400150386933, 100.92527881064643);
	var myOptions = {
		zoom: 15,
		center: coachella,
		mapTypeId: google.maps.MapTypeId.TERRAIN
	};
	map = new google.maps.Map(document.getElementById("map-canvas"),
			myOptions);
	
	drawRects();
	
	google.maps.event.addListener(map, 'click', function(event) {
		var sidediv = document.getElementById('content-window');
		sidediv.innerHTML = 'lat/lng = ' + event.latLng.toString() ;	
	});
 
} 

function drawRects () {
	var MM = ["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O"];
	var NW=new google.maps.LatLng(12.63038336,100.9123909);
	var width = 15;
	var height = 10;
	var grid = 200;
	
	var NS = google.maps.geometry.spherical.computeOffset(NW,grid,90);
	var SS = google.maps.geometry.spherical.computeOffset(NW,grid,180);	
	for (var i = 0; i < height; i++) {
		NE = google.maps.geometry.spherical.computeOffset(NS,i*grid,180);
		SW = google.maps.geometry.spherical.computeOffset(SS,i*grid,180);
		for (var a = 0; a < width; a++) {
			var rectangle = new google.maps.Rectangle();
			var rectOptions = {
					//name: MM[a] + (i+1),
					strokeColor: "#FF0000",
					strokeOpacity: 0.8,
					strokeWeight: 1,
					fillColor: cols[Math.floor(Math.random()*cols.length)],
					fillOpacity: 0.35,
					map: map,
					bounds: new google.maps.LatLngBounds(SW,NE)
			};
			rectangle.setOptions(rectOptions);
			var name= MM[a] + (i+1);
			var nn = google.maps.geometry.spherical.computeOffset(NE,grid,-90);;
			bindWindow(rectangle,name,nn);
			
			customTxt = "<div> "+name+" </div>";
			txt = new TxtOverlay(nn,customTxt,"customBox",map );
			
			var SW = google.maps.geometry.spherical.computeOffset(SW,grid,90);
			var NE = google.maps.geometry.spherical.computeOffset(NE,grid,90);
		}
	}
}
	
function bindWindow(rectangle,num,nn){
	google.maps.event.addListener(rectangle, 'click', function(event) {
		var sidediv = document.getElementById('content-window');
		sidediv.innerHTML = 'position = ' + num + '<br> lat/lng = ' + nn;
	});
}
	
	google.maps.event.addDomListener(window, 'load', initialize);
	
</script>
</head>
<body>
	<div id="map-canvas" style="width:80%; height:100%; float:left"></div>
	<div id="content-window" style="width:20%; height:100%; float:left"></div> 
</body>
</html>
