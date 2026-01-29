# Frosted Glass Home Assistant Tablet Dashboard inspired by iOS 26

![GitHub Repo stars](https://img.shields.io/github/stars/jimmy-landry/ha-tablet-dashboard-config)
![YouTube Video Views](https://img.shields.io/youtube/views/vtzaqNQkd6I)
![YouTube Video Likes](https://img.shields.io/youtube/likes/vtzaqNQkd6I)
[![YouTube](https://img.shields.io/badge/YouTube-Tamper%20Evident-red?logo=youtube)](https://www.youtube.com/@dontuseiftamperevident)

<img width="1920" height="1080" alt="Preview" src="https://github.com/user-attachments/assets/a5ff7ab4-521b-47f2-bd6c-0e96dded0cf3"/>

I've finally updated my long outdated tablet dashboard! I really wanted to focus on making it modern, sleek and everything in between, and Apple's iOS 26 update gave me some great ideas. This page will guide you through the process of loading this dashboard into your Home Assistant instance and answer some commonly asked questions from last time.

Thanks for all the positive feedback on last year's dashboard(s), it really means a lot!

## ⬙ HIGHLIGHTS ⬙
- All-new frosted glass design
- 5 sections with customizable tiles
- Fully customizable sidebar (add whatever cards you want!)
- Custom media player card with quick controls (power, play, pause, skip, and volume) and album art as background
- Weather card with beautiful animated icons
- Footer buttons that open popups for further control

## ⬙ BEFORE YOU START ⬙

### Check Custom Components
Ensure you have **all** of the following custom components installed. Otherwise, things may not work properly. All can be installed via HACS.
- [Layout Card](https://my.home-assistant.io/redirect/hacs_repository/?owner=thomasloven&repository=lovelace-layout-card&category=plugin)
- [Button Card](https://my.home-assistant.io/redirect/hacs_repository/?owner=custom-cards&repository=button-card&category=plugin)
- [Bubble Card](https://my.home-assistant.io/redirect/hacs_repository/?owner=Clooos&repository=Bubble-Card&category=plugin)
- [Mini Graph Card](https://my.home-assistant.io/redirect/hacs_repository/?owner=kalkih&repository=mini-graph-card&category=plugin)
- [Swipe Card](https://my.home-assistant.io/redirect/hacs_repository/?owner=bramkragten&repository=swipe-card&category=plugin)
- [Vertical Stack In Card](https://my.home-assistant.io/redirect/hacs_repository/?owner=ofekashery&repository=vertical-stack-in-card&category=plugin)
- [Card Mod](https://my.home-assistant.io/redirect/hacs_repository/?owner=thomasloven&repository=lovelace-card-mod&category=plugin)
> [!IMPORTANT]
> The layout card component is **CRUCIAL** for the dashboard to function properly. If not installed, nothing will load and you'll get a nice, large error box that shows the _entire_ config file that makes the dashboard work. No one wants that, so please be sure you have this installed. I got a lot of questions about this last time around so I figured I should make it extra clear.

### Configure Date & Time Sensors (optional)

<img width="1920" height="1080" alt="Preview" src="https://github.com/user-attachments/assets/184e391e-a492-4c90-bdda-66382ef8af5b"/>

<br>

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

Take note of the names you choose for the sensors as you'll need them once you create the dashboard.

## ⬙ SETUP ⬙

### 1. Download the Required Files
- At the top of the main page of this repository, click the green "Code" button.
- Choose "Download ZIP."
- Extract the contents using your preferred method.
- Once extracted, you'll get a folder containing the files for both the old dashboard and the new dashboard. This guide will only focus on the new dashboard, so you can ignore the old dashboard files for now.

### 2. Set Up the Animated Weather Icons
> [!TIP]
> If you followed along with my  [mobile dashboard guide last summer](https://www.youtube.com/watch?v=__n_XveKa4Q]), you already have these icons set up correctly and you can skip this step.
> 
> **⬘ ALSO ⬘**
>
> I would strongly recommend using the [Samba Share add-on](https://my.home-assistant.io/redirect/supervisor_addon/?addon=core_samba) for this step. However, if you don't already have it, don't worry about installing it specifically for this purpose as file editor will work just fine. Instructions for both methods are below.

#### Samba Share Users:
- Connect to your HA server using your computer's network storage feature.
    - In most cases, you can use either the hostname or IP address, but this can vary depending on your network configuration
- Mount the "config" volume.
- Once connected, find the "www" folder and open it.
- From another window, drag and drop the "weather_icons" folder into the "www" folder.
- Wait for it to finish, then open the folder and check if all icons were added. There should be 25 icons total.

#### File Editor Users:
- Open the file editor add-on.
- Choose "Browse Filesystem" in the top left.
- Navigate to your "www" folder. In the folder, create a new folder and name it "weather_icons". The name must exactly match the name of the weather icons folder in the GitHub download, otherwise it won't work.
- Open the newly created folder.
- At the top of the file browser, click the upload icon. Click the "File" button and navigate to the weather icons folder in the GitHub download on your computer.
- Choose the first icon in the folder and upload it.
- Repeat for each of the icons.
    - I warned you this wasn't fun 🙃
- When done, double check that you added all of the icons. There should be 25 icons total.

### 3. Set Up the Theme

- Back in the GitHub download, find the "Frosted Glass Dashboard Theme.yaml" file.
- On your Home Assistant server, find the "themes" folder.
    - If you don't have this folder: 
        - Create it. It must be named "themes" exactly, otherwise, again, it won't work.
        - Add the following snippet of code to your configuration.yaml file:
            ```
             frontend:
               themes: !include_dir_merge_named themes
            ```
        - Restart Home Assistant.
- **Samba Share Users:** Drag and drop the theme file to the themes folder on your Home Assistant server.
- **File Editor Users:** Navigate to the themes folder, choose upload file, and upload the theme file from the GitHub download.
- Go to Developer Tools, actions, then search for the "Reload themes" action and run it.

### 4. Set Up the Dashboard
- Double check that you have all the required custom components installed.
- Go to Settings, Dashboards, Add dashboard, and create a new dashboard from scratch.
- Name it whatever you want.
- Open the new dashboard.
- In this GitHub repository, go to the "Frosted Glass Dashboard Config" file and open it.
- At the top right, choose "Copy raw file."
- Back in Home Assistant, click the three dots in the top right and choose "Raw Configuration Editor."
- Delete anything that's in there. Then, paste in the config file copied from GitHub. Scroll to the bottom and verify that everything was pasted in. There should be approximately 3039 lines of code.
- Click save at the top right and exit the raw config editor.

### 5. Add the Background Image
- Click the pencil icon next to the view name at the top of the dashboard.
- Go to the Background tab.
- In the GitHub download, find the "background.jpg" file and drag it into the drop zone in Home Assistant.
-  Save and exit.
> [!TIP]
> I found that the dashboard looks pretty good with most other dark-ish photos as well so feel free to use whatever image you want, you don't have to use the one included in this repo.

<img width="1920" height="1080" alt="different background 001" src="https://github.com/user-attachments/assets/89312943-1d1e-482c-b30a-32e33d7d7f46" />
<div align="center">
  <strong>As seen in the YouTube thumbnail</strong>
</div>

### 6. Dealing with all the errors (the fun part!)
- Because my entities most likely don't exist on your Home Assistant instance, pretty much nothing is going to work right off the bat.
- To add your entities, click Edit on each card that has an error, find the entity parameter, and change it to an entity in your setup.
> [!NOTE] 
> The sidebar and HomePod cards are actually two cards layered on top of each other. You can edit the top layer by clicking the edit button as usual, but in order to edit the bottom layer you must first remove the top layer. To do this:
> - Click the three dots on the top layer card.
> - Choose "Cut."
> - You can now edit the bottom layer. When done, save, choose "Add card", and paste the top layer back in.
- This is by far the most time-consuming part, but trust me, it's all worth it in the end.

> [!TIP]
> If you created the custom date and time sensors, now's a good time to add them in. Simply paste the templates below into the content field in the card config to get them working. Feel free to modify the templates to your liking.
>
> **To change the 24 hour time to 12 hour time:** <br>
> ```{{ states('sensor.timenew') }}  ##  replace "timenew" with the entity ID of your 12 hour time sensor```
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


I **really** cleaned up the config file this time around so it should hopefully be much easier to read through and work with. Ideally, you shouldn't have to mess with the main config file since everything is editable via the normal UI, but if for whatever reason you do, it should be a lot more managable.

🎉 And that's it! Enjoy your new dashboard :)

## ⬙ FREQUENTLY ASKED QUESTIONS ⬙

**Why are my icons different from yours?** <br>
In additon to using the [custom brand icons integration](https://my.home-assistant.io/redirect/hacs_repository/?owner=elax46&repository=custom-brand-icons&category=plugin) from HACS, I also use some of Apple's SF Symbols icons simply because I think they look nicer than the built-in MDI icons. Some can be accessed and used with the [Cupertino icons integration](https://my.home-assistant.io/redirect/hacs_repository/?owner=menahishayan&repository=HomeAssistant-Cupertino-Icons&category=integration), however, most of the others are copyrighted and as a result the legality of adding them to this repository is murky. If you want to use them, you'll need to download them from the [Apple Developer website](https://developer.apple.com/sf-symbols/) yourself.

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
There are a few ways to do this, but I personally like using the [Kiosk Mode integration](https://my.home-assistant.io/redirect/hacs_repository/?owner=NemesisRE&repository=kiosk-mode&category=plugin) because you only need around 3 lines of config to get it working. However, you can also use [Browser Mod](https://my.home-assistant.io/redirect/hacs_repository/?owner=thomasloven&repository=hass-browser_mod&category=integration) or the [Wallpanel integration](https://my.home-assistant.io/redirect/hacs_repository/?owner=j-a-n&repository=lovelace-wallpanel&category=plugin) which can show a screensaver when your tablet isn't being used.

**Why does nothing load and a big red error that says "Custom element doesn't exist: grid-layout" fill the entire screen?** <br>
You didn't install [layout card](https://my.home-assistant.io/redirect/hacs_repository/?owner=thomasloven&repository=lovelace-layout-card&category=plugin)!!! <br> I told you it was crucial...


> I'll add to these if more repeated questions come up

## ⬙ CREDITS ⬙
- To [matt8707](https://github.com/matt8707) for providing the dashboard layout, button card templates, background image, bounce animation, and other odds and ends
- To [Clooos](https://github.com/Clooos) for his amazing work with Bubble Card
- To [Nezz](https://github.com/Nezz) (Adam) for the visionOS and Liquid Glass theme
- To [Basmilius](https://github.com/basmilius) for the [weather icons](https://basmilius.github.io/weather-icons/index.html)
- To [Thomas Lovén](https://github.com/thomasloven) for layout card, card mod, and a bunch of other custom integrations I can't remember the names of
- And to [RomRider](https://github.com/RomRider) for the most versitile card ever created, Button card.

<p align="right"><a href="#top"><img src="https://github.com/user-attachments/assets/ddfb8c1b-f779-4959-b267-be5ca41233ad" height="70px"></a></p>
