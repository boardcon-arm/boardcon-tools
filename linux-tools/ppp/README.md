# PPP Test Tool

PPP utilities for Boardcon embedded platforms.

This directory contains PPP related tools and configuration files for
4G modem and GPS testing.

## Contents

- pppd
- chat
- PPP configuration scripts


## 4G Network Test

### Start PPP Connection

Start 4G network connection:

```bash
pppd call quectel-ppp &
```

Check network status:

```bash
ifconfig ppp0
```

## GPS Test

### Enable GPS

Enable GPS function:

```bash
killall pppd
echo -e "AT+QGPS=1\r\n" > /dev/ttyUSB2
```

### Read GPS Data

Read GPS information:

```bash
cat /dev/ttyUSB1
```

## Supported Platforms

- Boardcon EM3588 (RK3588)
- Boardcon SBC3568 (RK3568)


## Kernel Support

- Linux 6.1 vendor kernel
