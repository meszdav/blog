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

```python
session = fastf1.get_session(2022, "Monaco Grand Prix", "Qualifying")
session.load()
```

After loading the session we can use all available data like lap times, results, telemetry data, etc. For this project I need first of all the telemetry data with some extra information like the distance and the color of the team

## Visualization

The first thing was to develop some functions to visualize the

![demo](assets/resources/screen_recording_0.gif)
