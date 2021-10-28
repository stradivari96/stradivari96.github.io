---
title: 📈 Charting libraries for React
excerpt: Some charting libraries for React
read_time: true
share: true
toc: true
related: true
comments: true
header:
  teaser: "https://miro.medium.com/max/700/0*3gh9AjHufGCzP6aL.png"
tags: [Data Visualization, Programming, Libraries]
---

![img](https://miro.medium.com/max/700/0*3gh9AjHufGCzP6aL.png)

**Note**: Sorted by number of downloads. For lower level things use [visx](https://airbnb.io/visx/) or [D3](https://d3js.org/).

Also keep an eye on [vega](https://vega.github.io/vega/) + [react-vega](http://vega.github.io/react-vega/).

Click on the ![Example](https://img.shields.io/badge/example-codesandbox-brightgreen) badge to try them.
{: .notice--info}

## [Recharts](http://recharts.org/en-US/)

[![Downloads](https://img.shields.io/npm/dw/recharts)](https://www.npmjs.com/package/recharts)
[![GitHub stars](https://img.shields.io/github/stars/recharts/recharts.svg?style=social)](https://github.com/recharts/recharts)
[![GitHub last commit](https://img.shields.io/github/last-commit/recharts/recharts)](https://github.com/recharts/recharts)
[![Example](https://img.shields.io/badge/example-codesandbox-brightgreen)](https://codesandbox.io/embed/sharp-solomon-16mn0?)

<center>
<img src="https://miro.medium.com/max/1418/1*vUH8fvt-Kus9LzDd_3kcMw.png" alt="recharts" width="40%"/>
<img src="https://miro.medium.com/max/1374/1*4tlEoQUZ2IG1q2zAaGKxKQ.png" alt="recharts" width="50%">
</center>

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
[![Example](https://img.shields.io/badge/example-codesandbox-brightgreen)](https://codesandbox.io/embed/sleepy-mendeleev-43izk?codemirror=1)

<center>
<img src="https://i.imgur.com/i2Ixu1e.png" alt="chartjs" width="45%">
<img src="https://i.imgur.com/CxIKnuz.png" alt="chartjs" width="45%"/>
</center>

Wrapper for [Chart.js](https://www.chartjs.org/)

```javascript
<Line data={data} options={options} />
```

## [Nivo](https://nivo.rocks/)

[![Downloads](https://img.shields.io/npm/dw/@nivo/core)](https://www.npmjs.com/package/@nivo/core)
[![GitHub stars](https://img.shields.io/github/stars/plouc/nivo.svg?style=social)](https://github.com/plouc/nivo)
[![GitHub last commit](https://img.shields.io/github/last-commit/plouc/nivo)](https://github.com/plouc/nivo)
[![Example](https://img.shields.io/badge/example-codesandbox-brightgreen)](https://codesandbox.io/embed/nivo-basic-demo-forked-phtz8?codemirror=1)

<center>
<img src="https://i.imgur.com/YhPiFVc.png" alt="nivo" width="50%">
<img src="https://i.imgur.com/isL0svO.png" alt="nivo" width="40%"/>
</center>

```javascript
<ResponsiveLine data={data} curve="natural" useMesh={true} />
```

## [Victory](https://formidable.com/open-source/victory/)

[![Downloads](https://img.shields.io/npm/dw/victory)](https://www.npmjs.com/package/victory)
[![GitHub stars](https://img.shields.io/github/stars/FormidableLabs/victory.svg?style=social)](https://github.com/FormidableLabs/victory)
[![GitHub last commit](https://img.shields.io/github/last-commit/FormidableLabs/victory)](https://github.com/FormidableLabs/victory)
[![Example](https://img.shields.io/badge/example-codesandbox-brightgreen)](https://codesandbox.io/embed/crazy-roentgen-iou24?codemirror=1)

![victory](https://i.imgur.com/cUzrQjW.png)

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
[![Example](https://img.shields.io/badge/example-codesandbox-brightgreen)](https://codesandbox.io/embed/busy-tree-zpm5p?codemirror=1)

![react-vis](https://i.imgur.com/pob24MD.png)

```javascript
<XYPlot height={300} width={400}>
  <HorizontalGridLines />
  <XAxis title="X" />
  <YAxis />
  <LineMarkSeries data={data} curve="curveMonotoneX" />
</XYPlot>
```

## [echarts-for-react](https://git.hust.cc/echarts-for-react/)

[![Downloads](https://img.shields.io/npm/dw/echarts-for-react)](https://www.npmjs.com/package/echarts-for-react)
[![GitHub stars](https://img.shields.io/github/stars/hustcc/echarts-for-react.svg?style=social)](https://github.com/hustcc/echarts-for-react)
[![GitHub last commit](https://img.shields.io/github/last-commit/hustcc/echarts-for-react)](https://github.com/hustcc/echarts-for-react)

<center>
<img src="https://miro.medium.com/max/700/1*Jt7E1KKUPzRIuZXscBi2nw.png" alt="echarts" width="80%">
</center>

Wrapper for [echarts](https://echarts.apache.org/en/index.html).

{% raw %}

```javascript
<ReactEcharts
  option={{
    xAxis: {
      type: "category",
      data: months,
    },
    yAxis: {
      type: "value",
    },
    series: [
      {
        data: data,
        type: "line",
      },
    ],
  }}
/>
```

{% endraw %}

## [react-plotly.js](https://plotly.com/javascript/react/)

[![Downloads](https://img.shields.io/npm/dw/react-plotly.js)](https://www.npmjs.com/package/react-plotly.js)
[![GitHub stars](https://img.shields.io/github/stars/plotly/react-plotly.js.svg?style=social)](https://github.com/plotly/react-plotly.js)
[![GitHub last commit](https://img.shields.io/github/last-commit/plotly/react-plotly.js)](https://github.com/plotly/react-plotly.js)

<center>
<img src="https://i.imgur.com/oCvlxGp.png" alt="plotly" width="60%"/>
</center>

Wrapper for [plotly](https://plotly.com/)

{% raw %}

```javascript
<Plot
  data={[
    {
      x: [1, 2, 3],
      y: [2, 6, 3],
      type: "scatter",
      mode: "lines+markers",
      marker: { color: "red" },
    },
    { type: "bar", x: [1, 2, 3], y: [2, 5, 3] },
  ]}
  layout={{ width: 320, height: 240, title: "A Fancy Plot" }}
/>
```

{% endraw %}

## [BizCharts](https://bizcharts.net/products/bizCharts)

[![Downloads](https://img.shields.io/npm/dw/bizcharts)](https://www.npmjs.com/package/bizcharts)
[![GitHub stars](https://img.shields.io/github/stars/alibaba/BizCharts.svg?style=social)](https://github.com/alibaba/BizCharts)
[![GitHub last commit](https://img.shields.io/github/last-commit/alibaba/BizCharts)](https://github.com/alibaba/BizCharts)
[![Example](https://img.shields.io/badge/example-codesandbox-brightgreen)](https://codesandbox.io/embed/mutable-firefly-h2ybp?codemirror=1)

<center>
<img src="https://img.alicdn.com/tfs/TB1GPKEJHj1gK0jSZFuXXcrHpXa-1080-800.png" alt="bizcharts" width="40%">
<img src="https://img.alicdn.com/tfs/TB13QqIJRr0gK0jSZFnXXbRRXXa-1080-800.png" alt="bizcharts" width="40%"/>
</center>

```javascript
<Chart data={data}>
  <Line position="x*votes" shape="smooth" />
  <Point position="x*votes" />
  <Tooltip showCrosshairs lock />
</Chart>
```

Most of the documentation is in Chinese.
{: .notice--warning}
