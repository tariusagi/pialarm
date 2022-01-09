# pialarm
Simple alarm system on a Raspberry Pi

## alarm

A Python 2 program to sound an buzzer via a GPIO pin.

This is a Python 2 program. It requires the `RPi.GPIO` package. Install it with `pip install rpi.gpio`.

Usage: 

```sh
Syntax: alarm [OPTION] [on|off]
OPTION:
-h      Display this help and exit.
-i sec  Set interval in seconds between alarms (float, default 0.5).
-n num  Repeat alarm "num" times (default 1).
-p num  Set the GPIO physical pin number (default 29).
-t sec  Set the alarm duration in second (float, default indefinitely).
on      Turn the alarm on.
off     Turn the alarm off.
```
