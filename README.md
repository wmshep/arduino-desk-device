# Arduino Desk Interface

A physical desk device that tracks study time, flashcard reviews and meditation sessions on an
OLED screen, so those habits live on my desk instead of in another app on my phone.

Built on an Arduino Uno with a 1.3" OLED and four tactile buttons. Went from my first Arduino
tutorial to a working multi-screen device in about ten days.

<!-- Add photos to a photos/ folder and uncomment:
![The device](photos/device.jpg)
-->

---

## What it does

Three screens, cycled with the bottom button:

**1. Daily stats**
```
Study:     1h 23m
Anki:      DONE
Meditate:  2x
2:32PM         Mon 24 May
```

**2. Server stats** — pulled from the home lab
```
NAS:  186GB/256GB
OPi:  uptime 4d 2h
Pi-hole: 34% blocked
Anki: 8 cards due
```

**3. Meditation countdown**
```
MEDITATION
Focus and breathe
      4:32
```

Data logs back to a server on my [home lab](https://github.com/wmshep/homelab) through a Python
bridge over USB serial.

---

## Hardware

| Component | Purpose |
|---|---|
| Arduino Uno | Controller |
| SH1106 1.3" OLED (I2C) | Display |
| Tactile buttons × 4 | Input |
| DFPlayer Mini + microSD module | Audio alerts |
| 3.5mm audio jack | Headphone output |
| TP4056 charger + LiPo | Portable power |
| 170pt breadboard, jumper wires | Prototyping |

### Wiring

**OLED (I2C)**

| OLED | Arduino |
|---|---|
| VCC | 3.3V |
| GND | GND |
| SCK | A5 |
| SDA | A4 |

**Buttons**

| Button | Pin | Function |
|---|---|---|
| Blue | D2 | Study timer start/stop |
| Yellow | D3 | Flashcards done/todo toggle |
| White | D8 | 5-minute meditation countdown |
| Red | D9 | Cycle screens |

---

## Things I learned the hard way

- **Two buttons were firing together no matter what I changed in code.** After stripping it back
  to a single-button test sketch, the cause turned out to be hardware: digital pins D4–D7 are
  faulty on this particular Uno. Moving those two buttons to D8 and D9 fixed it immediately.
  Worth remembering that "the code is wrong" is an assumption, not a diagnosis.
- **Buttons must bridge the centre gap of the breadboard.** Both legs on the same side of the gap
  shorts them out, and the symptom looks exactly like a code bug.
- **Every ground goes to a shared rail**, then one wire from the rail to the Arduino's GND — not
  a separate jumper per component.

---

## Next

- DS3231 RTC module so the daily reset doesn't depend on the host clock
- ESP8266 so it talks to the server over WiFi instead of USB
- Soldered perfboard and a project box instead of the breadboard
- A flashcard reader mode: show a card, play its audio, log easy/hard
