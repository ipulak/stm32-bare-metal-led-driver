# STM32 Bare Metal LED Driver

A register-level (Bare Metal) LED control project developed using the STM32 NUCLEO-F446RE development board.

This project demonstrates how to control the on-board LED (LD2 connected to PA5) **without using STM32 HAL GPIO APIs**. Instead, the firmware directly accesses the STM32 peripheral registers.

---

## 🎯 Objective

The objective of this project is to understand how ARM Cortex-M microcontrollers interact with hardware at the register level.

Topics covered:

- ARM Cortex-M4 Fundamentals
- Memory Mapped I/O
- GPIO Register Programming
- RCC Clock Configuration
- Bare Metal Programming
- Bit Manipulation
- Register Address Calculation

---

## Hardware

- STM32 NUCLEO-F446RE
- STM32F446RE (ARM Cortex-M4)
- ST-LINK Debugger
- On-board LED (LD2)
- USB Cable

---

## Software

- STM32CubeIDE
- Embedded C
- Bare Metal Programming

---

## Project Structure

```
stm32-bare-metal-led-driver
│
├── Core/
├── Drivers/
├── Startup/
├── README.md
├── ARCHITECTURE.md
├── BARE_METAL_PROGRAMMING_GUIDE.md
└── .gitignore
```

---

## Project Architecture

```
Application

↓

main()

↓

Register Access

↓

GPIO Registers

↓

GPIO Peripheral

↓

PA5

↓

LD2 LED
```

---

## Program Flow

```
Power ON

↓

Reset_Handler()

↓

main()

↓

Enable RCC Clock

↓

Configure GPIOA

↓

Configure PA5 Output

↓

Write GPIOA_ODR

↓

LED ON
```

---

## Memory Map

| Peripheral | Address |
|------------|----------|
| GPIOA | 0x40020000 |
| RCC | 0x40023800 |

---

## Registers Used

- RCC_AHB1ENR
- GPIOA_MODER
- GPIOA_ODR

---

## Learning Outcomes

After completing this project, I understood:

- Memory Mapped IO
- Register Programming
- GPIO Architecture
- RCC Clock System
- ARM Cortex-M4 Peripheral Access
- Bit Manipulation

---

## Future Improvements

- LED Blink
- Button Driver
- Interrupts
- UART Driver
- Timer Driver
- SPI Driver
- I2C Driver

---

## References

- STM32F446 Reference Manual (RM0390)
- STM32F446 Datasheet
- ARM Cortex-M4 Technical Reference Manual