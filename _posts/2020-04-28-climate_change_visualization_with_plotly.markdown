---
layout: post
title:      "Climate change visualization with Plotly"
date:       2020-04-28 20:41:49 +0000
permalink:  climate_change_visualization_with_plotly
---


A link for this blog on Analytics Vidhya:<br>
[https://medium.com/analytics-vidhya/climate-change-visualization-with-plotly-99905ae8d3bf?source=friends_link&sk=ae1a6d5a4c13c016fe7f317a262a25fd](http://)<br>

![](https://user-images.githubusercontent.com/23279623/80531797-2bf0e380-8969-11ea-903f-a18b3b59c128.png)

Climate change is one of the most pressing challenges facing humanity, and agriculture feels its effects in profound ways. Farmers are particularly impacted by extreme weather conditions, which include drought, severe heat, flooding, and other shifting climatic trends. I was curious to see the effect of climate change on crop production and decided to do my capstone project on this topic. For this blog, I will use FAO’s global crop production dataset and Plotly for visualization.<br>
Plotly Python library (plotly.py) is an interactive, open-source plotting library that supports over 40 unique chart types covering a wide range of statistical, financial, geographic, scientific, and 3-dimensional use-cases Built on top of the Plotly JavaScript library (plotly.js), plotly.py enables Python users to create beautiful interactive web-based visualizations that can be displayed in Jupyter notebooks, saved to standalone HTML files, or served as part of pure Python-built web applications using Dash.<br>
**Installation**<br>
plotly.py may be installed using pip
pip install plotly==4.6.0
conda install -c plotly plotly=4.6.0<br>

**Plotly Components**<br>

The major components in Plotly are trace, data, and layout.<br>

**Trace** is a dictionary of parameters of the data to be plotted, as well as information about the color and line types.<br>
**Data** consists of the list of plots in a chart where each plot is called a trace.<br>
**Layout** forms the look and feel of the plot. It includes background, grid, fonts, etc. All the components when fed to a Plotly function produces the necessary interactive figure.<br>
To start our visualization first we need to import Plotly.<br>
```
from plotly.offline import iplot
import plotly.graph_objects as go
```

Let’s see the relationship between the max crop yield and the maximum temperature anomaly on the same axis. It is similar to the sub-plot. We can do this easily by just setting the “secondary_y” subplot option to True.<br>

```
from plotly.subplots import make_subplots
# Create figure with secondary y-axis
fig = make_subplots(specs=[[{“secondary_y”: True}]])
fig.add_trace(go.Scatter(x=df_cropw_max[“Year”], y=df_cropw_max[“yield”], mode=”lines+markers”, name=”Max Wheat”),secondary_y=False,)
fig.add_trace( go.Scatter(x=df_cropr_max[“Year”], y=df_cropr_max[“yield”], mode=”lines+markers”, name=”Max Rice”), secondary_y=False,)
fig.add_trace(go.Scatter(x=df_cropm_max[“Year”], y=df_cropm_max[“yield”], mode=”lines+markers”, name=”Max Maize”),secondary_y=False,)
 
fig.add_trace(go.Scatter(x=df_temp_metro_max[“Year”], y=df_temp_metro_max[“temperature”], mode=”lines+markers”, name=”Max Temp”),secondary_y=True,)
fig.update_layout( title_text=”Max crop production & Maximum Temperature”)
fig.update_xaxes(title_text=”Year”)
fig.update_yaxes(title_text=”crop production per Hectare“, secondary_y=False)
fig.update_yaxes(title_text=”Temperature in celicious “, secondary_y=True)
fig.show()
```

![](https://user-images.githubusercontent.com/23279623/80532146-ade10c80-8969-11ea-8300-767c2bbf7be9.png)<br>
**Plotly Express:**<br>
Plotly Express is the easy-to-use, high-level interface to Plotly, which operates on “tidy” data. It is a wrapper for plotly.py that exposes a simple syntax for complex charts. With just a single import and one line code, you can make richly interactive plots such as maps and animations.<br>
**Choropleth map:**<br>
A choropleth map is an interactive geographic color heatmap for plotting data on top of a region of geography. Now will see some of the highly interactive plots<br>
One of the largest components of our changing climate is global warming which is the surface temperature increases from both natural and anthropogenic causes. Let us try to see how global warming is affecting different regions of the world through time, by using Plotly express and choropleth map with animation. Here is the data:<br>
![](https://user-images.githubusercontent.com/23279623/80532311-f13b7b00-8969-11ea-92b2-f5e5f5ebd8a8.png)
First, we need to import plotly express.<br>

```
import plotly.express as px
fig=px.choropleth(df_temp_metro,locations=”Area”,
locationmode=”countrynames”,animation_frame=”Year”,
animation_group=”Area”,color=”pos_temp”,
color_continuous_scale= ‘reds’ , hover_name=”Area”, 
title = ‘Global Temperature Anomaly’)
fig.show()
```

![](https://user-images.githubusercontent.com/23279623/80533019-01078f00-896b-11ea-8cb8-f7f55c250c65.png)

![](https://user-images.githubusercontent.com/23279623/80532883-c7368880-896a-11ea-8c85-0b78ab9bf171.png)
The plot clearly shows that the earth is warming.<br>
**Bar chart**<br>
Greenhouse gas emissions from human activity and livestock are a significant driver of climate change, trapping heat in the earth’s atmosphere, and triggering global warming. Let’s visualize the relationship between carbon dioxide emission and temperature anomaly through time.<br>

![](https://user-images.githubusercontent.com/23279623/80533201-4035e000-896b-11ea-8881-7ca0372b5eb0.png)

```
import plotly.express as px
fig = px.bar(df_temp_co2, x=’Year’, y=’co2',
hover_data=[‘co2’, ‘temperature’], color=’temperature’, 
title= ‘Global Co2 Emission & Temperature Anomaly’, 
labels={‘co2’:’World co2 emission’}, height=400)
fig.show()
```

![](https://user-images.githubusercontent.com/23279623/80533306-69567080-896b-11ea-82e2-306a00836114.png)

The plot clearly shows that when the carbon dioxide emission increases, the temperature increases.<br>
**Scatter plot with animation**<br>
What about the relationship between carbon dioxide emission, temperature anomaly, and crop yield through time? Here, we are trying to show 4 different variables on one plot. We split the plot just by using the “facet_col” option and choose the attribute/column we want to show. I wanted to see the effect of climate change on crop yield by area of the world, so I chose the region for my facet_col.<br>

```
fig = px.scatter(df_crop_temp_co2_reg, x=”temperature”,y=”co2", animation_frame=”Year”, animation_group=”Area”,size=”yield”, color=”region”, hover_name=”Area”, facet_col=”region”,size_max=45)
fig.show()
```

![](https://user-images.githubusercontent.com/23279623/80534121-bb4bc600-896c-11ea-95d4-3c596b5b09bc.png)
**Export Plotly charts to HTML**<br>
To export your visualization as an HTML file:
```
import plotly.io as pio
pio.write_html(fig, file='temp.html', auto_open=True)
```

**Uploading A Visualization to Plotly**<br>
To upload the visualization to your Plotly account, first, you need to install chart studio using<br>
```
!pip install chart_studio
```
and then import it using
```
import chart_studio
```
To connect to it from your notebook, you will need your username and API key from your Plotly account. Then set your credentials as follows:<br>

```
username = '' # your username
api_key = '' # your api key keychart_studio.tools.set_credentials_file(username=username, api_key=api_key)
```

Upload your visualization to your account using the following line of code:<br>
```
import chart_studio.plotly as py
py.plot(fig, filename = 'file name', auto_open=True)
```

Alright! We saw how to make very interactive visualizations using Plotly, export it to HTML, and upload it to a Plotly account.<br>
Thank you for reading!<br>
References :<br>
[https://towardsdatascience.com/how-to-create-a-plotly-visualization-and-embed-it-on-websites-517c1a78568b](http://)
[https://medium.com/plotly/introducing-plotly-express-808df010143d](http://)
