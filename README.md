# Ultrasonic Radar with Joystick Override

A little Arduino project that turns an HC-SR04 + servo into a radar scanner. It sweeps back and forth on its own, but if you hold the joystick button it switches to manual mode so you can aim it yourself. If something gets too close, the buzzer and LED kick in.

Started this as a "let's see if we can build something that actually looks cool on a breadboard" project. Turned out pretty fun.

## What it does

- Servo sweeps the ultrasonic sensor from 0° to 180° and back, like a little radar dish
- Click the joystick button to flip between **auto-sweep** and **manual (joystick) mode**
- In joystick mode, push the stick left/right to steer the servo — speed scales with how far you push it, not just on/off
- If it detects something closer than 20cm, LED turns on and buzzer sounds until the object clears
- Angle + distance readings get printed over Serial (9600 baud) so you can hook it up to a processing/plotting sketch if you want the classic sweeping radar-screen visual

## Hardware

- Arduino (Uno/Nano, anything with enough pins works)
- SG90 (or similar) micro servo
- HC-SR04 ultrasonic distance sensor
- Active buzzer
- LED + resistor
- Joystick module (just using the button + X axis here, Y axis isn't used)

## Wiring

| Component        | Arduino Pin |
|-------------------|:-----------:|
| Servo signal       | D9  |
| HC-SR04 TRIG        | D6  |
| HC-SR04 ECHO        | D7  |
| Buzzer              | D5  |
| LED                 | D4  |
| Joystick button (SW)| D2  |
| Joystick X axis     | A0  |

Joystick button is wired with `INPUT_PULLUP`, so it just needs to go to GND when pressed — no external resistor needed.

## Notes / gotchas

- `pulseIn()` has a 30ms timeout on the echo pin so it doesn't hang forever if nothing's in range — if it times out, distance comes back as `-1` and gets ignored.
- The button toggle has a basic debounce (300ms) since without it the mode was flipping like twice per press.
- Joystick deadzone is set to 60 around center (512) — my joystick module drifts a little when idle, so this stops it from crawling on its own. Might need to tweak this depending on your specific module.
- The `tone()` call in `alarmOn()` uses a 5 second duration just so it doesn't need to be re-triggered every loop — it naturally gets silenced by `alarmOff()` once the object clears anyway.
- Sweep step is 5° per loop with a 30ms delay — feels about right for a smooth-looking sweep without the servo sounding like it's struggling. Bigger steps make it twitchy.

## Ideas for later

- Add a second mode where it locks onto the closest object instead of just alarming
- Try feeding the Serial output into a Processing sketch for the classic green radar-screen look
- Maybe average a few distance readings to smooth out noisy pings

## License

Do whatever you want with this, it's just a hobby sketch.
