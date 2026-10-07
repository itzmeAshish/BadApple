# Bad Apple on a 0.96" OLED

![Arduino](https://img.shields.io/badge/Arduino-IDE-00979D?logo=arduino&logoColor=white)
![OLED](https://img.shields.io/badge/Display-SSD1306%20128x64-111827)
![Frames](https://img.shields.io/badge/frames-385-blue)
![Speed](https://img.shields.io/badge/speed-67ms%20~15fps-green)
![Board](https://img.shields.io/badge/board-ESP32%20%7C%20ESP8266-purple)

**385 frames. 128×64 SSD1306. ESP32 or ESP8266. Free Arduino sketch.**

The shadow-art Bad Apple loop, playing on a tiny mono display. Pictures only. No audio.

**Repo:** [https://github.com/itzmeAshish/BadApple](https://github.com/itzmeAshish/BadApple)  
**Full tutorial:** [Bad Apple OLED Arduino guide](https://www.oledanimationmaker.com/blog/bad-apple-oled-animation-arduino-ssd1306.html)  
**Made with:** [OLED Animation Maker](https://www.oledanimationmaker.com/)

> Drop a 10-second phone video of your OLED under this line. That clip is what gets the repo shared.

---

## Keywords

`bad apple oled` · `bad apple arduino` · `bad apple esp32` · `bad apple esp8266` · `bad apple ssd1306` · `bad apple 128x64` · `bad apple 0.96 oled` · `play bad apple on oled` · `progmem bitmap` · `adafruit ssd1306 animation`

---

## What you get

| | |
|---|---|
| Frames | 385 (`frame0` … `frame384`) |
| Display | 0.96" SSD1306, 128×64, address `0x3C` |
| Speed | 67 ms per frame, about 15 fps, one pass about 26 seconds |
| File | `main.ino` — Adafruit SSD1306 + GFX, pictures in PROGMEM |
| Sound | None. This is the animation only. |

One upload. No SPIFFS image, no SD card, no extra video file.

---

## Requirements

| Item | Notes |
|------|--------|
| Board | **ESP32**, or **ESP8266 with 4MB flash** |
| OLED | 0.96" SSD1306 I2C 128×64 |
| IDE | Arduino IDE 1.8+ or 2.x |
| Libraries | Adafruit SSD1306, Adafruit GFX |

> **Flash size:** 385 frames × 1024 bytes = **394,240 bytes** (385 KB) of pictures. A classic **Arduino Uno (32 KB)** cannot store this sketch. Use ESP32 or a 4MB ESP8266.

---

## Connection / Wiring

The sketch header lists Uno pins (SDA A4, SCL A5). For this frame count, wire an ESP32 or ESP8266.

### ESP32 (recommended)

| OLED | Board |
|------|--------|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO21 |
| SCL | GPIO22 |

```cpp
Wire.begin(21, 22);
```

### ESP8266 NodeMCU / Wemos D1 Mini

| OLED | Board | GPIO |
|------|--------|------|
| VCC | **3.3V** | — |
| GND | GND | — |
| SDA | **D2** | GPIO4 |
| SCL | **D1** | GPIO5 |

If the screen stays blank, add this before `display.begin()`:

```cpp
Wire.begin(4, 5);  // SDA, SCL on ESP8266
```

On ESP8266, set **Tools → Flash Size** so the sketch has room. A 4MB module is the safe choice. A 512 KB board will not fit 385 frames.

---

## Install & upload

1. **Download** this repo
   - ZIP: GitHub → **Code → Download ZIP**
   - or `git clone https://github.com/itzmeAshish/BadApple.git`
2. The ZIP folder is often `BadApple-main`. Rename it to **`main`**, then open `main/main.ino`.  
   The folder name must match the file name.
3. Install libraries: **Adafruit SSD1306** + **Adafruit GFX Library**
4. Select board + COM port
   - *ESP32 Dev Module* or *NodeMCU 1.0 (ESP-12E Module)*
5. Click **Upload**. This file is large, so compile takes longer than Blink.
6. The Bad Apple animation loops on the OLED.

Serial baud: **115200**. If you see `SSD1306 allocation failed`, check 3.3V, SDA, and SCL, then try address `0x3D`.

Search the sketch for `frame384` after download. If that name is missing, the ZIP was cut off. Download it again.

---

## Speed control

In `loop()`, change only the delay:

```cpp
if (millis() - lastMs >= 67) {  // lower = faster
```

| Value | Approx FPS | One pass (385 frames) |
|------:|-----------:|----------------------:|
| 67 | ~15 | ~26 seconds |
| 100 | 10 | 38.5 seconds |
| 150 | ~6.7 | ~58 seconds |

`numFrames` is **385**. The index is `uint16_t` because 385 does not fit in an 8-bit counter. Do not change it to `uint8_t`.

On a 100 kHz I2C bus, `display()` can take about 90 ms, so one pass may be closer to **35 seconds**. To hit the stock 26 seconds, after the bus starts:

```cpp
Wire.setClock(400000);
```

---

## How this was made

1. Open [oledanimationmaker.com](https://www.oledanimationmaker.com/)
2. **Import** a short Bad Apple clip
3. Tune threshold so the shadow art stays a clean cut-out on the 128×64 preview
4. **Get the Code** → Adafruit SSD1306 Arduino
5. Save as `main.ino` and push to this repo

Tutorial:  
https://www.oledanimationmaker.com/blog/bad-apple-oled-animation-arduino-ssd1306.html

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Blank screen | 3.3V on ESP, check SDA/SCL, try `0x3D` |
| Sketch too big | Use ESP32 or a 4MB ESP8266. Uno cannot hold 385 KB of frames |
| “Sketch folder must match” | Rename `BadApple-main` to `main` if the file is `main.ino` |
| Loop restarts in the middle | Keep `frameIdx` as `uint16_t` |
| Slower than 26 seconds | The I2C bus is the clock. Call `Wire.setClock(400000)` |
| No sound | Expected. This build is pictures only |

More help: [OLED blank screen guide](https://www.oledanimationmaker.com/blog/arduino-oled-blank-screen-fix-ssd1306.html)

---

## Show your build

Film the 0.96" glass for one full loop and post it. Tag the clip **Bad Apple OLED**.

---

## Links

- **Code:** https://github.com/itzmeAshish/BadApple
- **Blog:** https://www.oledanimationmaker.com/blog/bad-apple-oled-animation-arduino-ssd1306.html
- **Tool:** https://www.oledanimationmaker.com/
- **Author:** [Ashish (itzmeAshish)](https://github.com/itzmeAshish)
- **Instagram:** https://www.instagram.com/oled_animator
- **YouTube:** https://www.youtube.com/@oled_animator

---

## License

Free to use for learning, demos, and personal projects.  
Credit appreciated: link back to this repo and [oledanimationmaker.com](https://www.oledanimationmaker.com/).
