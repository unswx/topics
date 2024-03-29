%% Visualising data with points %%

# Planet data

Here's some data about the eight planets in our solar system, compiled by the National Aeronautics and Space Administration (NASA). For each planet, it shows the diameter (in kilometres), the density (in kilograms per cubic metre), the acceleration due to gravity (in metres per second squared), the escape velocity (in kilometres per second), the number of moons, and whether the planet has a system of rings.

```
Planet   Diameter Density Gravity Velocity Moons Rings
-------- -------- ------- ------- -------- ----- -----
Mercury     4,879   5,429     3.7      4.3     0 No
Venus      12,104   5,243     8.9     10.4     0 No
Earth      12,756   5,514     9.8     11.2     1 No
Mars        6,792   3,934     3.7      5.0     2 No
Jupiter   142,984   1,326    23.1     59.5    79 Yes
Saturn    120,536     687     9.0     35.5    82 Yes
Uranus     51,118   1,270     8.7     21.3    27 Yes
Neptune    49,528   1,683    11.0     23.5    14 Yes
```

This data raises many interesting questions. Consider the number of moons, for example. Why do some planets have many moons, while others have very few, or none? It seems to be related to diameter - planets with larger diameters have more moons, while planets with smaller diameters have fewer moons. Rather than looking at the raw numbers, it's easier to check if we visualise the data.

# Visualising the data with points

A good way to see whether there's a relationship between diameter and number of moons is to visualise the data using points. Each row of data is mapped to a point. The diameter variable is mapped to the horizontal position of a point, and the moons variable is mapped to its vertical position. The result is what we call a **scatter plot**:

<div id="points"></div>
<script>
  let points = Highcharts.chart("points", {
    chart: {type: "scatter"},
    title: {text: "Planet Diameter and Number of Moons"},
    caption: {text: "Source: NASA"},
    xAxis: {min: 0, max: 150000, title: {enabled: true, text: "Diameter (km)"}, gridLineWidth: 1, tickInterval: 10000},
    yAxis: {min: 0, max: 100, title: {text: "Number of moons"}, tickInterval: 10},
    series: [{
      name: "Diameter",
      marker: {radius: 7.5},
      dataLabels: {enabled: true, format: "{point.name}"},
      data: [
        {name: "Mercury", x: 4879, y: 0},
        {name: "Venus", x: 12104, y: 0},
        {name: "Earth", x: 12756, y: 1},
        {name: "Mars", x: 6792, y: 2},
        {name: "Jupiter", x: 142984, y: 79},
        {name: "Saturn", x: 120536, y: 82},
        {name: "Uranus", x: 51118, y: 27},
        {name: "Neptune", x: 49528, y: 14}
      ],
    }],
    legend: {enabled: false},
  });
</script>

This scatter plot makes it easy to see that there's a relationship between diameter and number of moons. Scatter plots are often a good way to visualise the relationship (or lack thereof) between two variables.

# Adding a line of best fit

When visualising data using points, you'll often be looking for a *linear* relationship between the two variables you're mapping. To help see any such relationship, it's good to add a **line of best fit**. There are various ways of defining "best fit", but it's common to use the "least squares" definition: the best fitting line is the one that minimises the sum of the squares of the distances between the points and the line. Your software should be able to find this line for you, and add it to your visualisation. Here's what we get in our case:

<div id="withline"></div>
<script>
  let withline = Highcharts.chart("withline", {
    title: {text: "Planet Diameter and Number of Moons"},
    caption: {text: "Source: NASA"},
    xAxis: {min: 0, max: 150000, title: {enabled: true, text: "Diameter (km)"}, gridLineWidth: 1, tickInterval: 10000},
    yAxis: {min: 0, max: 100, title: {text: "Number of moons"}, tickInterval: 10},
    series: [{
      type: "scatter",
      marker: {radius: 7.5},
      dataLabels: {enabled: true, format: "{point.name}"},
      data: [
        {name: "Mercury", x: 4879, y: 0},
        {name: "Venus", x: 12104, y: 0},
        {name: "Earth", x: 12756, y: 1},
        {name: "Mars", x: 6792, y: 2},
        {name: "Jupiter", x: 142984, y: 79},
        {name: "Saturn", x: 120536, y: 82},
        {name: "Uranus", x: 51118, y: 27},
        {name: "Neptune", x: 49528, y: 14}
      ],
    },{
      type: "line",
      data: [[0,-6.4],[150000,89.5]],
      marker: {radius: 0},
    }],
    legend: {enabled: false},
  });
</script>

# Which variable for which position?

It doesn't matter which variable you map to the horizontal position of a point, and which you map to the vertical position. It doesn't affect any pattern there might be in the points - it just changes their overall orientation. Try it for yourself, to see what difference it makes to the visualisation above:

<label onclick="withline.update({chart: {inverted: false}})"><input type="radio" name="withline" checked/>Map diameter to the horizontal position</label>
<label onclick="withline.update({chart: {inverted: true}, xAxis: {reversed: false}})"><input type="radio" name="withline" />Map diameter to the vertical position</label>

That said, sometimes one mapping is more natural. If we suspect that one variable might be causing the other, for example, then it's often more natural to map the causing variable to the horizontal position, and the other variable to the vertical position. This is the case in the example above: if there's any causal relationship between the diameter of a planet and its number of moons, then it's the diameter that causally influences the number of moons, not the other way around. Because of this, you might find it more natural to map diameter to the horizontal position. Is that what you found?

This is not a hard-and-fast rule. Sometimes it's more natural to ignore direction of causation. Time variables, for example, are usually best mapped to the horizontal position, regardless of any causation there might be - we're used to thinking of time as flowing from left to right, rather than up or down. Similarly, altitude is usually best mapped to the vertical position, to match physical reality.

To illustrate this point about altitude, here's a scatter plot of the temperature of the Earth's atmosphere at various altitudes. (Actually, it's a line plot - the visualisation is more effective if we join the points with lines.) You can use the options to try both orientations. Do you agree that mapping altitude to the vertical position is more natural than mapping it to the horizontal position? This is despite any causation there might be, which would be altitude causally influencing temperature, rather than vice versa. 

<div id="temps"></div>
<script>
  let temps = Highcharts.chart("temps", {
    chart: {type: "line"},
    title: {text: "Earth's Atmospheric Temperature at Various Altitudes"},
    caption: {text: "Source: NASA"},
    xAxis: {title: {enabled: true, text: "Altitude"}, gridLineWidth: 1, tickInterval: 5, labels: {format: "{value}km"}},
    yAxis: {title: {text: "Temperatue"}, tickInterval: 10, labels: {format: "{value}C"}},
    series: [{
      name: "Diameter",
      //marker: {radius: 7.5},
      dataLabels: {enabled: true, format: "{point.name}"},
      data: [[0,15.0],[11,-56.5],[20,-56.5],[32,-44.5],[47,-2.5],[51,-2.5],[71,-58.5],[84,-86.0]]
    }],
    legend: {enabled: false},
  });
</script>
<label onclick="temps.update({chart: {inverted: false}})"><input type="radio" name="temps" checked/>Map altitude to the horizontal position</label>
<label onclick="temps.update({chart: {inverted: true}, xAxis: {reversed: false}})"><input type="radio" name="temps" />Map altitude to the vertical position</label>

# Dealing with overplotting

Sometimes you'll have points that are close enough together that they overlap. When this happens, we say that there's **overplotting**. Overplotting means that some of the data is not visible, which can be a problem.

There's not much overplotting in the visualisation of the moon data above, but here's a visualisation in which there is. It shows the finishing times of runners in the men's and women's marathon, at the 2020 Tokyo Olympics:

<div id="overplot"></div>
<script>
  let overplot = Highcharts.chart("overplot", {
    chart: {type: 'scatter', inverted: true},
    title: {text: "Finishing Times in the 2020 Tokyo Olympics Marathons"},
    legend: {enabled: false},
    xAxis: {categories: ["","Men","Women",""], title: {text: ''}, gridLineWidth: 1},
    yAxis: {min: 125, max: 185, title: {text: 'Finishing time (minutes)'}},
    series: [{
      marker: {radius: 7.5, fillColor: "rgb(0, 0, 255)"},
      data: [
        // Men
        [1,128.63],[1,129.97],[1,130],[1,130.03],[1,130.27],[1,130.68],[1,131.58],[1,131.68],[1,131.97],[1,132.22],[1,132.37],[1,132.83],[1,133.03],[1,133.37],[1,133.48],[1,134.03],[1,134.55],[1,134.8],[1,134.97],[1,135.18],[1,135.35],[1,135.57],[1,135.83],[1,135.85],[1,135.93],[1,136.13],[1,136.27],[1,136.28],[1,136.43],[1,136.55],[1,136.58],[1,136.65],[1,136.7],[1,136.72],[1,136.95],[1,137.07],[1,137.28],[1,137.32],[1,137.73],[1,137.98],[1,138.45],[1,138.47],[1,138.57],[1,138.65],[1,138.67],[1,139.45],[1,139.73],[1,139.95],[1,140.6],[1,140.72],[1,140.88],[1,141],[1,141.25],[1,141.48],[1,141.53],[1,141.53],[1,141.58],[1,141.75],[1,142.1],[1,142.2],[1,142.25],[1,142.38],[1,142.83],[1,143.2],[1,143.68],[1,144.07],[1,145.05],[1,145.62],[1,146.13],[1,146.98],[1,147.8],[1,148.72],[1,150.13],[1,151.85],[1,153.37],[1,164.6],
        // Women
        [2,147.33],[2,147.6],[2,147.77],[2,148.63],[2,149.1],[2,149.27],[2,149.6],[2,150.22],[2,150.98],[2,151.23],[2,151.37],[2,151.6],[2,151.68],[2,152.07],[2,152.17],[2,152.38],[2,152.88],[2,153.13],[2,153.23],[2,153.25],[2,153.3],[2,153.32],[2,153.65],[2,153.97],[2,154.15],[2,154.32],[2,154.35],[2,154.4],[2,154.63],[2,154.87],[2,155],[2,155.15],[2,155.47],[2,155.55],[2,155.55],[2,155.58],[2,155.65],[2,156.48],[2,156.55],[2,156.63],[2,156.73],[2,156.78],[2,157.02],[2,157.08],[2,157.13],[2,157.7],[2,157.75],[2,157.87],[2,158.05],[2,158.68],[2,159.42],[2,159.48],[2,159.53],[2,159.98],[2,160.07],[2,160.17],[2,161.18],[2,162.42],[2,162.43],[2,163.5],[2,164.15],[2,165.38],[2,165.45],[2,165.75],[2,167.25],[2,168.52],[2,169.35],[2,173.43],[2,173.67],[2,175.02],[2,175.65],[2,182.17],[2,183.17]
      ]
    }]
  });
</script>

There are two ways you might deal with overplotting. One way is to adjust the **opacity** of the points - to make the points slightly transparent. This keeps the overlaps, but allows us to see points that are behind other points, because they partly shine through. Another way is to add some **jitter** to the points - to slightly adjust their positions. This reduces the amount of overlap, by adding space between the points. You can use one of these techniques by itself, or both together. Try it for yourself:

<label onclick="overplot.update({series: {jitter: undefined, marker: {fillColor: 'rgb(0,0,255)'}}})"><input type="radio" name="overplot" checked/>No adjustments</label>
<label onclick="overplot.update({series: {jitter: undefined, marker: {fillColor: 'rgba(0,0,255,0.4)'}}})"><input type="radio" name="overplot"/>Using opacity</label>
<label onclick="overplot.update({series: {jitter: {x:0.2, y:0}, marker: {fillColor: 'rgb(0,0,255)'}}})"><input type="radio" name="overplot"/>Using jitter</label>
<label onclick="overplot.update({series: {jitter: {x:0.2, y:0}, marker: {fillColor: 'rgba(0,0,255,0.4)'}}})"><input type="radio" name="overplot"/>Using both</label>

# Mapping to colour

As well as horizontal and vertical positions, points have colour, and we can map a variable to this visual property too.

A common technique is to use colour to distinguish the points. The moon data contains a variable called "Rings", whose values are "Yes" and "No", according to whether a planet has a ring system or not. If we map this variable to the colour of a point, we can see which planets have rings and which don't. This allows us to see any relationship there might be between diameter, number of moons, and presence of rings:

<div id="col"></div>
<script>
  Highcharts.chart("col", {
    chart: {type: "scatter"},
    title: {text: "Planet Diameter, Number of Moons, and Rings"},
    caption: {text: "Source: NASA"},
    xAxis: {max: 150000, title: {enabled: true, text: "Diameter (km)"}, gridLineWidth: 1, tickInterval: 10000},
    yAxis: {max: 100, title: {text: "Number of moons"}, tickInterval: 10},
    plotOptions: {series: {marker: {symbol: "circle", radius: 7.5}, dataLabels: {enabled: true, format: "{point.name}"}}},
    series: [{
      name: "Rings",
      data: [
        {name: "Jupiter", x: 142984, y: 79},
        {name: "Saturn", x: 120536, y: 82},
        {name: "Uranus", x: 51118, y: 27},
        {name: "Neptune", x: 49528, y: 14}
      ],
    },{
      name: "No rings",
      data: [
        {name: "Mercury", x: 4879, y: 0},
        {name: "Venus", x: 12104, y: 0},
        {name: "Earth", x: 12756, y: 1},
        {name: "Mars", x: 6792, y: 2},
      ],
    }],
    legend: {enabled: true},
  });
</script>

Notice that the visualisation now has a legend. The legend shows the details of the third mapping - which variable is being mapped to colour, and which data values are being mapped to which colours.

# Mapping to size

Points also have size, and we can map a variable to this visual property too.

This gives us another way to visualise the relationship between diameter and number of moons in the planet data. Consider the following visualisation. The planet variable is mapped to the horizontal position of a point. The diameter variable is mapped to the size of a point (in particular, to its width). The moons variable is used to label the points. No variable is mapped to the vertical position of a point - all points are given the same vertical position.  

<div id="bubble"></div>
<script>
  let bubble = Highcharts.chart("bubble", {
    chart: {type: "bubble"},
    title: {text: "Planet Diameter and Number of Moons"},
    caption: {text: "Source: NASA"},
    xAxis: {type: "category", title: {enabled: true, text: ""}, gridLineWidth: 1, tickmarkPlacement: "on"},
    yAxis: {title: {text: ""}, labels: {enabled: false}},
    series: [{
      name: "Diameter",
      marker: {radius: 7.5},
      maxSize: 120,
      dataLabels: {enabled: true, format: "{point.moons}"},
      sizeBy: "width",
      data: [
        {name: "Mercury", y: 1, z: 4879, moons: 0},
        {name: "Venus", y: 1, z: 12104, moons: 0},
        {name: "Earth", y: 1, z: 12756, moons: 1},
        {name: "Mars", y: 1, z: 6792, moons: 2},
        {name: "Jupiter", y: 1, z: 142984, moons: 79},
        {name: "Saturn", y: 1, z: 120536, moons: 82},
        {name: "Uranus", y: 1, z: 51118, moons: 27},
        {name: "Neptune", y: 1, z: 49528, moons: 14}
      ],
    }],
    legend: {enabled: false},
  });
</script>

When the size of points varies in this way, the visualisation is sometimes called a **bubble plot**.

Be careful when mapping a variable to the size of a point. There are two ways to understand "size" - as width (radius or diameter), or as area. This means there are two ways to map the variable - map it to width, or map it to area. Depending on the variable, one of these is usually misleading. In the visualisation above, the diameter variable is mapped to the width of a point. Mapping it to the area of a point would be misleading, because it would distort the relative sizes of the planets. Try it for yourself:

<label onclick="bubble.update({series: {sizeBy: 'width'}})"><input type="radio" name="bubble" checked/>Map to width</label>
<label onclick="bubble.update({series: {sizeBy: undefined}})"><input type="radio" name="bubble"/>Map to area</label>

# Check your understanding

[[[Q1. Suppose you're using points to visualise the relationship between age and height for a group of people. Which variable would you map to the horizontal position of a point, and which would you map to the vertical position? >>>
If there's any causal relationship between these two variables (there might not be) then it would be age causally influencing height, rather than the other way around. So, it would be most natural to map age to the horizontal position, and height to the vertical position.]]]

[[[Q2. What is overplotting, and why can it be a problem? >>>
It's when some of the points overlap, which means that some of the data is hidden behind other data, which can lead to a misleading impression of the data.]]]

[[[Q3. What are the two main ways of dealing with overplotting? >>>
Using opacity (by making the points somewhat transparent), and using jitter (altering the positions of the points by small random amounts).]]]

[[[Q4. What is a bubble plot? >>>
It's a visualisation using points, in which we map a third variable to the size of a point, thus creating "bubbles".]]]

[[[Q5. What are the two ways to understand the "size" of a point? Why does it matter which size we use? >>>
On one understanding, the size of a point is its width (or radius). On another understanding, it's the area of the point. When mapping a variable to the size of a point it makes a visual difference which size we use. If we map it to width, the points will be larger than if we map it to area. 
]]]