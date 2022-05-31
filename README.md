# Pebble Watchface

Forked from https://github.com/clach04/pebble_watchface_framework

Watchface that switches the background image every hour.

## Developing

Create a `resources/images` folder and put your background images inside them. Add images to the resources part of `package.json`.

Commands to help with developing. Run them inside this directory.

### Windows

Build

```sh
docker run -it -v //$(PWD)/:/pebble/ bboehmke/pebble-dev pebble build
```

Screen emulator (requires [Windows X-server](https://sourceforge.net/projects/vcxsrv/) with disabled access control)

```sh
docker run -e DISPLAY=host.docker.internal:0.0 -it -v //$(PWD)/:/pebble/ bboehmke/pebble-dev
```

then run

```sh
pebble build
pebble install --emulator basalt --logs # emulator aplite, basalt, or chalk
```

## Additional options

In an ideal situation, `watchface.c` and `watchface.h` should not need editing _ever_. There may be cases where `main.c` needs editing. Most of the time `watch_config.h` is the only file that will need editing. `watch_config.h` options:

- If `DEFAULT_TIME_COLOR` is defined it will be used for the default time color.
- If `DEFAULT_BACKGROUND_COLOR` is defined it will be used for the default background time color.
- If an image is present under resources and defined as `BG_IMAGE`, it will be used as a background image. For Basalt, image transparency is honored.
- If a font present under resources and defined as `FONT_NAME`, it will be used for displaying the time. E.g. `#define FONT_NAME RESOURCE_ID_.....`
- If `FONT_SYSTEM_NAME` is defined to a system font name and `FONT_NAME` is not defined, that system font will be used for time display
- if `REMOVE_LEADING_ZERO_FROM_TIME` is defined, and the watch is configured for 12 hour format display, the leading zero "0" will be removed for times in the morning.
- `CLOCK_POS`, `BT_POS, DATE_POS`, and `BAT_POS` to change the on screen position of Time, Bluetooth disconnect message, Date, and battery power.
- `FONT_BT_SYSTEM_NAME`, `FONT_BAT_SYSTEM_NAME`, and `FONT_DATE_SYSTEM_NAME` can override the system font used.
  - if `USE_TIME_FONT_FOR_DATE` is defined, the date is displayed using the same font as used for time
- `BT_ALIGN`, `BAT_ALIGN`, and `TIME_ALIGN` are used to change text alignment.
- If `BLUETOOTH_DISCONNECTED_STR` is defined, this text will be displayed for the Bluetooth disconnect message.
- If `BT_DISCONNECT_IMAGE` is defined, this resource image will be displayed on bluetooth disconnect.
  - `BT_DISCONNECT_IMAGE_GRECT` can be used to position the image
- If `DATE_FMT_STR` is defined it will be used for the format of the date text.
- If `BAT_FMT_STR` is defined it will be used for the format of the battery power text.
  - If `DRAW_SMALL_BATTERY` is defined a small gauge will be used.
- If `DRAW_BATTERY` is defined a graphical gauge will be used instead of text
- If `QUIET_TIME_IMAGE` is defined as a bitmap resource identifier it will be displayed when quiet time is active.
  - Optionally set `QUIET_TIME_IMAGE_GRECT` to location/size
- `NO_BLUETOOTH`, `NO_BATTERY`, and `NO_DATE` will disable display of bluetooth disconnect, battery status, and date.
- If `USE_HEALTH` is defined step counts will be displayed. Pebble Time and later only.
  - If `UPDATE_HEALTH_ON_ACTIVITY` is set, step count is updated when the Pebble Health service has an update. If not set, step count is updated once per minute.
  - `HEALTH_POS` is a GRect()
  - `HEALTH_FMT_STR` is the format of the string to display. `MAX_HEALTH_STR` should be updated if `HEALTH_FMT_STR` is set.
- If `USE_TIME_MACHINE` is defined https://github.com/MorrisTimm/pebble-time-machine will be used ('dependencies' in package.json should be filled in, known to work with version 1.0.2).
