---
title: Labelled scatter plot with Bokeh
excerpt: "Simple labeled scatter plot in Bokeh and matplotlib"
read_time: true
share: true
toc: true
related: true
comments: true
header:
  teaser: "https://cdn-7.nikon-cdn.com/Images/Learn-Explore/Photography-Techniques/2012/Bokeh-for-Beginners/Media/Lindsay-Silverman-Bokeh-lights.jpg"
tags: [Python, Programming, Bokeh, Data Visualization]
---

![img](https://cdn-7.nikon-cdn.com/Images/Learn-Explore/Photography-Techniques/2012/Bokeh-for-Beginners/Media/Lindsay-Silverman-Bokeh-lights.jpg)

> [**Bokeh**](https://bokeh.pydata.org/en/latest/) is an interactive visualization library that targets modern web browsers for presentation. It can help anyone who would like to quickly and easily create **interactive** plots, **dashboards**, and data **applications**.

Its syntax is quite similar to matplotlib, so it won't be difficult to learn if you are already familiar with plotting in Python.

One thing that makes **bokeh** different from [**Matplotlib**](https://matplotlib.org/) and [**Seaborn**](https://seaborn.pydata.org/) is that it renders its graphics using HTML and JavaScript. So you will need a browser and internet (there is flag to generate html that works offline).

## Instalation

Bokeh and matplotlib are both available on PyPI:

```
pip install bokeh
pip install matplotlib
```

You may also need to install tkinter for matplotlib to work

```
sudo apt-get install python3-tk
```

## Scatter plot

### Data

```python
data_dict = {
    'x': [0, 1, 2, 3, 4],
    'y': [4, 3, 2, 1, 0],
    'L1': ['a', 'b', 'c', 'd', 'e'],  # aditional label
    'L 2': ['aa', 'bb', 'cc', 'dd', 'ee']  # another label
}
data_df = pd.DataFrame(data_dict)

x_label = "Label X axis"
y_label = "Label Y axis"
title = "Figure Title"
```

### Matplotlib

```python
import matplotlib.pyplot as plt

# data_df['x'] also works
plt.scatter(data_dict['x'], data_dict['y'])
plt.title(title)
plt.xlabel(x_label)
plt.ylabel(y_label)
plt.show()
```

Showing labels on hover in Matplotlib is possible but not easy. See [this stackoverflow question](https://stackoverflow.com/questions/7908636/possible-to-make-labels-appear-when-hovering-over-a-point-in-matplotlib) to know more.

{% include figure image_path="/assets/images/scatter_plt.png" %}

### Bokeh

```python
from bokeh.models import ColumnDataSource
from bokeh.plotting import figure, show

# data_df also works
source = ColumnDataSource(data_dict)

p = figure(
    tools="hover,pan,wheel_zoom,save",
    toolbar_location="above",
    title=title
)
p.xaxis.axis_label = x_label
p.yaxis.axis_label = y_label

# show aditional values on Hover
p.hover.tooltips = [("Label1", "@L1"), ("Label2", "@{L 2}")]

p.circle("x", "y", size=12, source=source)
show(p)
```

To include the labels we just need to make sure that `hover` is in the tools of the figure and add the `p.hover.tooltips` attribute.

{% include to-embed/scatter_hover.html %}

## Other interesting examples

### Range tool

[Source](https://bokeh.pydata.org/en/latest/docs/gallery/range_tool.html)
{% include to-embed/range_tool.html %}

### Map of texas

[Source](https://bokeh.pydata.org/en/latest/docs/gallery/texas.html)
{% include to-embed/texas.html %}

### Stacked bar

[Source](https://bokeh.pydata.org/en/latest/docs/gallery/bar_stacked.html)
{% include to-embed/bar_stacked.html %}

### Matrix

[Source](https://bokeh.pydata.org/en/latest/docs/gallery/les_mis.html)
{% include to-embed/les_mis.html %}
