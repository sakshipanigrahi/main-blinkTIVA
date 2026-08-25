# Lab 2 Report

**Sakshi Panigrahi**
Electronic Systems Engineering, IISc

## Introduction

- Implements RGB LED control on the TM4C123GH6PM microcontroller using two push-button switches, SW1 and SW2, connected on Port F.
- **Program 1** — sets the LED colour instantly based on which switches are pressed.
- **Program 2** — makes the LED blink continuously, with SW1 cycling through a fixed set of blink rates and SW2 cycling through a fixed set of colours.

## Hardware Used and GPIO Configuration

| Pin | Role |
|-----|------|
| PF1 | Red — output, drives RGB LED |
| PF2 | Blue — output, drives RGB LED |
| PF3 | Green — output, drives RGB LED |
| PF4 | SW1 — input |
| PF0 | SW2 — input |

- Both switches are active-low, so internal pull-up resistors are enabled (`GPIO_PORTF_PUR_R = 0x11`) to hold the pins high when unpressed.
- Pressing pulls the pin to logic 0.
- PF0 is shared with the NMI function, so it is unlocked using `GPIO_PORTF_LOCK_R` and `GPIO_PORTF_CR_R` before use.
- `GPIO_PORTF_DIR_R` sets the LED pins as outputs and switch pins as inputs.
- `GPIO_PORTF_DEN_R` enables digital mode on all five pins.
- White is produced by driving all three colour pins high together (PF1+PF2+PF3).

## Program 1: LED Colour Based on Switch State

- Depends on SW1 and SW2 and sets the LED colour based on the combination read.
- Controls the RGB LED on Port F using two switches, SW1 (PF4) and SW2 (PF0).
- First turns on power to Port F, then unlocks pin PF0 as it is normally protected.
- Sets PF1, PF2, PF3 as output (for LEDs) and PF0, PF4 as input (for switches), using pull-up resistors so they default to HIGH when not pressed.
- Inside `while(1)`, reads both switches and checks four cases:

| SW1 | SW2 | Value written | Colour |
|-----|-----|----------------|--------|
| Pressed | Pressed | 0x0E | Red + Blue + Green = White |
| Pressed | Not pressed | 0x04 | Blue |
| Not pressed | Pressed | 0x08 | Green |
| Not pressed | Not pressed | 0x02 | Red |

- The LED updates in real time as switches are pressed and released.

## Program 2: Blinking LED with Rate and Colour Cycling

- Blinks the LED continuously using two fixed arrays defined in advance.
- SW1 changes the blink rate and SW2 changes the blink colour, both while the LED is on.

| Array | Values | Cycled by |
|-------|--------|-----------|
| colours[4] | {Red, Green, Blue, White} | colorIndex |
| rates[3] | {1000, 500, 250} | rateIndex |

- A press of SW1 increments `rateIndex` by one.
- A press of SW2 increments `colorIndex` by one.
- Both indices cycle back to zero once they pass the last index, so values move through a fixed set repeatedly.
- The loop counts `i` up to `delayLimit` (= `rates[rateIndex]`) — more counts means longer wait.
- Switches are checked every loop, so presses are detected promptly, but a press mid-blink only takes effect from the next phase onward.

## Conclusion

- Both programs demonstrate core embedded GPIO concepts on the TM4C123GH6PM:
  - Reading active-low switch inputs with pull-up resistors
  - Unlocking the protected PF0 pin
  - Driving an RGB LED
- **Program 1** reads two switches directly and maps their combination to a fixed LED colour (Red/Blue/Green/White) in real time with no timing involved.
- **Program 2** builds on this: SW1 and SW2 no longer pick a colour directly but instead step through arrays (`rates[]` and `colors[]`) — each press equals one step.
