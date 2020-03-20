---
author: admin
date: 2014-03-02 14:30:07+00:00
draft: true
title: '#TwitterMetrics: Daily Twitter Sentiment'
type: post
url: /twitter-sentiment/
---

The #TwitterMetrics project is about creating stories form everyday Twitter data. In this example I measure the sentiment of trending Twitter topics every 15 minutes using a Python script and plot the results using the [d3.js](https://github.com/mbostock/d3) library. You can follow the project on [Twitter](https://twitter.com/adamdynamic) to get regular updates.

[d3-link]
[TwitterSentimentLineChartStyles](http://adamdynamic.com/d3css/TwitterSentimentLineChartStyles.css)
[/d3-link]
[d3-source canvas="ChartDiv_TwitterMetrics"]

var DivIdToScaleTo = "WidthDiv_TwitterMetrics"; // Page id that will scale the width
var HeightWidthRatio = 0.9; // Height as a fixed ratio of width

// Determines how much above and below the maximum positive index value the y axis extends
var yAxisScalingFactorMax = 1.2;
var yAxisScalingFactorMin = 1.2;

var ele = document.getElementById(DivIdToScaleTo);
var eleStyle = window.getComputedStyle(ele);

var margin = {top: 20, right: 0, bottom: 30, left: 50}
;

var w = parseInt(eleStyle.width) - margin.left - margin.right
;
var h = (w * HeightWidthRatio) - margin.top - margin.bottom
;

var parseDate = d3.time.format("%Y-%m-%d %H:%M:%S").parse
;

var x = d3.time.scale()
.range([0, w])
;

var y = d3.scale.linear()
.range([h, 0])
;

var xAxis = d3.svg.axis()
.scale(x)
.orient("bottom")
.ticks(10)
;

var yAxis = d3.svg.axis()
.scale(y)
.orient("left")
.ticks(5)
;

// Define the lines used in the graph
// 15 mins
var lineIndex = d3.svg.line()
.x(function(d) { return x(d.DateTime); })
.y(function(d) { return y(d.Index); })
;

// Daily
var lineDaily = d3.svg.line()
.x(function(d) { return x(d.DateTime); })
.y(function(d) { return y(d.DailyAverage); })
;

// Weekly
var lineWeekly = d3.svg.line()
.x(function(d) { return x(d.DateTime); })
.y(function(d) { return y(d.WeeklyAverage); })
;

var svg = d3.select(".ChartDiv_TwitterMetrics").append("svg")
.attr("width", w) //width + margin.left + margin.right)
.attr("height", h) //height + margin.top + margin.bottom)
.attr("viewBox", "0 0 " + w + " " + h )
.attr("preserveAspectRatio", "xMidYMid meet")
.append("g")
.attr("transform", "translate(" + margin.left + "," + margin.top + ")")
;

// Import the data from external JSON file
d3.json("/json/TwitterSentimentLineChart.php", function(error, InputData) {

InputData.forEach(function(d) {
d.DateTime = parseDate(d.DateTime);
d.Index = +d.Index;
d.DailyAverage = +d.DailyAverage;
d.WeeklyAverage = +d.WeeklyAverage;
})
;

// Set the domains of the axis (set y to the largest positive index value)
x.domain(d3.extent(InputData, function(d) { return d.DateTime; }));
y.domain([
-yAxisScalingFactorMin * d3.max(InputData, function(d) { return d.Index; }),
yAxisScalingFactorMax * d3.max(InputData, function(d) { return d.Index; })
])
;

svg.append("g")
.attr("class", "x axis")
.attr("transform", "translate(0," + h * (yAxisScalingFactorMax/(yAxisScalingFactorMax + yAxisScalingFactorMin)) + ")") // h * (...) so that the axis is in line with zero
.call(xAxis)
;

svg.append("g")
.attr("class", "y axis")
.call(yAxis)
.append("text")
.attr("transform", "rotate(-90)")
.attr("y", 6)
.attr("dy", ".71em")
.style("text-anchor", "end")
.text("Twitter Sentiment Index")
;

svg.append("path")
.datum(InputData)
.attr("class", "lineIndex")
.attr("d", lineIndex)
;

svg.append("path")
.datum(InputData)
.attr("class", "lineDaily")
.attr("d", lineDaily)
;

svg.append("path")
.datum(InputData)
.attr("class", "lineWeekly")
.attr("d", lineWeekly)
;

// Chart Title
svg.append("text")
	.attr("x",w/2)
	.attr("y",h * 0.03)
	.attr("text-anchor","start")
	.style("font-size","0.8em")
	.text("Twitter Sentiment")
	;

// Legend
svg.append("text")
	.attr("x",w/2 - 40)
	.attr("y",h * 0.07)
	.attr("text-anchor","start")
	.style("font-size","0.6em")
        .style("fill", "#E52D11")
	.text("DailyAverage")
	;

svg.append("text")
	.attr("x",w/2 + 40)
	.attr("y",h * 0.07)
	.attr("text-anchor","start")
	.style("font-size","0.6em")
        .style("fill", "#14A8B6")
	.text("WeeklyAverage")
	;

});

// ### DOESN'T WORK ###
// From http://stackoverflow.com/questions/16265123/resize-svg-when-window-is-resized-in-d3-js

function updateWindow(){

ele = document.getElementById(DivIdToScaleTo);
eleStyle = window.getComputedStyle(ele);

w = parseInt(eleStyle.width) - margin.left - margin.right;
h = w * HeightWidthRatio - margin.top - margin.bottom ;

svg.attr("width", w).attr("height", h);
}

window.onresize = updateWindow;

[/d3-source]

### The Data

Getting the data out of Twitter uses the [Twitter API](https://dev.twitter.com/), accessed using the [python-twitter](https://code.google.com/p/python-twitter/https://code.google.com/p/python-twitter/) library. It's cobbled together using some MySQL databases, formatted into JSON files and displayed using the [d3.js](http://d3js.org/) graphical library (which takes some time and skill to get the most out of it but is certainly worth it).

The python scripts I've written runs automatically via a cron job on my [Raspberry Pi](http://www.raspberrypi.org/). The script scans Twitter every 15 minutes and the web-data is updated once a day.

### Sentiment Wordlists

The TwitterMetrics project uses a dictionary of positive and negative keywords developed by the American academics [Tim Loughran](http://www3.nd.edu/~tloughra) and [Bill McDonald](http://www3.nd.edu/~mcdonald/http://www3.nd.edu/~mcdonald/), which is in turn an extension and refinement of the [Harvard IV-4 Psychosocial Dictionary](http://www.wjh.harvard.edu/~inquirer/homecat.htmhttp://www.wjh.harvard.edu/~inquirer/homecat.htm). The list is extensive but doesn't include some terms I thought would be relevant (I don't think they encourage the use of 'lol' or certain expletives in acedemic literature) so I added to the list some 'Twitter-specific' terms of my own.

The approach isn't completely bullet-proof, it doesn't work well with sarcasm or some slang ("That new NeYo song is the bomb, yo!"* would probably be misinterpreted for example). It's good enough to make a fun infographic though.

Loughran & McDonald's wordlist is [available here](http://www3.nd.edu/~mcdonald/Word_Lists.htmlhttp://www3.nd.edu/~mcdonald/Word_Lists.html).

\*I've no idea whether or not this is something 'the kids' would actually say.

### The Twitter Sentiment Index

To generate the Twitter Sentiment Index, the number of positive words are counted and the number of negative words subtracted. The total is then divided by the total number of words in the returned tweets to measure the relative 'positive-ness' of the tweets returned.

[latex]SentimentIndex_{t} = \left ( \left ( \frac{PosWords_{t} - NegWords_{t}}{TotalWords_{t}}\right ) - AvgSentimentIndex \right ) * 10000[/latex]

The data is normalised by subtracting the average sentiment since the data gathering began, then multiplied by 10,000 (for no other reason than to remove the decimal places and make the numbers more readable).

The data is displayed with a daily and weekly [simple moving average](http://en.wikipedia.org/wiki/Moving_average) so that the change in sentiment can be visualised over time.

### The Code

You can download the Python code and an importable SQL file for the database from [GitHub](https://github.com/AdamDynamic/TwitterMetrics)
