---
author: admin
date: 2014-03-02 22:10:38+00:00
draft: true
title: '#TwitterMetrics: Happiness Heat Map :)'
type: post
url: /twitter-happiness-heatmap/
---

The Twitter Happiness Heat Map attempts to find the happiest place on earth by searching Twitter for the 'smiley face emoticon' - :) - and creating a heatmap of where the returned tweets originated.


[d3-link]
[/d3-link]
[d3-source canvas="ChartDivHeatmap"]
d3.json("/json/TwitterHeatmapData.php", function(error, data) {

data.forEach(function(d) {
d.row = +d.row;
d.col = +d.col;
d.Total = +d.Total;
})
;

// Set the dimensions of the graphic
var DivIdToScaleTo = "WidthDivHeatmap"; // Page id that will scale the width
var HeightWidthRatio = 480/740; // Height as a fixed ratio of width

var ele = document.getElementById(DivIdToScaleTo);
var eleStyle = window.getComputedStyle(ele);

//height of each row in the heatmap
//width of each column in the heatmap

var NumberOfColumns = 840;
var NumberOfRows = 480;

var gridSize = (parseInt(eleStyle.width) / NumberOfColumns),
h = gridSize,
w = gridSize,
rectPadding = 10
;

// Set the max value
var MaxTotal = d3.max(data, function(d) { return d.Total; });

var colorLow = 'rgba(255, 255, 255, 0.15)',
colorHigh = 'rgba(255, 255, 255, 1)'
;

var margin = {top: 10, right: 10, bottom: 10, left: 10},
width = (w * NumberOfColumns)// - margin.left - margin.right,
height = (w * NumberOfRows)// - margin.top - margin.bottom
;

var colorScale = d3.scale.linear()
.domain([0, MaxTotal])
.range([colorLow, colorHigh])
;

var svg = d3.select(".ChartDivHeatmap").append("svg")
.attr("width", width)// + margin.left + margin.right)
.attr("height", height)// + margin.top + margin.bottom)
.append("g")
//.attr("transform", "translate(" + margin.left + "," + margin.top + ")")
;

var heatMap = svg.selectAll(".ChartDivHeatmap")
.data(data, function(d) { return d.row + ':' + d.col; })
.enter()
.append("svg:rect")
.attr("x", function(d) { return d.col * w; })
.attr("y", function(d) { return d.row * h; })
.attr("width", function(d) { return w; })
.attr("height", function(d) { return h; })
.style("fill", function(d) { return colorScale(d.Total); })
;

});
[/d3-source]

### The Data


Getting the data out of Twitter uses the [Twitter API](https://dev.twitter.com/), accessed using the [python-twitter](https://code.google.com/p/python-twitter/https://code.google.com/p/python-twitter/) library. It’s cobbled together using some MySQL databases, formatted into JSON files and displayed using the [d3.js](http://d3js.org/) graphical library.

The python script runs on a [Raspberry Pi](http://www.raspberrypi.org/) and searches Twitter for the :) emoticon every 5 minutes, returning the first 200 results each time.

In order to plot the geographic position the twitter user must have  geographic location activated for their account - only ~2% of users seem to have this so a lot of the tweets are just discarded. In addition, a user's location is set to where their account is registered rather than the location they tweeted from - this means that if I had my (London) location registered and tweeted "_#OMG these Kangaroos are awesome! :) #Kangaroo #AussieRules_" while on holiday in Australia then the increase in happiness would be measured in the UK, not 'down under'.

The other thing to consider is that when you search for :) on Twitter it also returns tweets that contain characters such as **:D **and **:-)**. They all seem pretty happy though so the script doesn't filter them out.

I started the script at the start of February 2014 - so far it's collected [php function=7] tweets :)

### The Projection

The Twitter data returns the location of each tweet as latitude and longitude which need to be plotted onto a plain using a [map projection](http://en.wikipedia.org/wiki/Map_projection).

There are a dizzing array of different projections available, from the [Winkel Tripel](http://en.wikipedia.org/wiki/Winkel_tripel_projection) projection to the more esoteric [Hammer retroazimuthal](http://en.wikipedia.org/wiki/Hammer_retroazimuthal_projection) projection. The graphic uses the classic [Mercator](http://en.wikipedia.org/wiki/Mercator_projection) projection as it's the one most viewers will be familiar with (important where the image of the map has no outlines and instead emerges with the data).

It's critics say the Mercator projection distorts the landmass around the poles - unless there are a disproportionate number of Twitter users in [Nuuk](http://en.wikipedia.org/wiki/Nuuk) though, it shouldn't distort the graphic too much.
