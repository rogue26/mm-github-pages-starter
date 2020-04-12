---
title: "Waterfall charts in Tableau""
date: 2020-04-10T15:34:30-04:00
categories:
  - blog
tags:
  - Python
  - Tableau
---
![alt text][logo]

#Reshaping the Data

Once you've got the data loaded into tableau, you're going to need to reshape it. Tableau makes this easy, however it won't work for data loaded from _______

In data sources area, highlight columns with the waterfall elements
Select arrow in the upper right corner. Select Pivot.
Rename columns as "Waterfall Element" and "Value". It should look something like this.

If you're like me and prefer to do this kind of data manipulation in python, you can easily reshape the data using the melt feature in pandas.

'''python
id_vars = df.columns[:df.columns.get_loc("List Price")]
df = df.melt(id_vars=id_vars, var_name='Waterfall Element')
'''

#Add Some Calculated Columns

You're going to need to add four calculated columns:

the size of the bar (BarSize)
one for the keeping track of the cumulative amount on the waterfall (Final WF Value)
one for the order of the waterfall elements
one for the color (Color)

BarSize is easy. It's just -1 times the value.

Final WF Value is the "value" except for the calculated intermediate values. We don't want those included in the running total as we go across the waterfall. The list price is the starting point, so it doesn't get zeroed out.

The order field sets the order of the waterfall elements. You can technically make it work without this, but I think this way is easier.

The last calculated field is used to control the color of the bars. This one is optional, but it's a nice touch. We're simply setting the field to 1 if it's "List Price" or one of the calculated columns and 0 if it's not.

#Creating the plot

Make a new sheet. The first thing to do is set the mark type to "Gantt Bar."

The columns are going to be "Order" and "Waterfall Element". You'll need to drag "Order" up to the dimensions area first.

The "Rows" will be the "Final WF Value" calculated field. After you add it, click the down arrow --> Quick Table Calculation --> Running Total.

[bokeh_org]: https://bokeh.org
[logo]: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 2"