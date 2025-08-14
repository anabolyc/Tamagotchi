# ArduinoGotchi - A real Tamagotchi emulator for Arduino UNO

## Synopsis

**ArduinoGotchi** is a real [Tamagotchi P1](https://tamagotchi.fandom.com/wiki/Tamagotchi_(1996_Pet)) emulator running in Arduino UNO hardware. The emulation core is based on [TamaLib](https://github.com/jcrona/tamalib) with intensive optimization to make it fit into UNO's hardware that only comes with 32K Flash 2K RAM.

![Tamagotchi P1 Actual Devices](../main/images/TamaP1_devices.jpg)

## Fork notice

I did the following changes after forking [original repo](https://github.com/GaryZ88/ArduinoGotchi)
- Created a platformio project, so it is easy to target multiple platforms
- Created ports for ESP8266 and ESP32, mainly because speed on 8-bit AVR is just too slow
- Added long click on "back" button - if you press it for 5 seconds, it will reset memory back to egg state
- Added inverted Speaker connection setting. Mainly because the Piezo modules that I have are active on Low. Another reason is mentioned below.

I personally assembled the ESP8266 version with Wemos D1 Mini on perf board, using built-in LED together with speaker, so when it sounds, LED is blinking as well.

### Demo
![Demo #1](/images/VID_20220923_205516.mp4.gif)
![Demo #2](/images/VID_20220923_205528.mp4.gif)
![Demo #3](/images/VID_20220923_205823.mp4.gif)

## How to build and run

Use Platformio. Run `build` task to build for all platforms. Next, run the `Upload` task for a specific platform

### Additional notes
- To activate your pet, you have to configure the clock by pressing the middle button. Otherwise, your pet will not be alive.
- The emulator will save the game status every 5 minutes. You can change that by changing the AUTO_SAVE_MINUTES setting
- The speed of the emulator is a bit slower than the actual Tamagotchi device on AVR, still, it is fun. On ESPs it runs smoothly.
- There are a few costs in the `platformio.ini` that you can adjust to fit your needs:
```
  -D DISPLAY_I2C_ADDRESS=0x3C
  -D SCREEN_WIDTH=128
  -D SCREEN_HEIGHT=64
  -D ENABLE_TAMA_SOUND
  -D ENABLE_TAMA_SOUND_ACTIVE_LOW
  -D ENABLE_AUTO_SAVE_STATUS
  -D ENABLE_LOAD_STATE_FROM_EEPROM
```

### Board revisions

#### Revision A

First prototype, used external boost converter module soldered on the PCB for simplicity. However boost converter I used would give up too early, not pulling all the juice from the battery.
This version contained one schematic issue: GPIO2 used for buzzer caused boot issues (bootstrap pin)

#### Revision B

Switched over to on-board boost converter (TPS61040DBVR). Buzzer pin changed to IO0.

#### Revision C

Identical electrically to rev B, however I switched to onboard SMD buttons (soldered from factory), as they had much better feel compared to the through-hole buttons I used before. 

#### Revision D

Switched to XC9140A331MR boost coverter, as this one can run down to 0.9V before it dies out. Added onboard power switch, so it can be switched off without removing batteries. Switched to onboard buzzer I used in another project to make bottom line flat (that way it can be placed on the desk for example).

### Wemos D1 Mini

![image](https://user-images.githubusercontent.com/5459747/192046441-1461c20e-b5f6-4a72-a79b-7815a77e1c00.png)
![image](https://user-images.githubusercontent.com/5459747/192046464-36be5fc8-1893-4b04-a4e0-53a563c3ad33.png)
![image](https://user-images.githubusercontent.com/5459747/192046512-4251114a-d74a-42fc-b67a-0f48a1a26ef4.png)
![image](https://user-images.githubusercontent.com/5459747/192046612-6c835d33-e341-4386-8917-5823d573414d.png)

### License
ArduinoGotchi is distributed under the GPLv2 license. See the LICENSE file for more information.

### Where to buy

You may support my work by ordering the kit on [Tindie](https://www.tindie.com/products/sonocotta/esp8266-tamagotchi-diy-kit/) or [Elecrow](https://www.elecrow.com/esp8266-tamagotchi-diy-kit.html)
