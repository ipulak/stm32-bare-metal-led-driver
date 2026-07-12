# Architecture

## Overview

This project controls the on-board LED (LD2) using direct register programming.

Unlike HAL programming, the firmware directly writes to the STM32 peripheral registers.

---

# High Level Architecture

```
                 User Application

                        │

                        ▼

                    main()

                        │

                        ▼

              Register-Level Driver

                        │

        ┌───────────────┴───────────────┐

        ▼                               ▼

      RCC                           GPIOA

        │                               │

Enable Peripheral Clock          Configure PA5

        │                               │

        └───────────────┬───────────────┘

                        ▼

                 GPIO Output Driver

                        ▼

                    LD2 LED
```

---

# Software Architecture

```
Application

↓

main()

↓

Register Definitions

↓

Memory Mapped Registers

↓

Peripheral Hardware

↓

LED
```

---

# Hardware Architecture

```
USB

↓

ST-LINK

↓

STM32F446RE

↓

GPIOA

↓

PA5

↓

LED
```

---

# GPIO Initialization

```
Enable RCC Clock

↓

GPIOA Clock Enabled

↓

Configure PA5 Mode

↓

Output Mode

↓

Write ODR Register

↓

LED ON
```

---

# Register Flow

```
RCC_AHB1ENR

↓

GPIOA Clock Enabled

↓

GPIOA_MODER

↓

PA5 Output

↓

GPIOA_ODR

↓

Bit5 = 1

↓

LED ON
```

---

# Register Access Sequence

```
CPU

↓

AHB Bus

↓

RCC

↓

GPIOA

↓

GPIO Register

↓

PA5

↓

LED
```

---

# Why Bare Metal?

Advantages

- Better understanding of hardware
- Faster execution
- Smaller code size
- Useful for interviews
- No dependency on HAL

Disadvantages

- More complex
- Hardware specific
- Requires Reference Manual

---

# Lessons Learned

This project demonstrates that controlling an LED requires much more than writing a single line of code.

The complete sequence is:

1. Enable peripheral clock.
2. Configure GPIO mode.
3. Configure output.
4. Write to output register.

Understanding these steps provides the foundation for learning UART, SPI, I2C, Timers, DMA and Interrupts.

---

# Next Project

Register-Level Button Driver

Topics

- GPIO Input
- IDR Register
- Polling
- Debouncing
- Button Controlled LED