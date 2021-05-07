---
title: Charting libraries for React
excerpt: Some charting libraries for React
read_time: true
share: true
toc: true
related: true
comments: true
header:
  teaser: "https://miro.medium.com/max/700/0*3gh9AjHufGCzP6aL.png"
tags: [Data Visualization, Programming, Programming]
---

![img](https://miro.medium.com/max/700/0*3gh9AjHufGCzP6aL.png)

**Note**: Sorted by number of downloads. For lower level things use [visx](https://airbnb.io/visx/) or [D3](https://d3js.org/).

## [Recharts](http://recharts.org/en-US/)

[![Downloads](https://img.shields.io/npm/dw/recharts)](https://www.npmjs.com/package/recharts)
[![GitHub stars](https://img.shields.io/github/stars/recharts/recharts.svg?style=social)](https://github.com/recharts/recharts)
[![GitHub last commit](https://img.shields.io/github/last-commit/recharts/recharts)](https://github.com/recharts/recharts)

[Example](https://codesandbox.io/embed/sharp-solomon-16mn0?)

```javascript
<LineChart width={500} height={300} data={data}>
  <XAxis dataKey="name" />
  <YAxis />
  <CartesianGrid stroke="#eee" strokeDasharray="5 5" />
  <Line type="monotone" dataKey="uv" stroke="#8884d8" />
  <Line type="monotone" dataKey="pv" stroke="#82ca9d" />
</LineChart>
```

## [react-chartjs-2](https://reactchartjs.github.io/react-chartjs-2)

[![Downloads](https://img.shields.io/npm/dw/react-chartjs-2)](https://www.npmjs.com/package/react-chartjs-2)
[![GitHub stars](https://img.shields.io/github/stars/reactchartjs/react-chartjs-2.svg?style=social)](https://github.com/reactchartjs/react-chartjs-2)
[![GitHub last commit](https://img.shields.io/github/last-commit/reactchartjs/react-chartjs-2)](https://github.com/reactchartjs/react-chartjs-2)

[Example](https://codesandbox.io/embed/sleepy-mendeleev-43izk?codemirror=1)

```javascript
<Line data={data} options={options} />
```

## [Nivo](https://nivo.rocks/)

[![Downloads](https://img.shields.io/npm/dw/@nivo/core)](https://www.npmjs.com/package/@nivo/core)
[![GitHub stars](https://img.shields.io/github/stars/plouc/nivo.svg?style=social)](https://github.com/plouc/nivo)
[![GitHub last commit](https://img.shields.io/github/last-commit/plouc/nivo)](https://github.com/plouc/nivo)

[Example](https://codesandbox.io/embed/nivo-basic-demo-forked-phtz8?codemirror=1)

```javascript
<ResponsiveLine data={data} curve="natural" useMesh={true} />
```

## [Victory](https://formidable.com/open-source/victory/)

[![Downloads](https://img.shields.io/npm/dw/victory)](https://www.npmjs.com/package/victory)
[![GitHub stars](https://img.shields.io/github/stars/FormidableLabs/victory.svg?style=social)](https://github.com/FormidableLabs/victory)
[![GitHub last commit](https://img.shields.io/github/last-commit/FormidableLabs/victory)](https://github.com/FormidableLabs/victory)

[Example](https://codesandbox.io/embed/crazy-roentgen-iou24?codemirror=1)

```javascript
<VictoryChart>
  <VictoryAxis />
  <VictoryAxis dependentAxis />
  <VictoryLine data={data} interpolation="natural" y="votes" />
</VictoryChart>
```

## [react-vis](https://uber.github.io/react-vis/)

[![Downloads](https://img.shields.io/npm/dw/react-vis)](https://www.npmjs.com/package/react-vis)
[![GitHub stars](https://img.shields.io/github/stars/uber/react-vis.svg?style=social)](https://github.com/uber/react-vis)
[![GitHub last commit](https://img.shields.io/github/last-commit/uber/react-vis)](https://github.com/uber/react-vis)

[Example](https://codesandbox.io/embed/busy-tree-zpm5p?codemirror=1)

```javascript
<XYPlot height={300} width={400}>
  <HorizontalGridLines />
  <XAxis title="X" />
  <YAxis />
  <LineMarkSeries data={data} curve="curveMonotoneX" />
</XYPlot>
```

## [BizCharts](https://bizcharts.net/products/bizCharts)

[![Downloads](https://img.shields.io/npm/dw/bizcharts)](https://www.npmjs.com/package/bizcharts)
[![GitHub stars](https://img.shields.io/github/stars/alibaba/BizCharts.svg?style=social)](https://github.com/alibaba/BizCharts)
[![GitHub last commit](https://img.shields.io/github/last-commit/alibaba/BizCharts)](https://github.com/alibaba/BizCharts)

[Example](https://codesandbox.io/embed/mutable-firefly-h2ybp?codemirror=1)

```javascript
<Chart data={data}>
  <Line position="x*votes" shape="smooth" />
  <Point position="x*votes" />
  <Tooltip showCrosshairs lock />
</Chart>
```
