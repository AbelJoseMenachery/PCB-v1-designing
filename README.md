# PCB-v1-designing
ATtiny85 LED Debounce Project
 Author: Abel Jose
 ## Project Overview
 This project uses an ATtiny85 microcontroller to control an LED with a debounced input button. It
 was designed entirely using KiCad and includes both schematic and PCB layout.
 ## Features- Debounce logic in software to handle button bounce.- Minimalist design for easy replication and understanding.- LED toggles on button press using a single digital input.
 ## Getting Started
 ### Requirements- ATtiny85 (or ATtiny20, ATtiny45, depending on your variant)- Tactile push button- LED- 220-ohm resistor (for LED)- KiCad (for viewing/modifying schematic and PCB)- USBASP or similar programmer (for uploading code)
 ### Software Debounce Logic
 This project uses a simple debounce function to prevent false triggering due to noisy button signals.
 The firmware waits for a stable input state before registering a press.
 Page 1
ATtiny85 LED Debounce Project
 ```c
 #define F_CPU 1000000UL
 #include <avr/io.h>
 #include <util/delay.h>
 #define BUTTON PB0
 #define LED PB1
 uint8_t debounce() {
    if (!(PINB & (1 << BUTTON))) {
        _delay_ms(20);
        if (!(PINB & (1 << BUTTON))) return 1;
    }
    return 0;
 }
 int main(void) {
    DDRB |= (1 << LED);
    PORTB |= (1 << BUTTON); // Pull-up
    while (1) {
        if (debounce()) {
            PORTB ^= (1 << LED);
            while (!(PINB & (1 << BUTTON))); // Wait for release
 Page 2
            _delay_ms(50);
 ATtiny85 LED Debounce Project
        }
    }
 }
 ```
 ## PCB Design Notes- Make sure all net connections in KiCad are valid.- Run DRC and fix "missing connection" errors by adjusting track overlaps or vias.- Verify diode orientation and LED polarity.
 ## How to Upload to ATtiny85
 1. Compile code using `avr-gcc` or Arduino IDE with ATtiny85 support.
 2. Burn using USBASP and `avrdude`:
   ```bash
   avrdude -c usbasp -p t85 -U flash:w:main.hex
   ```--
Happy Hacking! *
 Page 3
