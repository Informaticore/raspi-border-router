# raspi-border-router
All you need to know about how to setup an OpenThread Border Router using a Raspberry Pi 4 and the nRF52840 USB Dongle

# Build and flash nRF52840 USB Dongle

In order to open up the Thread network we need a IEEE802.15.4 transceiver. OTBR supports a 15.4 radio chip in Radio Co-Processor (RCP) mode. In this mode, the OpenThread stack is running on the host side and transmits/receives frames over the IEEE802.15.4 transceiver. The nRF52840 USB Dongle can do just that for us!

Go to the [ot-nrf528xx repo](https://github.com/openthread/ot-nrf528xx) clone it and follow the [OpenThread on nRF52840 Example](https://github.com/openthread/ot-nrf528xx/blob/main/src/nrf52840/README.md)

#### Short Summary
```
git clone https://github.com/openthread/ot-nrf528xx.git
cd ot-nrf528xx
git submodule update --init
./script/bootstrap
./script/build nrf52840 USB_trans -DOT_COMMISSIONER=ON -DOT_THREAD_VERSION=1.3 -DOT_BOOTLOADER=USB
arm-none-eabi-objcopy -O ihex build/bin/ot-rcp ot-rcp.hex
```

Attention: we will need the RCP firmware for the raspberry pi.

# Install OpenThread on the Raspberry Pi

You can go and follow this [OpenThread Guide](https://openthread.io/codelabs/openthread-border-router#1) - I will just summarise it here.

Install [raspios-buster-armhf-lite](https://downloads.raspberrypi.org/raspios_lite_armhf/images/raspios_lite_armhf-2021-05-28/2021-05-07-raspios-buster-armhf-lite.zip) onto your Raspberry Pi 4. The guide recommends this version I am not sure if it will also run with a plain Raspberry OS.

#### On the Raspberry Pi:

```
git clone https://github.com/openthread/ot-br-posix.git --depth 1

cd ot-br-posix
./script/bootstrap
INFRA_IF_NAME=wlan0 ./script/setup
```

once this is working stick in the nRF52840 stick and restart the service `sudo service otbr-agent restart`
