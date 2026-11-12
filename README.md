# Cryptographic Decoder

A feature-rich, menu-driven USB-serial cryptographic and telecommunications tool designed for the **Raspberry Pi Pico 2 (RP2350)** using the official Pico SDK. 

This firmware combines analog audio signal processing (Morse code decoding), hardware-driven PWM audio transmission, and classical/modern cryptography—feeding all text transformation results directly back into audio Morse code via a buzzer.

---

## Features

1. **Acoustic Morse Decoder (`ADC`)**: Continuously samples an analog microphone input, performs real-time AC peak-to-peak amplitude analysis, filters background noise, parses dot/dash durations (tuned for 15 WPM), and prints decoded characters to the serial console.
2. **PWM Morse Transmitter (`PWM Buzzer`)**: Takes any user-entered string and plays it back audibly via a piezoelectric buzzer driven by hardware Pulse Width Modulation (PWM).
3. **Base64 Encoder & Decoder**: Fully handles binary-to-text ASCII encoding/decoding with proper padding calculation (`=`) and sign-extension protection.
4. **Caesar Shift & ROTX Ciphers**: Case-preserving substitution ciphers supporting arbitrary shift or rotation parameters ($X$) with wrap-around logic.
5. **64-bit Textbook RSA**: Secure modular arithmetic routines (`mul_mod` using overflow-safe double-and-add multiplication and `power_mod` via square-and-multiply) to encrypt messages ($C = M^e \pmod N$) and decrypt ciphertexts ($M = C^d \pmod N$).

---

## Hardware Pinout & Wiring

| Component | Pico 2 GPIO Pin | Pico Pin (Physical) | Description |
| :--- | :--- | :--- | :--- |
| **Microphone Output** | `GPIO 26` | Pin 31 (`ADC0`) | Analog audio input for the Morse decoder |
| **Buzzer Control** | `GPIO 15` | Pin 20 (`PWM 7B`) | PWM-driven audio output for Morse transmission |

* **Microphone Circuit:** Connect your microphone module's analog out to `GPIO26`. Ensure proper DC biasing (typically $1.65\text{V}$ or powered via $3.3\text{V}$) so the AC audio signal oscillates cleanly within the `0`-`4095` 12-bit ADC range.
* **Buzzer Circuit:** Connect the positive terminal of an active/passive buzzer or speaker driver circuit to `GPIO15`, and the negative terminal to GND (use a current-limiting resistor or transistor driver depending on your buzzer spec).

---

## Software Architecture & Dependencies

* **Platform:** Raspberry Pi Pico / Pico 2 (`RP2040` / `RP2350`)
* **SDK:** Raspberry Pi Pico SDK (`pico/stdlib.h`, `hardware/adc.h`, `hardware/pwm.h`)
* **Communication:** USB Serial (stdio over USB) at `115200` baud.

---

## Building and Flashing

### Prerequisites
1. Install the [Raspberry Pi Pico SDK](https://github.com/raspberrypi/pico-sdk) and ensure your `PICO_SDK_PATH` environment variable is correctly configured.
2. Ensure you have `cmake`, `gcc-arm-none-eabi`, and build tools installed.

### Build Instructions
1. Create a `CMakeLists.txt` file in your project directory linking against `pico_stdlib`, `hardware_adc`, and `hardware_pwm`.
2. Run the following commands to compile the firmware:

```bash
mkdir build
cd build
cmake ..
make
