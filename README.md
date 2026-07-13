# Ultrasonic Radar with Joystick Override

A little Arduino project that turns an HC-SR04 + servo into a radar scanner. It sweeps back and forth on its own, but if you hold the joystick button it switches to manual mode so you can aim it yourself. If something gets too close, the LED kicks in.

Started this as a "let's see if we can build something that actually looks cool on a breadboard" project. Turned out pretty fun.

## What it does

- Servo sweeps the ultrasonic sensor from 0° to 180° and back, like a little radar dish
- Click the joystick button to flip between **auto-sweep** and **manual (joystick) mode**
- In joystick mode, push the stick left/right to steer the servo — speed scales with how far you push it, not just on/off
- If it detects something closer than 20cm, LED turns on until the object clears
- Angle + distance readings get printed over Serial (9600 baud) so you can hook it up to a processing/plotting sketch if you want the classic sweeping radar-screen visual

## Hardware

- Arduino (Uno/Nano, anything with enough pins works)
- SG90 (or similar) micro servo
- HC-SR04 ultrasonic distance sensor
- Active buzzer
- LED + resistor
- Joystick module (just using the button + X axis here, Y axis isn't used)

## Wiring
This is the full wiring list
This is with a breadbard, because a arduino dosen't have enough GND and 5V ports

| Component        | Arduino Pin |
|-------------------|:-----------:|
| Arduino GND         |-Rail|
| Arduino 5V          |+Rail|
| Servo signal        | D9  |
| Servo GND           |-Rail|
| HC-SR04 TRIG        | D6  |
| HC-SR04 ECHO        | D7  |
| HC-SR04 GND         |-Rail|
| HC-SR04 VCC         |+Rail|
| Buzzer +            | D5  |
| Buzzer -            |-Rail|
| LED Cathod          | D4  |
|LED Anode->resistor->|-Rail|
| Joystick button (SW)| D2  |
| Joystick X axis     | A0  |
| Joystick GND        |-Rail|
| Joystick +5V        |+Rail|

Joystick button is wired with `INPUT_PULLUP`, so it just needs to go to GND when pressed — no external resistor needed.

## Notes / gotchas

- `pulseIn()` has a 30ms timeout on the echo pin so it doesn't hang forever if nothing's in range — if it times out, distance comes back as `-1` and gets ignored.
- The button toggle has a basic debounce (300ms) since without it the mode was flipping like twice per press.
- Joystick deadzone is set to 60 around center (512) — my joystick module drifts a little when idle, so this stops it from crawling on its own. Might need to tweak this depending on your specific module.
- The `tone()` call in `alarmOn()` uses a 5 second duration just so it doesn't need to be re-triggered every loop — it naturally gets silenced by `alarmOff()` once the object clears anyway.
- Sweep step is 5° per loop with a 30ms delay — feels about right for a smooth-looking sweep without the servo sounding like it's struggling. Bigger steps make it twitchy.
- If you want to potentialy add more sensors or modules to this project of mine, try to buy the arduino r3 mega on amazzon, it comes with extra modules and has way more digital pins than a arduino uno/nano.
- I wrote down a comment, so if someone uses the code, it should show were you can adjust/change the detecting distance

## Ideas for later

- Add a second mode where it locks onto the closest object instead of just alarming
- Try feeding the Serial output into a Processing sketch for the classic green radar-screen look
- Maybe average a few distance readings to smooth out noisy pings
- make a human only detection system

## Issues

If you are having problems look at this list:
- If your adruino isn't turning on, check if you messed up the GND and/or 5V connections, this normally overheates the arduino and makes it stop working
- If you downloaded the code, but don't want to connect it to your device, first verify the code and then upload, then finally use a barreljack(should be provided) to make it work without USB cable, a 9V battery won't work.
- Comment any other problems you have.

## License

Do whatever you want with this, it's just a hobby sketch.

## Pictures and a Video
<img width="1600" height="1200" alt="WhatsApp Image 2026-07-13 at 2 22 45 PM" src="https://github.com/user-attachments/assets/03b14aad-6b98-4726-bbc8-8e5d306a1284" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-07-13 at 2 22 45 PM" src="https://github.com/user-attachments/assets/1c729496-ea13-4bd4-bd2f-aa6cb7deb74a" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-07-13 at 2 22 45 PM" src="https://github.com/user-attachments/assets/b3b6831c-84c5-449b-a238-053749e09089" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-07-13 at 2 22 45 PM" src="https://github.com/user-attachments/assets/a965e64f-878c-4d3b-a62f-8a81abcf57f5" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-07-13 at 2 22 45 PM" src="https://github.com/user-attachments/assets/54b401ed-be12-42f6-8a31-b6e68a1e1cb3" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-07-13 at 2 22 45 PM" src="https://github.com/user-attachments/assets/6058a23e-00ed-4e9d-8f74-8009b23241dc" />
<img width="1600" height="1200" alt="WhatsApp Image 2026-07-13 at 2 22 45 PM" src="https://github.com/user-attachments/assets/def92c7d-7947-4624-b1a7-9294a26a30fc" />


https://github.com/user-attachments/assets/446aee7b-6975-445f-88c4-1dae998f8a5b

