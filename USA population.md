# U.S. state capitals overlayed on a map of the U.S

This is a layered geographic visualization that shows US capitals overlayed on a map.




```python
import altair as alt
from vega_datasets import data

states = alt.topo_feature(data.us_10m.url, 'states')
capitals = data.us_state_capitals.url

# US states background
background = alt.Chart(states).mark_geoshape(
    fill='lightgray',
    stroke='white'
).properties(
    title='US State Capitols',
    width=650,
    height=400
).project('albersUsa')

# Points and text
hover = alt.selection(type='single', on='mouseover', nearest=True,
                      fields=['lat', 'lon'])

base = alt.Chart(capitals).encode(
    longitude='lon:Q',
    latitude='lat:Q',
)

text = base.mark_text(dy=-5, align='right').encode(
    alt.Text('city', type='nominal'),
    opacity=alt.condition(~hover, alt.value(0), alt.value(1))
)

points = base.mark_point().encode(
    color=alt.value('black'),
    size=alt.condition(~hover, alt.value(30), alt.value(100))
).add_selection(hover)

background + points + text
```





<div id="altair-viz-cd552a5ce4b44782ab89592f5111726a"></div>
<script type="text/javascript">
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-cd552a5ce4b44782ab89592f5111726a") {
      outputDiv = document.getElementById("altair-viz-cd552a5ce4b44782ab89592f5111726a");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.8.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function loadScript(lib) {
      return new Promise(function(resolve, reject) {
        var s = document.createElement('script');
        s.src = paths[lib];
        s.async = true;
        s.onload = () => resolve(paths[lib]);
        s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
        document.getElementsByTagName("head")[0].appendChild(s);
      });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else if (typeof vegaEmbed === "function") {
      displayChart(vegaEmbed);
    } else {
      loadScript("vega")
        .then(() => loadScript("vega-lite"))
        .then(() => loadScript("vega-embed"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300}}, "layer": [{"data": {"url": "https://cdn.jsdelivr.net/npm/vega-datasets@v1.29.0/data/us-10m.json", "format": {"feature": "states", "type": "topojson"}}, "mark": {"type": "geoshape", "fill": "lightgray", "stroke": "white"}, "height": 400, "projection": {"type": "albersUsa"}, "title": "US State Capitols", "width": 650}, {"data": {"url": "https://cdn.jsdelivr.net/npm/vega-datasets@v1.29.0/data/us-state-capitals.json"}, "mark": "point", "encoding": {"color": {"value": "black"}, "latitude": {"field": "lat", "type": "quantitative"}, "longitude": {"field": "lon", "type": "quantitative"}, "size": {"condition": {"value": 30, "selection": {"not": "selector004"}}, "value": 100}}, "selection": {"selector004": {"type": "single", "on": "mouseover", "nearest": true, "fields": ["lat", "lon"]}}}, {"data": {"url": "https://cdn.jsdelivr.net/npm/vega-datasets@v1.29.0/data/us-state-capitals.json"}, "mark": {"type": "text", "align": "right", "dy": -5}, "encoding": {"latitude": {"field": "lat", "type": "quantitative"}, "longitude": {"field": "lon", "type": "quantitative"}, "opacity": {"condition": {"value": 0, "selection": {"not": "selector004"}}, "value": 1}, "text": {"type": "nominal", "field": "city"}}}], "$schema": "https://vega.github.io/schema/vega-lite/v4.8.1.json"}, {"mode": "vega-lite"});
</script>



# US Population Over Time
This chart visualizes the age distribution of the US population over time. 
It uses a slider widget that is
bound to the year to visualize the age distribution over time.


```python

source = data.population.url

pink_blue = alt.Scale(domain=('Male', 'Female'),
                      range=["steelblue", "salmon"])

slider = alt.binding_range(min=1900, max=2000, step=10)
select_year = alt.selection_single(name="year", fields=['year'],
                                   bind=slider, init={'year': 2000})

alt.Chart(source).mark_bar().encode(
    x=alt.X('sex:N', title=None),
    y=alt.Y('people:Q', scale=alt.Scale(domain=(0, 12000000))),
    color=alt.Color('sex:N', scale=pink_blue),
    column='age:O'
).properties(
    width=20
).add_selection(
    select_year
).transform_calculate(
    "sex", alt.expr.if_(alt.datum.sex == 1, "Male", "Female")
).transform_filter(
    select_year
).configure_facet(
    spacing=8
)
```





<div id="altair-viz-0915138d4f7c4543989f1e7e281cb859"></div>
<script type="text/javascript">
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-0915138d4f7c4543989f1e7e281cb859") {
      outputDiv = document.getElementById("altair-viz-0915138d4f7c4543989f1e7e281cb859");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.8.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function loadScript(lib) {
      return new Promise(function(resolve, reject) {
        var s = document.createElement('script');
        s.src = paths[lib];
        s.async = true;
        s.onload = () => resolve(paths[lib]);
        s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
        document.getElementsByTagName("head")[0].appendChild(s);
      });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else if (typeof vegaEmbed === "function") {
      displayChart(vegaEmbed);
    } else {
      loadScript("vega")
        .then(() => loadScript("vega-lite"))
        .then(() => loadScript("vega-embed"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300}, "facet": {"spacing": 8}}, "data": {"url": "https://cdn.jsdelivr.net/npm/vega-datasets@v1.29.0/data/population.json"}, "mark": "bar", "encoding": {"color": {"type": "nominal", "field": "sex", "scale": {"domain": ["Male", "Female"], "range": ["steelblue", "salmon"]}}, "column": {"type": "ordinal", "field": "age"}, "x": {"type": "nominal", "field": "sex", "title": null}, "y": {"type": "quantitative", "field": "people", "scale": {"domain": [0, 12000000]}}}, "selection": {"year": {"type": "single", "fields": ["year"], "bind": {"input": "range", "max": 2000, "min": 1900, "step": 10}, "init": {"year": 2000}}}, "transform": [{"calculate": "if((datum.sex === 1),'Male','Female')", "as": "sex"}, {"filter": {"selection": "year"}}], "width": 20, "$schema": "https://vega.github.io/schema/vega-lite/v4.8.1.json"}, {"mode": "vega-lite"});
</script>



# US Population Pyramid Over Time
A population pyramid shows the distribution of age groups within a population. It uses a slider widget that is 
bound to the year to visualize the age distribution over time.


```python


slider = alt.binding_range(min=1850, max=2000, step=10)
select_year = alt.selection_single(name='year', fields=['year'],
                                   bind=slider, init={'year': 2000})

base = alt.Chart(source).add_selection(
    select_year
).transform_filter(
    select_year
).transform_calculate(
    gender=alt.expr.if_(alt.datum.sex == 1, 'Male', 'Female')
).properties(
    width=250
)


color_scale = alt.Scale(domain=['Male', 'Female'],
                        range=['#1f77b4', '#e377c2'])

left = base.transform_filter(
    alt.datum.gender == 'Female'
).encode(
    y=alt.Y('age:O', axis=None),
    x=alt.X('sum(people):Q',
            title='population',
            sort=alt.SortOrder('descending')),
    color=alt.Color('gender:N', scale=color_scale, legend=None)
).mark_bar().properties(title='Female')

middle = base.encode(
    y=alt.Y('age:O', axis=None),
    text=alt.Text('age:Q'),
).mark_text().properties(width=20)

right = base.transform_filter(
    alt.datum.gender == 'Male'
).encode(
    y=alt.Y('age:O', axis=None),
    x=alt.X('sum(people):Q', title='population'),
    color=alt.Color('gender:N', scale=color_scale, legend=None)
).mark_bar().properties(title='Male')

alt.concat(left, middle, right, spacing=5)

```





<div id="altair-viz-8795ee90a6594f6aa14bf6f8fff1d909"></div>
<script type="text/javascript">
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-8795ee90a6594f6aa14bf6f8fff1d909") {
      outputDiv = document.getElementById("altair-viz-8795ee90a6594f6aa14bf6f8fff1d909");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.8.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function loadScript(lib) {
      return new Promise(function(resolve, reject) {
        var s = document.createElement('script');
        s.src = paths[lib];
        s.async = true;
        s.onload = () => resolve(paths[lib]);
        s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
        document.getElementsByTagName("head")[0].appendChild(s);
      });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else if (typeof vegaEmbed === "function") {
      displayChart(vegaEmbed);
    } else {
      loadScript("vega")
        .then(() => loadScript("vega-lite"))
        .then(() => loadScript("vega-embed"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300}}, "concat": [{"mark": "bar", "encoding": {"color": {"type": "nominal", "field": "gender", "legend": null, "scale": {"domain": ["Male", "Female"], "range": ["#1f77b4", "#e377c2"]}}, "x": {"type": "quantitative", "aggregate": "sum", "field": "people", "sort": "descending", "title": "population"}, "y": {"type": "ordinal", "axis": null, "field": "age"}}, "selection": {"year": {"type": "single", "fields": ["year"], "bind": {"input": "range", "max": 2000, "min": 1850, "step": 10}, "init": {"year": 2000}}}, "title": "Female", "transform": [{"filter": {"selection": "year"}}, {"calculate": "if((datum.sex === 1),'Male','Female')", "as": "gender"}, {"filter": "(datum.gender === 'Female')"}], "width": 250}, {"mark": "text", "encoding": {"text": {"type": "quantitative", "field": "age"}, "y": {"type": "ordinal", "axis": null, "field": "age"}}, "selection": {"year": {"type": "single", "fields": ["year"], "bind": {"input": "range", "max": 2000, "min": 1850, "step": 10}, "init": {"year": 2000}}}, "transform": [{"filter": {"selection": "year"}}, {"calculate": "if((datum.sex === 1),'Male','Female')", "as": "gender"}], "width": 20}, {"mark": "bar", "encoding": {"color": {"type": "nominal", "field": "gender", "legend": null, "scale": {"domain": ["Male", "Female"], "range": ["#1f77b4", "#e377c2"]}}, "x": {"type": "quantitative", "aggregate": "sum", "field": "people", "title": "population"}, "y": {"type": "ordinal", "axis": null, "field": "age"}}, "selection": {"year": {"type": "single", "fields": ["year"], "bind": {"input": "range", "max": 2000, "min": 1850, "step": 10}, "init": {"year": 2000}}}, "title": "Male", "transform": [{"filter": {"selection": "year"}}, {"calculate": "if((datum.sex === 1),'Male','Female')", "as": "gender"}, {"filter": "(datum.gender === 'Male')"}], "width": 250}], "data": {"url": "https://cdn.jsdelivr.net/npm/vega-datasets@v1.29.0/data/population.json"}, "spacing": 5, "$schema": "https://vega.github.io/schema/vega-lite/v4.8.1.json"}, {"mode": "vega-lite"});
</script>



# US Population: Wrapped Facet
This chart visualizes the age distribution of the 
US population over time, using a wrapped faceting of the data by decade.




```python


alt.Chart(source).mark_area().encode(
    x='age:O',
    y=alt.Y(
        'sum(people):Q',
        title='Population',
        axis=alt.Axis(format='~s')
    ),
    facet=alt.Facet('year:O', columns=5),
).properties(
    title='US Age Distribution By Year',
    width=90,
    height=80
)
```





<div id="altair-viz-18fa05404b0647439113eaa3b237b5b7"></div>
<script type="text/javascript">
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-18fa05404b0647439113eaa3b237b5b7") {
      outputDiv = document.getElementById("altair-viz-18fa05404b0647439113eaa3b237b5b7");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.8.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function loadScript(lib) {
      return new Promise(function(resolve, reject) {
        var s = document.createElement('script');
        s.src = paths[lib];
        s.async = true;
        s.onload = () => resolve(paths[lib]);
        s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
        document.getElementsByTagName("head")[0].appendChild(s);
      });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else if (typeof vegaEmbed === "function") {
      displayChart(vegaEmbed);
    } else {
      loadScript("vega")
        .then(() => loadScript("vega-lite"))
        .then(() => loadScript("vega-embed"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300}}, "data": {"url": "https://cdn.jsdelivr.net/npm/vega-datasets@v1.29.0/data/population.json"}, "mark": "area", "encoding": {"facet": {"type": "ordinal", "columns": 5, "field": "year"}, "x": {"type": "ordinal", "field": "age"}, "y": {"type": "quantitative", "aggregate": "sum", "axis": {"format": "~s"}, "field": "people", "title": "Population"}}, "height": 80, "title": "US Age Distribution By Year", "width": 90, "$schema": "https://vega.github.io/schema/vega-lite/v4.8.1.json"}, {"mode": "vega-lite"});
</script>


