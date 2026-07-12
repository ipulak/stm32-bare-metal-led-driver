# Bare Metal Programming Guide

**Project:** STM32 Bare Metal LED Driver

**Target Board:** STM32 NUCLEO-F446RE

**Processor:** ARM Cortex-M4

**Programming Language:** Embedded C

**Programming Style:** Register-Level (Bare Metal)

---

# Table of Contents

1. What is Bare Metal Programming?
2. HAL vs Bare Metal
3. STM32 Memory Map
4. Bare Metal Programming Flow
5. Step 1 - Identify the Peripheral
6. Step 2 - Enable Peripheral Clock
7. Step 3 - Configure GPIO
8. Step 4 - Control the LED
9. Register-Level Programming
10. Why Volatile?
11. Common Mistakes
12. Interview Questions
13. Summary

---

# 1. What is Bare Metal Programming?

Bare Metal Programming means writing software that directly communicates with the hardware registers of the microcontroller.

Instead of using library APIs like:

```c
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_SET);
```

we directly access the GPIO registers.

Example:

```c
GPIOA_ODR |= (1<<5);
```

This provides a much deeper understanding of the hardware.

---

# 2. HAL vs Bare Metal

## HAL

```
Application

↓

HAL API

↓

HAL Driver

↓

Registers

↓

GPIO Hardware

↓

LED
```

Advantages

- Easy to use
- Faster development
- Portable

Disadvantages

- Larger code size
- Less control
- Slower execution

---

## Bare Metal

```
Application

↓

Registers

↓

GPIO Hardware

↓

LED
```

Advantages

- Maximum control
- Smaller code
- Faster execution
- Better interview preparation

Disadvantages

- Hardware dependent
- More difficult to learn

---

# 3. STM32 Memory Map

The STM32 stores every peripheral at a fixed memory address.

Example:

| Peripheral | Base Address |
|------------|--------------|
| GPIOA | 0x40020000 |
| RCC | 0x40023800 |

The CPU accesses these addresses directly.

---

# 4. Bare Metal Programming Flow

Every peripheral follows the same sequence.

```
Find Peripheral

↓

Find Base Address

↓

Enable Clock

↓

Configure Registers

↓

Enable Peripheral

↓

Read or Write Register

↓

Verify on Hardware
```

---

# 5. Step 1 - Identify the Peripheral

The on-board LED (LD2) is connected to

```
GPIO Port : A

Pin : 5
```

Therefore

```
GPIOA

Pin 5
```

---

# 6. Step 2 - Enable Peripheral Clock

Every peripheral requires a clock before it can be used.

```
CPU

↓

RCC

↓

GPIOA Clock Enabled

↓

GPIO Active
```

Register

```
RCC_AHB1ENR
```

Bit

```
GPIOAEN (Bit 0)
```

Code

```c
RCC_AHB1ENR |= (1<<0);
```

---

# 7. Step 3 - Configure GPIO

GPIO pins are configured using the MODER register.

Each GPIO pin occupies **2 bits**.

Example

```
PA5

Bits 11:10
```

Output Mode

```
01
```

Code

```c
GPIOA_MODER |= (1<<10);

GPIOA_MODER &= ~(1<<11);
```

---

# 8. Step 4 - Control the LED

The Output Data Register (ODR) controls the output level.

Register

```
GPIOA_ODR
```

LED ON

```c
GPIOA_ODR |= (1<<5);
```

LED OFF

```c
GPIOA_ODR &= ~(1<<5);
```

---

# 9. Register-Level Programming

The CPU writes directly to memory.

Example

```
GPIOA Base Address

0x40020000
```

ODR Offset

```
0x14
```

Final Register Address

```
0x40020014
```

Writing to this address changes the voltage on PA5.

---

# 10. Why Volatile?

Hardware registers can change at any time.

The compiler must never optimize them.

Example

```c
volatile unsigned int *GPIOA_ODR;
```

Without `volatile`, the compiler may cache values and produce incorrect behavior.

---

# 11. Common Mistakes

### Forgetting RCC Clock

GPIO will not work.

---

### Wrong Register Address

Always verify the base address and register offset from the Reference Manual.

---

### Wrong MODER Bits

Each GPIO pin occupies two bits.

For PA5

```
Bits 11:10
```

---

### Forgetting Volatile

Compiler optimization may prevent register updates.

---

# 12. Interview Questions

## What is Bare Metal Programming?

Writing software by directly accessing hardware registers without middleware or libraries.

---

## Why enable RCC first?

Because peripherals are clock-gated to reduce power consumption.

---

## Why does PA5 use bits 10 and 11?

Each GPIO pin occupies two bits in the MODER register.

For pin n

```
Bits

2n

2n+1
```

For PA5

```
10

11
```

---

## Why use volatile?

Hardware registers can change independently of the CPU. `volatile` forces every read and write to access the actual register.

---

## Difference between HAL and Bare Metal?

| HAL | Bare Metal |
|------|------------|
| Easy | Advanced |
| Larger Code | Smaller Code |
| Library Based | Register Based |
| Portable | Hardware Specific |

---

# 13. Summary

Bare Metal Programming always follows the same sequence.

```
Find Peripheral

↓

Find Base Address

↓

Enable Clock

↓

Configure Registers

↓

Write Register

↓

Verify on Hardware
```

This pattern applies to

- GPIO
- UART
- SPI
- I2C
- Timers
- DMA
- ADC
- Interrupts

Mastering this workflow provides a strong foundation for embedded firmware development and technical interviews.
