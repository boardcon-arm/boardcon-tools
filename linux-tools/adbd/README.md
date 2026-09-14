# ADB Tool

ADB daemon and USB Gadget configuration for Boardcon embedded Linux platforms.

This directory contains the pre-built `adbd` binary and configuration
steps for enabling ADB device mode on Rockchip Linux platforms.

## Contents

- adbd
- USB Gadget ConfigFS setup commands

## Usage

### 1. Switch USB OTG to Device Mode

```bash
echo device > /sys/kernel/debug/usb/fcc00000.usb/mode
```

### 2. Create ADB ConfigFS Gadget

Create USB gadget:

```bash
G=/sys/kernel/config/usb_gadget/rockchip
mkdir -p $G
echo 0x2207 > $G/idVendor
echo 0x0010 > $G/idProduct
```

Create USB descriptor:

```bash
mkdir -p $G/strings/0x409
echo 0123456789ABCDEF > $G/strings/0x409/serialnumber
echo Boardcon > $G/strings/0x409/manufacturer
echo SBC3568 > $G/strings/0x409/product
```

Create configuration:

```bash
mkdir $G/configs/b.1
mkdir $G/configs/b.1/strings/0x409
echo "ADB" > $G/configs/b.1/strings/0x409/configuration
echo 120 > $G/configs/b.1/MaxPower
```

Create FunctionFS ADB function:

```bash
mkdir $G/functions/ffs.adb
```

Create function link:

```bash
cd $G
ln -s functions/ffs.adb configs/b.1/
```

### 3. Install adbd

Copy `adbd` to target:

```bash
cp adbd /usr/bin/adbd
chmod 755 /usr/bin/adbd
```

### 4. Mount FunctionFS

```bash
mkdir -p /dev/usb-ffs/adb
mount -t functionfs adb /dev/usb-ffs/adb
```

### 5. Start ADB daemon

```bash
/usr/bin/adbd &
```


### 6. Bind Rockchip UDC

```bash
echo fcc00000.usb > $G/UDC
```


## Supported Platforms

- Boardcon EM3588 (RK3588)
- Boardcon SBC3568 (RK3568)


## Kernel Support

- Linux 6.1 vendor kernel
