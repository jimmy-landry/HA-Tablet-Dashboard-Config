# The Original Tamper Evident HA Tablet Dashboard

<img width="1240" alt="Screenshot of the tablet dashboard." src="https://github.com/jimmylandry54/HA-Tablet-Dashboard-Config/assets/121106900/462097c4-b3cf-4529-90f8-1bcfb4f361d4">

![GitHub Repo stars](https://img.shields.io/github/stars/jimmy-landry/ha-tablet-dashboard-config)
![YouTube Video Views](https://img.shields.io/youtube/views/d9ewXJsxKrI)
[![YouTube](https://img.shields.io/badge/YouTube-Tamper%20Evident-red?logo=youtube)](https://www.youtube.com/@dontuseiftamperevident)

## ⬙ BEFORE YOU START ⬙

### Check Custom Components
Ensure you have **all** of the following custom components installed. Otherwise, things may not work properly. All can be installed via HACS. <br>
 - [Layout Card](https://github.com/thomasloven/lovelace-layout-card)
 - [Button Card](https://github.com/custom-cards/button-card)
 - [Vertical Stack In Card](https://github.com/ofekashery/vertical-stack-in-card)
 - [Mushroom Cards (used under the clock in the sidebar) ](https://github.com/piitaya/lovelace-mushroom)
 - [Mini Media Player](https://github.com/kalkih/mini-media-player)
 - [Simple Weather Card](https://github.com/kalkih/simple-weather-card)
 - [Card Mod](https://github.com/thomasloven/lovelace-card-mod)
> [!IMPORTANT]
> The layout card component is **CRUCIAL** for the dashboard to function properly. If not installed, nothing will load and you'll get a nice, large error box like the one below that shows the _entire_ config file that makes the dashboard work. No one wants that, so please be sure you have this installed. I got a lot of questions about this so I figured I should make it extra clear.

### Configure Date & Time Sensors (optional)

If you would like to have the time displayed in 12 hour format and the names of the weekdays and months shown instead of the numbers, you'll need to configure a few custom sensors to provide this data as Home Assistant doesn't allow you to do this natively. To do this, go to Settings > Devices and Services > Helpers > Create Helper > Template and choose "Template a sensor."
In the dialog box, enter a name, choose an icon if you want, and in the value template field, paste in the code from one of the below dropdowns.
<br>

Depending on what you want to do, you may not need to set up all the sensors. 

<details>
  <summary>Change the 24 hour time to 12 hour format</summary>

    {% set time = states('sensor.time') %}
    {% set hour = time[:2]|int %}
    {% set timenew = '{:2d}:{}'.format(hour if hour == 12 else hour % 12, time[3:5]) %}
    {% set hournew = timenew[:2]|int %}
    {% set hournew2 = (hournew + 12 if hournew == 0 else hournew) %}
    {{ '{:2d}:{} {}'.format(hournew2 if hournew2 == 12 else hournew % 12, timenew[3:5], 'PM' if hour>11 else 'AM') }}
</details>
<br>
<details>
  <summary>Display the day of the week</summary>

    {% set weekday = now().weekday() %} 
    {% if (weekday == 0) %} Monday 
    {% elif (weekday == 1) %} Tuesday 
    {% elif (weekday == 2) %} Wednesday
    {% elif (weekday == 3) %} Thursday 
    {% elif (weekday == 4) %} Friday
    {% elif (weekday == 5) %} Saturday 
    {% elif (weekday == 6) %} Sunday
    {% endif %}
</details>
<br>
<details>
  <summary>Display the month of the year</summary>

    {% set month = now().month %} 
    {% if (month == 1) %} January 
    {% elif (month == 2) %} February 
    {% elif (month == 3) %} March 
    {% elif (month == 4) %} April  
    {% elif (month == 5) %} May
    {% elif (month == 6) %} June 
    {% elif (month == 7) %} July 
    {% elif (month == 8) %} August 
    {% elif (month == 9) %} September 
    {% elif (month == 10) %} October 
    {% elif (month == 11) %} November 
    {% elif (month == 12) %} December 
    {% endif %}
</details>
<br>

Take note of the names you choose for the sensors as you'll need them once you create the dashboard.

## ⬙ SETUP ⬙
### 1. Set Up the Theme
- In this repository, find the "theme.yaml" file. 
- Open the file and choose Copy raw file in the top right.
- On your Home Assistant server, find the "themes" folder using File editor or Visual Studio Code.
    - If you don't have this folder: 
        - Create it. It must be named "themes" exactly, otherwise, again, it won't work.
        - Add the following snippet of code to your configuration.yaml file:
            ```
             frontend:
               themes: !include_dir_merge_named themes
            ```
        - Restart Home Assistant.
- Create a new file inside of the themes folder and name it "Wall Tablet Theme.yaml"
- In the new file, paste in the entire theme file copied from this repository. There should be 207 lines total. Save the file and exit.
- Go to Developer Tools, actions, then search for the "Reload themes" action and run it.

### 2. Set Up the Dashboard
- Double check that you have all the required custom components installed.
- Go to Settings, Dashboards, Add dashboard, and create a new dashboard from scratch.
- Name it whatever you want.
- Open the new dashboard.
- In this GitHub repository, go to the "wall_tablet_grid_redacted.yaml" file and open it.
- At the top right, choose "Copy raw file."
- Back in Home Assistant, click the three dots in the top right and choose "Raw Configuration Editor."
- Delete anything that's in there. Then, paste in the config file copied from GitHub. Scroll to the bottom and verify that everything was pasted in. There should be approximately 3232 lines of code.
- Click save at the top right and exit the raw config editor.

### 3. Dealing with all the errors
- Because my entities most likely don't exist on your Home Assistant instance, pretty much nothing is going to work right off the bat.
- To add your entities, click Edit on each card that has an error, find the entity parameter, and change it to an entity in your setup.
- This is by far the most time-consuming part, but trust me, it's all worth it in the end.
> [!TIP]
> If you created the custom date and time sensors, now's a good time to add them in. Simply paste the templates below into the content field in the card config to get them working.
>
> **To change the 24 hour time to 12 hour time:** <br>
> ```{{ states('sensor.timenew') }}  ##  replace "timenew" with the entity ID of your 12 hour time sensor```
<br>
>
> **To add the day of the week:** <br>
> ```{{ states('sensor.weekday') }} ## replace "weekday" with the entity ID of your weekday sensor```
>
> **To add the month of the year:** <br>
> ```{{ states('sensor.month') }} ## replace "month" with the entity ID of your month sensor```
>
> **For a result like "Thursday, August 7":** <br>
> ```{{ states('sensor.weekday') }}, {{ states('sensor.month') }} {{ now().day }}```
>
> **For a result like "Thursday, 7 August":** <br>
> ```{{ states('sensor.weekday') }}, {{ now().day }} {{ states('sensor.month') }}```
> 
> Hopefully you get the idea.


🎉 And that's it! Enjoy your new dashboard :)

## ⬙ FREQUENTLY ASKED QUESTIONS ⬙
**How do you get the "phu:" icons working?** <br>
Install the [custom brand icons integration](https://my.home-assistant.io/redirect/hacs_repository/?owner=elax46&repository=custom-brand-icons&category=plugin) from HACS.

**My lights don't have dimmers and can only be turned on and off. How can I remove the brightness circle from the light tiles?** <br>
In the card config under "template", remove "light" and "circle" and add "base" instead. <br>
Like this ↓

```
template:                                       template:
  - light               → → → → → →               - base
  - circle
```


**The 12 hour time sensor isn't working. Why?** <br>
You need to add the Date and Time integration. Most likely, the 12 hour time sensor is attempting to find the 24 hour time sensor (which is provided by this integration) but can't, and that's what's causing the error. You can use the button below to add it.

[![Open your Home Assistant instance and start setting up a new integration.](https://my.home-assistant.io/badges/config_flow_start.svg)](https://my.home-assistant.io/redirect/config_flow_start/?domain=time_date)

**What font are you using?**
<br>
The font is called Quicksand and you can use it for free [with Google Fonts](https://fonts.google.com/specimen/Quicksand). I didn't cover how to get it working in the video/this repo because it would've made the setup process significantly more complicated than I wanted it to be. However, if you do want to use it or another font, the channel My Smart Home did a great tutorial on how to get custom fonts working in Home Assistant, which you can find [here](https://www.youtube.com/watch?v=p6HAxsEGe9M).

**How do I remove the sidebar and top header menu?** <br>
There are a few ways to do this, but I personally like using the [Kiosk Mode integration](https://my.home-assistant.io/redirect/hacs_repository/?owner=NemesisRE&repository=kiosk-mode&category=plugin) because you just need around 3 lines of configuration to get it working. However, you can also use [Browser Mod](https://my.home-assistant.io/redirect/hacs_repository/?owner=thomasloven&repository=hass-browser_mod&category=integration), or the [Wallpanel integration](https://my.home-assistant.io/redirect/hacs_repository/?owner=j-a-n&repository=lovelace-wallpanel&category=plugin) which can also show a screensaver when your tablet isn't being used.

**Why does nothing load and a big red error that says "Custom element doesn't exist: grid-layout" fill the entire screen?** <br>
You didn't install [layout card](https://my.home-assistant.io/redirect/hacs_repository/?owner=thomasloven&repository=lovelace-layout-card&category=plugin)!!! <br> I told you it was crucial  <img width="24" height="24" alt="Untitled 10 001" src="https://github.com/user-attachments/assets/8ffb71c4-af83-492c-a49d-438a0d3a2944" />

## ⬙ CREDITS ⬙
- To [matt8707](https://github.com/matt8707) for providing the dashboard layout and button card templates
- To [Thomas Lovén](https://github.com/thomasloven) for layout card and card mod
- And to [RomRider](https://github.com/RomRider) for the most versitile card ever created, Button card