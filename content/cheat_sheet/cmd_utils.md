+++
date = '2025-03-18T15:44:05+01:00'
draft = false
title = 'Cmd_utils'
+++

Just personal notes.



# Network
## Connect with NetwokManager
List of wifi:
```bash
nmcli device wifi list
```

Connection:
```bash
nmcli device wifi connect "SSID" password "PASSWORD"
```

Check connection:
```bash
nmcli device show
```

## IP
Get IP address:
```bash
ifconfig | grep -E -o '([0-9]{1,3}\.){3}[0-9]{1,3}'
```

Or use `ip`:
```bash
ip | grep -E -o '([0-9]{1,3}\.){3}[0-9]{1,3}'
```

# Arduino-cli
## New sketch:
```bash
arduino-cli sketch new "sketch_name"
```

## List boards:
```bash
arduino-cli board list
```

## Install board:
```bash
arduino-cli core install "board_name:architecture"
```

## List of install
```bash
arduino-cli core list
```

## Compile:
```bash
arduino-cli compile --fqbn "board_name:architecture" "sketch_name"
```

## Upload:
```bash
arduino-cli upload -p "port" --fqbn "board_name:architecture" "sketch_name"
```

## Search library:
```bash
arduino-cli lib search "library_name"
```
- WiFiNINA
- RadioHead


## Install library:
```bash
arduino-cli lib install "library_name"
```
