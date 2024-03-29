%% Filtering and grouping data %%

Sometimes we have questions about our data that are best answered if we **filter** the data, which means selecting some subset of the rows and ignoring the rest. Other times we have questions that are best answered by **grouping** the data, which means putting the rows into groups and then combining each group into a single row. Sometimes we have questions that are best answered by *both* filtering and grouping. We'll illustrate these techniques with some examples.

# The Tour de France

The Tour de France is one of the world's most gruelling cycling races, and is widely regarded as the most prestigious. It was first held in 1903, and has since been held every year, except during the two world wars. For the races held between 1999 and 2005 (inclusive) there is no recognised winner, because there was later discovered to have been widespread abuse of performance-enhancing drugs during those years.

Taking into account the wars and the drug abuse, between 1903 and 2022 the race has been won 102 times. Here is some data about those wins. It includes the year, the winning cyclist, the winning cyclist's country, the total distance cycled (in kilometres), and the number of stages that the winner won during the race. 

```
Year Cyclist             Country       Distance Stages
---- ------------------- -------       -------- ------
1903 Maurice Garin       France	           2428      3
1904 Henri Cornet        France	           2428      1
1905 Louis Trousselier   France	           2994      5
1906 René Pottier        France            4637      5
1907 Lucien Petit-Breton France	           4488      2
...
2018 Geraint Thomas      Great Britain     3349      2
2019 Egan Bernal         Colombia          3366      0
2020 Tadej Pogacar       Slovenia          3484      3
2021 Tadej Pogacar       Slovenia          3414      3
2022 Jonas Vingegaard    Denmark           3328      2
```

You might like to download the complete data set, and do some of the analyses described below.

[Download the data](0be1d0f6daf3.csv)

# Without filtering or grouping

There are various questions about this data that we can answer without doing any filtering or grouping. We just need to inspect the data, perhaps after sorting it, and perhaps after calculating a summary statistic for a column. For example:

**How many races have had winners?** We can answer this by getting the count of the rows.

**How many distinct winners have there been?** We can answer this by getting the count of the distinct values of the cyclist variable.

**How many distinct countries have had winners?** We can answer this by getting the count of the distinct values of the country variable.

**How long have the shortest and longest races been?** We can answer this by getting the minimum and maximum values of the distance variable (which we could do by sorting).

**Who won the longest race?** We can answer this by sorting the longest race to the top, and inspecting the name of the winning cyclist.

**How many times did Miguel Indurain win?** We can answer this by sorting the rows by cyclist, finding the ones for Miguel Indurain, and counting them manually.

# With filtering

Some questions are best answered if we filter the data first. For example:

**How many times has a French cyclist won?** We *could* do this without filtering, by sorting the rows by country, finding the French rows, and counting them manually. But if there are many of them (and there are), then this is laborious. It's better to filter the rows to just those where the country is France, and then get the count of the rows.

**What's the mean length of the races won by French cyclists?** This question is even harder to answer without filtering. You would need to sort the rows by country, find the French rows, count them manually, sum the distances manually, and calculate the mean distance. It's much better to filter the rows to just those where the country is France, and then get the mean of the distance column.

# With grouping

Some questions are difficult to answer with or without filtering, and are best answered by grouping the data. For example:

**Which cyclist (or cyclists) have won the most often?** We *could* answer this without grouping. We could sort the rows by cyclist, manually count the number of wins for each, then see who's won the most often. Or, we could filter the rows by cyclist, one-by-one, get a total row count for each cyclist, and then see which one has the most rows. But both of these methods would be slow and tedious (and error prone). It's better to group the rows by cyclist, and get the count of the rows in each group. Here's what we get, sorted by decreasing count:

```
Cyclist          Count
---------------- -----
Jacques Anquetil     5
Eddy Merckx          5
Bernard Hinault      5
Miguel Indurain      5
Chris Froome         4
Philippe Thys        3
Louison Bobet        3
Greg LeMond          3
etc.
```

**Which cyclist (or cyclists) have had the most stage wins during winning races?** This question is similar to the previous, and is also best answered by grouping. We could group the rows by cyclist, get the sum of the stages variable for each group, and sort them by decreasing sum. Here's what we get:

```
Cyclist                Sum
---------------- ---------
Eddy Merckx             32
Bernard Hinault         21
Jacques Anquetil        16
Miguel Indurain         10
Gino Bartali             9
André Leducq             8
Fausto Coppi             8
Nicolas Frantz           8
Ottavio Bottecchia       8
etc.
```

# With both filtering and grouping

Some questions are best answered using a combination of filtering and grouping. For example:

**Among the wins in which the winner had two or more stage wins, who had the most wins?** To answer this question, we need to first filter the rows to just those in which the winner had two or more stage wins. Then we need to group by the cyclist variable. Then, for each group, get the count of the rows. Then sort the results by count.

Perhaps you might try this?