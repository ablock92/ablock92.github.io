---
layout: post
title: "New York Times Headlines Analysis"
date: 2014-10-31 11:21:34 -0400
comments: true
published: false
categories: 
---

Many people read the New York Times or other newspapers on a daily basis to keep up to date on current political events. It's a great way to learn more about the world and gain a better appreciation for what is happening around us and I almost never take the time to do it. In fact, even skimming through the headlines is sometimes a bit of a chore. I imagine that many key political moments have occurred with me barely even noticing.

As an aspiring Data Scientist, the solution to this problem was fairly obvious: instead of trying to read back through the news and figure out all of the major political events that I have missed, I could make a computer look at the news and tell me everything that I need to know.

<!-- More -->

**Results**  
Ultimately, I defined 100 different topics based off the headlines and looked at keyword frequencies to try and determine what the topic was actually about. 

<div class="d3post">
</div>


 
**Data**  
I looked at the presidential terms of the last 5 United States Presidents (Obama, Bush, Clinton, Bush, Reagan). Since the New York Times API did not define political articles, I searched for articles that mentioned "President _(Current President)_" during each presidential term. In total, I gathered 167,425 articles.

<!-- 
Even more people only skim the headlines and gain a general idea of the issues and topics that are being most talked about. Some people don't read the New York Times at all! 

New York Times headlines are a ne



This project organically derives political topics from the headlines of New York Times articles and then tracks the appearance of those topics over time. 
 -->



<meta charset="utf-8">
<style>

body {
  font: 10px sans-serif;
}

.axis path,
.axis line {
  fill: none;
  stroke: #000;
  shape-rendering: crispEdges;
}

.x.axis path {
  display: none;
}

.line {
  fill: none;
  stroke: steelblue;
  stroke-width: 1.5px;
}

</style>
<body>
<script src="http://d3js.org/d3.v3.js"></script>
<script>

var margin = {top: 20, right: 20, bottom: 30, left: 50},
    width = 700 - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom;

var parseDate = d3.time.format("%d-%b-%y").parse;

var x = d3.time.scale()
    .range([0, width]);

var y = d3.scale.linear()
    .range([height, 0]);

var xAxis = d3.svg.axis()
    .scale(x)
    .orient("bottom");

var yAxis = d3.svg.axis()
    .scale(y)
    .orient("left");

var line = d3.svg.line()
    .x(function(d) { return x(d.date); })
    .y(function(d) { return y(d.close); });

var svg = d3.select("d3post").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

d3.csv("data/data.csv", function(error, data) {
  data.forEach(function(d) {
    d.date = parseDate(d.date);
    d.close = +d.close;
  });

  x.domain(d3.extent(data, function(d) { return d.date; }));
  y.domain(d3.extent(data, function(d) { return d.close; }));

  svg.append("g")
      .attr("class", "x axis")
      .attr("transform", "translate(0," + height + ")")
      .call(xAxis);

  svg.append("g")
      .attr("class", "y axis")
      .call(yAxis)
    .append("text")
      .attr("transform", "rotate(-90)")
      .attr("y", 6)
      .attr("dy", ".71em")
      .style("text-anchor", "end")
      .text("Price ($)");

  svg.append("path")
      .datum(data)
      .attr("class", "line")
      .attr("d", line);
});

</script>