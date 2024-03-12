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

```
* * * * * /path/to/razer-battery-colors.py >/dev/null 2>&1
```

Because you are going to run the script each minute (or your desired frequency), you don't need to have an endless loop, so you would need to remove the following two lines from code and adjust indentation:

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

## Acknowledgements

- Base script from `lah7`: https://github.com/polychromatic/polychromatic/issues/369#issuecomment-955575360
- `is_charging` value from `Bitmods`: https://github.com/Bitmods/razerbatterymonitor
- Effects from `Openrazer examples`: https://github.com/openrazer/openrazer/tree/master/examples
- Openrazer documentation from the `feature_docs` branch: https://github.com/openrazer/openrazer/tree/feature_docs