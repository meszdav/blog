{
  "title": "\ud83e\udd1f LLM-s for F1 Telemetry Data \ud83c\udfce\ufe0f",
  "author": {
    "name": "David Meszaros"
  },
  "date": "2025-04-29",
  "type": [
    "post",
    "posts"
  ]
}
Disclaimer:
> This is my first LLM project, where I wanted to learn and explore the basics of LLMs. My strategy to learn is to build > small projects which can showcase the features of the newest and hottest tools.

## Project Idea

The idea was fairly simple: I want to build a dashboard where the user can use natural language to get the telemetry data from two racers from a given event. The telemetry data is a time series data which is showing the speed, throttle, brake of the car. 
One of my most important criteria, that I want to use real data, and not some data generated or halucinated by the LLM. For this reason my strategy is to develop some custom tools which can be used to collect the publicly available telemetry data.

You can might ask the question, how is this better than just using some conventional dropdowns or radio buttons to filter the data. This is a good question, which I asked myself too 😀 Considering that the user uses natural language the definition of the prompt can be implicit like give me the `first` and the `fourth` best racer from a grand prix. The LLM can use some tools to find these drivers and then fetch the results. Or you can describe an event just by a country or city without specifying the event exactly. This gives the user much more flexibility and make the dashboard much more natural than just using dozens of buttons and dropdowns. But anyway the goal of the project to learn something new.

## Telemetry data

The first thing was to get telemetry data. For this I used the fastf1 python library, which makes it simple to download telemetry data. First thing is to load a session, where only the year, event name and the session type need to be specified.

After loading the session we can use all available data like lap times, results, telemetry data, etc.

The session object than can be used to retrieve telemetry data for specific drivers and laps.




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>RPM</th>
      <th>Speed</th>
      <th>nGear</th>
      <th>Throttle</th>
      <th>Brake</th>
      <th>DRS</th>
      <th>Source</th>
      <th>Time</th>
      <th>SessionTime</th>
      <th>Distance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2022-05-28 14:55:07.386</td>
      <td>11096.0</td>
      <td>279.0</td>
      <td>7</td>
      <td>100.0</td>
      <td>False</td>
      <td>12</td>
      <td>car</td>
      <td>0 days 00:00:00.085000</td>
      <td>0 days 01:10:06.992000</td>
      <td>6.587500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2022-05-28 14:55:07.586</td>
      <td>11190.0</td>
      <td>282.0</td>
      <td>7</td>
      <td>100.0</td>
      <td>False</td>
      <td>12</td>
      <td>car</td>
      <td>0 days 00:00:00.285000</td>
      <td>0 days 01:10:07.192000</td>
      <td>22.254167</td>
    </tr>
  </tbody>
</table>
</div>



This gives a pandas dataframe whith all the required information to plot the telemetry data.

## Colors

Another not that crucial but for me important detail was to be able to plot the data with the colors of the corresponding team color of the given driver. 
This information can be found with the fastf1 library too.


<div style="width: 50px; height: 50px; background-color: #0600ef;"></div>


Another detail is that the color in case of comparing a driver with another driver from the same team. In this case one of the drivers color will be inverted. This can be achived by the following code:



<div style="display: flex; align-items: center;">
    <div style="width: 50px; height: 50px; background-color: #0600ef;"></div>
    <div style="margin: 0 10px;">&#8594;</div>
    <div style="width: 50px; height: 50px; background-color: #F9FF10;"></div>
</div>



Putting everything together the telemetry will be plotted like this:


    
![svg](resources/output_14_0.svg)
    


The full code can be found the the github [repository](https://github.com/meszdav/f1-playground)

## Visualization

The first thing was to develop some functions to visualize the

![demo](resources/screen_recording_0.gif)
