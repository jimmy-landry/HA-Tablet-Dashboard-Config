# HA-Tablet-Dashboard-Config
Wall-mounted tablet dashboard config as seen in my YouTube video (https://www.youtube.com/watch?v=Wr7s_49w9VA)

<img width="1240" alt="Screenshot 2024-02-10 at 8 46 18 AM" src="https://github.com/jimmylandry54/HA-Tablet-Dashboard-Config/assets/121106900/462097c4-b3cf-4529-90f8-1bcfb4f361d4">


Running HA OS on a Raspberry Pi 3B+. Using an Amazon Fire HD 10 running Android WallPanel for the dashboard.

## You'll need the following custom repos:
_All these are available in HACS._

[layout-card](https://github.com/thomasloven/lovelace-layout-card)

[button-card](https://github.com/custom-cards/button-card)

[vertical-stack-in-card](https://github.com/ofekashery/vertical-stack-in-card)

[lovelace-mushroom (used under the clock in the sidebar) ](https://github.com/piitaya/lovelace-mushroom)

[mini-media-player](https://github.com/kalkih/mini-media-player)

[browser-mod (optional, used for popups on the buttons on the bottom of the screen)](https://github.com/thomasloven/hass-browser_mod)

[simple-weather-card](https://github.com/kalkih/simple-weather-card)

------------------------------------

**Credit where credit is due:**
The layout of the dashboard was heavily inspired by matt8707's tablet dashboard, which you can find here: https://github.com/matt8707/hass-config. I reused some of the button-card templates, replaced the sidebar with a vertical-stack-in-card, and deleted the custom icon config so I could use the built-in MDI icons.

I'm sure there's probably code in there I don't need, as the actual card config doesn't even start until line 2696 😂... however it's been working great for me for more than 6 months now, so I haven't seen a reason to waste time removing it.

It's not necessary, but I recommend enabling the theme.yaml file on your tablet, as I designed this dashboard with that theme in mind. It's a smattering of stock HA, UI-Lovelace-Minimalist, and matt8707's theme, so it's a total mess too, but should work just fine.

Don't forget to star my repo if you like this dashboard! 😁
