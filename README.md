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

## alarmserver

Run a network server and serve requests from clients to control the attached buzzer.

This program need `ncat` and `alarm` on system PATH.

Usage:

```sh
./alarmserver server 127.0.0.1 1235
```

To send command to this server, use `nc`, `ncat` or nay network client. For example, the following command tell the server to sound a quick beep 3 times:

```sh
echo on -t0.1 -i0.1 -n3 | nc -w1 127.0.0.1 1235
```

Supported commands are:

- `on`: sound the alarm with the arguments per the `alarm` usages.
- `off`: turn the alarm off.

To enable logging, add "log" at the end of command. The server's log is at `/var/log/alarmserver.log`.

To run this server at boot, use `/etc/rc.local` or create a service file `/etc/systemd/system/alarmserver.service` like this (assumming path to this server is at `/usr/local/bin/alarmserver`):

```systemd
[Unit]
Description=Listen and display on attached LCD
Wants=network-online.target
After=network-online.target

[Service]
User=peter
Group=peter
ExecStart=/usr/local/bin/alarmserver server 0.0.0.0 1235
ExecReload=/bin/bash -c 'echo off | nc -w1 127.0.0.1 1235'
Restart=on-failure
RestartSec=5

[Install]
WantedBy=default.target
```

Then run:

```sh
sudo systemctl daemon-reload
sudo systemctl enable alarmserver
sudo systemctl start alarmserver
```

To start the server as a service.
