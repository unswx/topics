%% Visualising data with boxes %%

# The women's marathon

There were 73 finishers in the women's marathon at the 2020 Tokyo Olympics. Here's a selection of their times, in minutes:

```
Pos Name                    Time
--- --------------------- ------
  1 Peres JEPCHIRCHIR     147.33
  2 Brigid KOSGEI         147.60
  3 Molly SEIDEL          147.77
  4 Roza DEREJE           148.63
  5 Volha MAZURONAK       149.10
...
 69 Juliet CHEKWEL        173.67
 70 Sara Catarina RIBEIRO 175.02
 71 Jess PIASECKI         175.65
 72 Sharon FIRISUA        182.17
 73 Dayna PIDHORESKY      183.17
```

How are their times distributed? Are the evenly distributed from first to last? Do they cluster in the middle? Do they cluster towards one of the ends? Are there two or more clusters?

# Their five number summary

One way to gauge the distribution of the times is to calculate their so-called "five number summary". This is a commonly used statistical description of a set of values. It includes: 

- The minimum value. This is the smallest (fastest) time, which is 147.33 minutes.

- The first quartile value (Q1). This is the time below which 25% of the times lie, which is the 19th time, which is 153.23 minutes.

- The median value. This is the time below which 50% of the times lie (i.e. the middle time), which is the 37th time, which is 155.65 minutes. 

- The third quartile value (Q3). This is the time below which 75% of the times lie, which is the 57th time, which is 160.07 minutes.

- The maximum value. This is the largest (slowest) time, which is 183.17 minutes.

We can display these numbers in the following table, which has one row and five columns:

```
Minimum     Q1 Median     Q3 Maximum
------- ------ ------ ------ -------
 147.33 153.23 155.65 160.07  183.17
```

# Visualising using a box

This table of numbers gives us some idea of the distribution of the times, but we can see it more vividly by visualising them using a **box**. When the x-axis starts at 0, the box is a bit hard to see - you can use the options below the visualisation to zoom in and out. 

<div id="chart"></div>
<script src="https://code.highcharts.com/highcharts-more.js"></script>
<script>
  let chart = Highcharts.chart("chart", {
    chart: {type: 'boxplot', inverted: true},
    title: {text: "Distribution of Times in the Tokyo 2020 Women's Marathon"},
    legend: {enabled: false},
    xAxis: {visible: false},
    yAxis: {min: 0, max: 200, title: {text: 'Finishing time (minutes)'}},
    series: [{
      data: [{low: 147.33, q1: 153.23, median: 155.65, q3: 160.07, high: 183.17}]
    }]
  });
</script>
<label onclick="chart.update({yAxis: {min: 0, max: 200}})"><input type="radio" name="chart" checked/>Unzoomed</label>
<label onclick="chart.update({yAxis: {min: 140, max: 190}})"><input type="radio" name="chart" />Zoomed</label>

This is called a **box plot** (or a "box and whisker plot"). The box has the following five visual properties: 

- Position of the left/bottom whisker
- Position of the left/bottom of the box
- Position of the median line
- Position of the right/top of the box
- Position of the right/top whisker

The left/bottom whisker extends to the lowest value, and the right/top whisker extends to the highest value. The box represents the middle 50% of rows, with the left/bottom end of the box at the first quartile (Q1), and the right/top end of the box at the third quartile (Q3). The line in the middle of the box represents the median.

Boxes are a good way to show the distribution of values of a variable in a compact way.

# Distinguishing outliers

Sometimes it's better to exclude the most extreme values when visualising the five number summary. These extreme values are called "outliers".

There's a formula that's often used to quantify what counts as an outlier.

First, we calculate the distance between Q1 and Q3, which is Q3 - Q1. This is called the **interquartile range**, often abbreviated to IQR. In this case, IQR is 160.07 - 153.23, which is 6.84.

Next, we define an outlier to be any value that is either more than 1.5 IQRs below Q1, or more than 1.5 IQRs above Q3. In this case, that means any value that is either below 142.97, or above 170.33.

There are no values below 142.97, but there are six values above 170.33. So there are six outliers:

```
173.43
173.67
175.02
175.65
182.17
183.17
```

We stop the whiskers of the box before they get to the outliers, and we show the outliers as separate points. Here's what we get (zoomed in):

<div id="outliers"></div>
<script src="https://code.highcharts.com/highcharts-more.js"></script>
<script>
  Highcharts.chart("outliers", {
    chart: {inverted: true},
    title: {text: "Distribution of Times in the Tokyo 2020 Women's Marathon"},
    legend: {enabled: false},
    xAxis: {visible: false},
    yAxis: {min: 120, max: 200, title: {text: 'Finishing time (minutes)'}},
    series: [{
      type: "boxplot",
      data: [{low: 147.33, q1: 153.23, median: 155.65, q3: 160.07, high: 169.35}]
    },{
      type: "scatter",
      data: [[0, 173.43], [0, 173.67], [0, 175.02], [0, 175.65], [0, 182.17], [0, 183.17]],
    }]
  });
</script>

# Using multiple boxes

Boxes are especially useful for showing multiple distibutions in one visualisation, where space can be at a premium.

There were 76 finishers in the men's marathon at the 2020 Tokyo Olympics. Here's a selection of their times (in minutes):

```
Pos Name                   Time
--- -------------------- ------
  1 Eliud KIPCHOGE       128.63
  2 Abdi NAGEEYE         129.97
  3 Bashir ABDI          130.00
  4 Lawrence CHERONO     130.03
  5 Ayad LAMDASSEM       130.27
...
 72 Cameron LEVINS       148.72
 73 Yuma HATTORI         150.13
 74 Jesus ARTURO ESPARZA 151.85
 75 Jorge CASTELBLANCO	 153.37
 76 Ivan ZARCO	         164.60
```

Here's their five number summary:

```
Minimum     Q1 Median     Q3 Maximum
------- ------ ------ ------ -------
 128.63 135.13 137.53 141.63  164.60
```

Here are the outliers:

```
151.85
153.37
164.60
```

And here's what we get when we visualise the women and men together. Notice that it shows clearly how the two distributions compare:

<div id="both"></div>
<script src="https://code.highcharts.com/highcharts-more.js"></script>
<script>
  Highcharts.chart("both", {
    chart: {inverted: true},
    title: {text: "Distribution of Times in the Tokyo 2020 Marathons"},
    legend: {enabled: false},
    xAxis: {categories: ["Men","Women"], title: {text: ""}},
    yAxis: {min: 120, max: 200, title: {text: "Finishing time (minutes)"}},
    series: [{
      type: "boxplot",
      data: [
        {low: 128.63, q1: 135.13, median: 137.53, q3: 141.63, high: 150.13},
        {low: 147.33, q1: 153.23, median: 155.65, q3: 160.07, high: 169.35}
      ]
    },{
      type: "scatter",
      data: [
        [0, 151.85], [0, 153.37], [0, 164.60],
        [1, 173.43], [1, 173.67], [1, 175.02], [1, 175.65], [1, 182.17], [1, 183.17]],
    }]
  });
</script>

# Check your understanding

[[[Q1. What does a box show us about a variable? >>>
How the values of the variable are distributed.]]]

[[[Q2. What are the five main visual properties of a box? >>>
1. The position of the left/bottom whisker
2. The position of the left/bottom of the box
3. The position of the median line
4. The position of the right/top of the box
5. The position of the right/top whisker
]]]

[[[Q3. Which data values are mapped to these five visual properties, when we're not showing outliers? >>>
1. The minimum value of the variable
2. The Q1 value of the variable
3. The median value of the variable
4. The Q3 value of the variable
5. The maximum value of the variable
]]]

[[[Q4. What about when we're showing outliers? >>>
1. The minimum value of the variable **that is not an outlier**
2. The Q1 value of the variable (same as above)
3. The median value of the variable (same as above)
4. The Q3 value of the variable (same as above)
5. The maximum value of the variable **that is not an outlier**
]]]