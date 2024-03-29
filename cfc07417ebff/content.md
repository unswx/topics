%% Principles of data visualisation %%

# Australia's capital cities

A large proportion of Australians live in capital cities. How are they thus distributed geographically? The table below shows the location and population of each city. The locations are given by latitude and longitude. The populations are for the end of June 2020, as estimated by the Australian Bureau of Statistics:

```
City     Latitude Longitude Population
-------- -------- --------- ----------
Adelaide    -34.9     138.6  1,376,601
Brisbane    -27.5     153.0  2,560,720
Canberra    -35.3     149.1    431,380
Darwin      -12.4     130.8    147,231
Hobart      -42.9     147.3    238,834
Melbourne   -37.8     145.0  5,159,211
Perth       -32.0     115.9  2,125,114
Sydney      -33.9     151.2  5,367,206
```

These numbers give us some idea of how the capital city dwellers are distributed. We can see, for example, that quite a few more live in Sydney and Melbourne than in Darwin and Hobart. But we can make the distribution more vivid by visualising it.

# A visualisation

Here's one way to do it. It shows the population of each city, making their relative sizes more evident.

<div id="pops"></div>
<script>
  Highcharts.chart("pops", {
  	title: {text: "Population of Australia's Capital Cities on 30 June 2020"},
  	subtitle: {text: "Populations are estimates"},
  	caption: {text: "Source: Australian Bureau of Statistics"},
  	xAxis: {title: {text: "Capital city"}, type: "category",},
  	yAxis: {min: 0, title: {text: "Population"}, tickInterval: 1000000},
  	legend: {enabled: false},
  	series: [{
  	  type: 'column',
  	  dataLabels: {enabled: true},
  		data: [
        ["Adelaide", 1376601],
        ["Brisbane", 2560720],
        ["Canberra", 431380],
        ["Darwin", 147231],
        ["Hobart", 238834],
        ["Melbourne", 5159211],
        ["Perth", 2125114],
        ["Sydney", 5367206],
      ],
  	}]
  });
</script>

# Visualisation terminology

We'll be talking a lot about the features of a visualisation, so it's good to establish some terminology. Usage varies a bit, but the following is reasonably standard:

<div id="terminology"></div>
<script>
  Highcharts.chart("terminology", {
    title: {text: "Visualisation title"},
	  subtitle: {text: "Visualisation subtitle"},
	  caption: {text: "Visualisation caption"},
  	xAxis: {title: {text: "x-axis title/label"}, labels: {format: "Value label"}},
  	yAxis: {min: 0, title: {text: "y-axis title/label"}, tickInterval: 1000000, labels: {format: "Value label"}},
    legend: {title: {text: "Legend title", style: {"fontWeight": "normal"}}, itemStyle: {fontWeight: "normal"}},
  	series: [{
  	  type: 'column',
  	  name: "Series label",
  	  dataLabels: {enabled: true, format: "Data label", style: {"fontWeight": "normal"}},
  		data: [1376601, 2560720, 431380, 147231, 238834, 5159211, 2125114, 5367206],
  	}]
  });
</script>

Visualisations themselves are variously called "figures", "charts", "plots", or "graphs". People sometimes claim that figures, charts, plots and graphs are slightly different things, but the distinctions they draw can be hard to understand, and for most purposes you can consider them to be the same thing.

# How a visualisation works

The visualisation above follows some general principles:

1. Each row of the data set (each case) is mapped to a geometrical object. In this case, each row is mapped to a bar.

1. Some columns of the data set (some variables) are each mapped to a visual property of the geometrical objects. In this case, the city variable is mapped to the horizontal position of a bar, and the population variable is mapped to the vertical position of a bar (i.e. its top). These two variables are thus used to locate the bars.

1. The titles of the axes tell us which variables are mapped to the horizontal and vertical positions of the geometrical objects: the x-axis title tells us which variable is mapped to the horizontal position (in this case: city); the y-axis title tells us which variable is mapped to the vertical position (in this case: population).

1. The value labels on the axes tell us which data values are mapped to which horizontal and vertical positions. They allow us to reverse the mappings - to recover the data values from the positions. In the case above, the x-axis value labels allow us to recover the city of a bar from its horizontal position, and the y-axis value labels allow us to recover the population of a bar from its vertical position. Gridlines are added to help us judge the positions. Data labels have been added in this case, so we don't need to deduce the populations from the vertical positions - they're explicitly given to us. But that's not always the case.

1. An overall title and a caption are added, for additional information about the data. (This is sometimes called "metadata".)

These principles apply to every visualisation we make. Making a visualisation is largely a matter of choosing the mappings - what geometric objects to use, and which visual properties to map the data values to.

# A second visualisation

Here's another way to visualise the capital city data:

<div id="longs"></div>
<script>
  Highcharts.chart("longs", {
  	title: {text: "Population of Australia's Capital Cities on 30 June 2020"},
  	caption: {text: "Source: Australian Bureau of Statistics"},
  	xAxis: {title: {text: "Longitude"}, gridLineWidth: 1, tickInterval: 2},
  	yAxis: {min: 0, title: {text: "Population"}, tickInterval: 1000000},
  	legend: {enabled: false},
  	series: [{
  	  type: "line",
  	  pointWidth: 20,
  		data: [
        {name: "Perth", x: 115.9, y: 2125114},
        {name: "Darwin", x: 130.8, y: 147231},
        {name: "Adelaide", x: 138.6, y: 1376601},
        {name: "Melbourne", x: 145.0, y: 5159211},
        {name: "Hobart", x: 147.3, y: 238834},
        {name: "Canberra", x: 149.1, y: 431380},
        {name: "Sydney", x: 151.2, y: 5367206},
        {name: "Brisbane", x: 153.0, y: 2560720},
      ],
  	}]
  });
</script>

The same general principles are used to create this visualisation, but the particulars are a bit different:

- The geometrical objects ares lines - each row is mapped to a line, rather than to a bar.

- The longitude variable is mapped to the horizontal position of a line (more specifically, of its starting point).

- The population variable is mapped to the vertical position of a line (more specifically, of its starting point).

- The titles and labels of the x- and y-axes have been changed accordingly.

# A third visualisation

Here's a third way to visualise the capital city data:

<div id="latlonpop"></div>
<script>
  Highcharts.chart("latlonpop", {
    chart: {type: "bubble", height: 500},
  	title: {text: "Population of Australia's Capital Cities on 30 June 2020"},
  	caption: {text: "Source: Australian Bureau of Statistics"},
    xAxis: {min: 110, max: 160, title: {enabled: true, text: "Longitude"}, gridLineWidth: 1, tickInterval: 2},
    yAxis: {title: {text: "Latitude"}, tickInterval: 2},
    series: [{
      name: "",
      data: [
        {name: "Adelaide", y: -34.9, x: 138.6, z: 1376601},
        {name: "Brisbane", y: -27.5, x: 153.0, z: 2560720},
        {name: "Canberra", y: -35.3, x: 149.1, z: 431380},
        {name: "Darwin", y: -12.4, x: 130.8, z: 147231},
        {name: "Hobart", y: -42.9, x: 147.3, z: 238834},
        {name: "Melbourne", y: -37.8, x: 145.0, z: 5159211},
        {name: "Perth", y: -32.0, x: 115.9, z: 2125114},
        {name: "Sydney", y: -33.9, x: 151.2, z: 5367206},
      ],
      dataLabels: {enabled: false, format: "{point.name}<br>{point.z:,.0f}", useHTML: true, allowOverlap: true},
    }],
    legend: {title: {text: "Population", style: {"fontWeight": "normal"}}, symbolWidth: 0, align: "right", verticalAlign: "middle", layout: "vertical", bubbleLegend: {enabled: true, ranges: [{value: 2000000},{value: 4000000},{value: 6000000}]}},
  });
</script>

The same general principles have again been used, but the particulars are again a bit different:

- The geometrical objects are points - each row is mapped to a point, rather than to a bar or line.

- The longitude variable is mapped to the horizontal position of a point.

- The latitude variable is mapped to the vertical position of a point.

- The population variable is mapped to the size of a point (its area).

- The titles and labels of the x- and y-axes have been changed accordingly.

- A legend has been added, to show how the population variable is mapped to the size of a point. 

It would help to have a map of Australia superimposed in the background, but even this simple scatter plot is effective. It makes it clear that the vast majority of Australia's capital city dwellers are in the cities along the south coast of the mainland, especially in the east.

# Commonly used geometrical objects

A wide variety of geometrical objects are used to make visualisations, but you can achieve quite a lot with just a small subset of them. Here are some of the most commonly used geometrical objects, and their most commonly used visual properties:

- **Bars**: horizontal position, vertical position, width, colour. 

- **Lines**: horizontal position, vertical position, thickness, style (dots, dashes, etc.), colour. 

- **Points**: horizontal position, vertical position, size, colour. 

- **Wedges** (used in pie charts): radius, angle, colour. 

- **Boxes**: minimum position, Q1 position, median position, Q3 position, maximum position.

- **Patches of colour** (used in heat maps): horizontal position, vertical position, colour.

Every geometrical object has a location - a horizontal and vertical position in your visualisation. They all have a shape, size, and colour as well. If they involve lines, then those lines have widths and styles (patterns of dots and dashes). If they include text, then that text has a font family, a font face, and a font size.

Some visual properties can vary continuously, and thus represent continuous data. Horizontal position is like this, and so is the size of a point, and the colour of a bar. To be clear, these properties *can* vary continuously, but they need not. We might use a continuous spectrum to colour the bars in a visualisation, or we might use a discrete spectrum, perhaps with just a few colours. So these visual properties are very versatile - we can map both continuous and discrete variables to them.

Some visual properties cannot vary continuously. The style of a line (its pattern of dots and dashes) is like this. These visual properties are less versatile - we can map discrete variables to them, but we can't map continuous variables (unless we don't mind losing some information in the process).

# Visualisation as an art

Although there are general principles, there are no hard and fast rules about visualising data: for each principle, there are circumstances in which it's better not to follow it. This is true of many endeavours. It's true of writing, for example. There are general principles about writing, such as not starting a sentence with "But", which are generally good to follow. But not always.

With experience, you'll develop an eye for a well-designed visualisation, just as you might develop an eye for a well-composed photograph, or an ear for a well-written passage. It takes time to learn how to create *excellent* visualisations, but the general principles will help you to create *good* ones. So start with them, and let your skills develop from there.

# Check your understanding

Consider the following data about a business's sales during a certain year, and a visualisation of it:
```
Quarter  Target   Sales
------- ------- -------
Q1      $45,000 $40,200
Q2      $50,000 $48,350
Q3      $55,000 $56,100
Q4      $60,000 $65,840
```
<div id="sales"></div>
<script>
  Highcharts.chart("sales", {
  	title: {text: "Quarterly Sales"},
  	xAxis: {title: {text: "Quarter"}, type: "category",},
  	yAxis: {min: 0, title: {text: "Sales ($AU)"}},
  	legend: {enabled: false},
  	series: [{
  	  type: 'column',
  	  dataLabels: {enabled: true},
  		data: [["Q1",40200],["Q2",48350],["Q3",56100],["Q4",65840]],
  	}]
  });
</script>

[[[ Q1. Each row of data has been mapped to a geometrical object. What kind of object? >>>
A bar
]]]

[[[ Q2. Which variable has been mapped to the horizontal position of the object? >>>
Quarter
]]]

[[[ Q3. Which variable has been mapped to the vertical position of the object? >>>
Sales
]]]

[[[ Q4. What is the title of the visualisation? >>>
"Quarterly Sales"
]]]

[[[ Q5. What is the title of the vertical axis? >>>
"Sales ($AU)"
]]]

[[[ Q6. What are the numbers above the bars called? >>>
Value labels
]]]

[[[ Q7. What are the horizontal lines in the background called? >>>
Gridlines
]]]