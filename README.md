# Razer Viper Ultimate (Wireless) and Razer Mouse Dock

Set the colors on the Dock depending on the battery of the mouse:

- Green: Battery > 75%
- Yellow: Battery > 50% and <= 75%
- Orange: Battery > 25% and <= 50%
- Red: Battery < 25%

Glowing effect if mouse is charging (docked), static when not charging (undocked).

## Usage

```bash
chmod +x razer-battery-colors.py
```

### Crontab:

```bash
* * * * * /path/to/razer-battery-launcher.sh >/dev/null 2>&1
```

For this approach we can't call the script directly from crontab like this: `* * * * * /path/to/razer-battery-colors.py >/dev/null 2>&1`, doing so will result in errors like this (see https://github.com/slavid/razer-mouse-dock-color-battery/issues/1):

```
dbus.exceptions.DBusException: org.freedesktop.DBus.Error.NotSupported: Unable to autolaunch 
a dbus-daemon without a $DISPLAY for X11
```

A workaround for this is to call the Python script from a shell script that has exported the variable `DBUS_SESSION_BUS_ADDRESS` with your user id in it like this:

```bash
#!/bin/sh

export DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus

/usr/bin/python3 /path/to/crontab-razer-battery-colors.py
```

Where `1000` is your user id (you can check your user id with the command `$ id` and copy the number after uid `uid=1000`).

Because you are going to run the script each minute (or your desired frequency), you don't need to have an endless loop, so you would need to use the script named `crontab-razer-battery-colors.py` which what it does is remove the following two lines from `razer-battery-colors.py` and adjust indentation:

Line 10:
```python
while True:
```

Line 71:
```python
time.sleep(300) # 5 minutes
```

### Run at Startup:

On Ubuntu-based systems you can create a `Startup Application` and add the following:

```
Name: Razer Battery Level Dock
Command: /usr/bin/python3 /path/to/razer-battery-colors.py &
Description: Change Dock colors depending on the battery level of the mouse
```

You can adjust the time between checks from 300 seconds (5 minutes) to your desired frequency.

This approach may not work perfectly, see https://github.com/slavid/razer-mouse-dock-color-battery/issues/1

## Acknowledgements

- Base script from `lah7`: https://github.com/polychromatic/polychromatic/issues/369#issuecomment-955575360
- `is_charging` value from `Bitmods`: https://github.com/Bitmods/razerbatterymonitor
- Effects from `Openrazer examples`: https://github.com/openrazer/openrazer/tree/master/examples
- Openrazer documentation from the `feature_docs` branch: https://github.com/openrazer/openrazer/tree/feature_docs