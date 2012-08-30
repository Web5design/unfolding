---
layout: page
title: Markers 
description: How to use point, line, and polygon markers, how to style them, and how to create your own.
group: tutorials-beginner
thumbnail: ../assets/images/tutorials/basic-map-thumb.png
finalimage: 
---

{% include JB/setup %}


## Marker

Displaying markers on a map is very straight-forward. Just create a marker with a location and add it to the map once.
Here, for instance, we are creating two point markers, one for Berlin and one for Dublin.

	Location berlinLocation = new Location(52.5, 13.4);
	Location dublinLocation = new Location(53.35, -6.26);
  	
	// Create point markers for locations
	SimplePointMarker berlinMarker = new SimplePointMarker(berlinLocation);
	SimplePointMarker dublinMarker = new SimplePointMarker(dublinLocation);
  	
	// Add markers to the map
	map.addMarkers(berlinMarker, dublinMarker);

They will be drawn automatically on top of the map, always at the correct position.

![Simple Point Markers](../assets/images/tutorials/markers-simple.png)

Unfolding provides a default marker style, and has point, line, and polygon markers out of the box.


## Style your markers

The easiest method to style your marker is to draw it yourself, instead of adding it to the map.

For this you need to make the marker global, i.e. define the variable first in your sketch (line 2), then assign a new marker in setup (line 11).
In draw() get the current position of the marker (line 19), and draw some visual representation with Processing's drawing functions (lines 20-23).

	UnfoldingMap map;
	SimplePointMarker berlinMarker;

	void setup() {
	  size(800, 600, GLConstants.GLGRAPHICS);

	  map = new UnfoldingMap(this);
	  MapUtils.createDefaultEventDispatcher(this, map);

	  Location berlinLocation = new Location(52.5, 13.4);
	  berlinMarker = new SimplePointMarker(berlinLocation);

	  // Do not add marker to the map
	}

	void draw() {
	  map.draw();

	  ScreenPosition berlinPos = berlinMarker.getScreenPosition(map);
	  strokeWeight(1);
	  stroke(0, 100);
	  fill(0, 200, 0, 100);
	  ellipse(berlinPos.x, berlinPos.y, 20, 20);
	}

`marker.getScreenPosition(map)` returns the current x,y-position of the marker on the map. The method converts the geo-location of that marker to the current `ScreenPosition` on the canvas of the sketch.

![Styled marker with fixed size](../assets/images/tutorials/marker-fixed-size.png)


Now, you can style your markers in any way you want. In the following example we draw the marker as ring with a text label.

![Marker as ring with label](../assets/images/tutorials/marker-style-1.png)

Note, that we draw a zoom dependent marker, i.e. the marker will be bigger in zoomed in, and smaller in zoomed out maps.

	ScreenPosition posLondon = markerLondon.getScreenPosition(map);
	strokeWeight(16);
	stroke(200, 0, 0, 100);
	noFill();
	// Zoom dependent marker size
	float s = map.getZoom();
	ellipse(posLondon.x, posLondon.y, s, s);
	fill(0);
	text("London", posLondon.x - textWidth("London") / 2, posLondon.y + 4);

(By drawing the marker yourself, the marker is rendered after the map. In special cases, such as when using the Processing2D renderer, this can result in slight lags. If you have experienced this you should try the second option: Create your own marker class.) 


## Create your own marker

By creating your own marker class you have full control over style and interaction handling. Besides, you will have cleaner code, and can manage hundreds of markers with a more elegant software architecture, as the drawing will be handled automatically by the map.

![Image marker](../assets/images/tutorials/markers-image.png)

See the [Image Marker example](examples/40_image-marker.html) for code on how to create an own marker class extending AbstractMarker.

(More following soon.)

## Line and polygon marker

(More following soon.)

## Multi marker

You can also create markers consisting of multiple sub markers.

The following example shows the shape for France and Corsica, both defined as (crude) polygons. Both areas are `SimplePolygonMarker` and combined in one `MultiMarker`. Only the `MultiMarker` is added to the map.

![Multi-Marker with two polygons](../assets/images/tutorials/marker-multi-select.png)

As you can see, this works also with selection handling. When the user moves the mouse over one of the areas, both are highlighted. 

(More following soon.)


## Creating data markers from GeoJSON, GPX & Co

If you want to load data from GeoJSON or various other file formats, take a look at our [Markers & Data tutorial](markers-data.html).




