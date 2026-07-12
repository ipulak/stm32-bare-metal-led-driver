# Memory-Mapped I/O

**Project:** STM32 Bare Metal LED Driver

**Board:** STM32 NUCLEO-F446RE

**Processor:** ARM Cortex-M4

---

# Table of Contents

1. What is Memory-Mapped I/O?
2. Why Memory-Mapped I/O?
3. STM32 Memory Map
4. CPU View of Memory
5. Memory Regions
6. Peripheral Base Addresses
7. Register Offsets
8. Register Address Calculation
9. CPU to Peripheral Communication
10. Example: LED ON
11. Why Use Pointers?
12. Why Use `volatile`?
13. Common Mistakes
14. Interview Questions
15. Summary

---

# 1. What is Memory-Mapped I/O?

Memory-Mapped I/O is a technique where hardware peripherals are assigned fixed addresses in the processor's address space.

Instead of calling special hardware instructions, the CPU simply **reads from** or **writes to** these addresses.

Think of every peripheral as having its own "house address."

Example:

```
GPIOA

Address

0x40020000
```

When the CPU writes to this address, it is communicating with the GPIO hardware.

---

# 2. Why Memory-Mapped I/O?

Imagine you want to turn on a light in your house.

Instead of shouting to someone else, you walk to the wall switch and flip it directly.

Memory-Mapped I/O works the same way.

```
CPU

↓

Memory Address

↓

Peripheral Register

↓

Hardware Changes
```

This approach is:

- Fast
- Simple
- Efficient
- Uniform (memory and peripherals use the same instructions)

---

# 3. STM32 Memory Map

The STM32 address space is divided into regions.

```
+---------------------------+
| Flash Memory              |
| 0x08000000                |
+---------------------------+

| SRAM                      |
| 0x20000000                |
+---------------------------+

| Peripheral Registers      |
| 0x40000000                |
+---------------------------+

| Cortex-M System Registers |
| 0xE0000000                |
+---------------------------+
```

Each region has a specific purpose.

---

# 4. CPU View of Memory

The CPU does not know whether it is accessing RAM or a peripheral.

It only knows one thing:

> "I have an address to read or write."

```
CPU

↓

Address

↓

Memory System

↓

Flash / SRAM / Peripheral
```

The address determines the destination.

---

# 5. Memory Regions

## Flash

Stores the firmware.

Example:

```
main()

Interrupt Vector Table

Constants
```

---

## SRAM

Stores variables during program execution.

Example:

```c
int counter = 0;
```

---

## Peripheral Region

Contains hardware registers.

Examples:

- GPIO
- UART
- SPI
- I2C
- Timers
- RCC

---

# 6. Peripheral Base Addresses

Every peripheral has a unique base address.

| Peripheral | Base Address |
|------------|--------------|
| GPIOA | 0x40020000 |
| GPIOB | 0x40020400 |
| RCC | 0x40023800 |

The Reference Manual provides these addresses.

---

# 7. Register Offsets

Each peripheral contains multiple registers.

Example:

| Register | Offset |
|----------|--------|
| MODER | 0x00 |
| OTYPER | 0x04 |
| OSPEEDR | 0x08 |
| PUPDR | 0x0C |
| IDR | 0x10 |
| ODR | 0x14 |

The offset is added to the base address to locate the register.

---

# 8. Register Address Calculation

Example:

GPIOA Base Address

```
0x40020000
```

ODR Offset

```
0x14
```

Final Address

```
0x40020014
```

The CPU writes to **0x40020014** to change the GPIO output.

---

# 9. CPU to Peripheral Communication

```
CPU

↓

Address Bus

↓

AHB Bus

↓

GPIO Peripheral

↓

Output Register

↓

PA5

↓

LED
```

This entire sequence happens in hardware.

---

# 10. Example: LED ON

Code:

```c
GPIOA_ODR |= (1U << 5);
```

What happens internally?

```
CPU executes instruction

↓

Address = 0x40020014

↓

Write value

↓

GPIO Output Register updated

↓

PA5 becomes HIGH

↓

Current flows

↓

LED turns ON
```

---

# 11. Why Use Pointers?

A register address is just a number.

To access it in C, we convert it into a pointer.

Example:

```c
#define GPIOA_ODR (*(volatile unsigned int *)(GPIOA_BASE + 0x14))
```

This tells the compiler:

1. Calculate the register address.
2. Treat it as a pointer to a 32-bit register.
3. Read or write the value at that address.

---

# 12. Why Use `volatile`?

Hardware registers can change independently of your program.

Without `volatile`, the compiler may cache values or remove repeated reads.

Using `volatile` ensures every access goes directly to the hardware register.

---

# 13. Common Mistakes

- Using an incorrect base address.
- Using the wrong register offset.
- Forgetting to enable the peripheral clock.
- Forgetting `volatile`.
- Confusing Flash, SRAM, and peripheral memory.

---

# 14. Interview Questions

### What is Memory-Mapped I/O?

Memory-Mapped I/O is a technique where hardware peripherals are accessed through fixed memory addresses.

---

### Why use Memory-Mapped I/O?

It allows the CPU to access peripherals using the same load and store instructions used for normal memory.

---

### What is a Base Address?

The starting address of a peripheral's register block.

---

### What is a Register Offset?

The distance between the peripheral's base address and a specific register.

---

### How do you calculate a register address?

```
Register Address = Base Address + Register Offset
```

---

### Why use `volatile`?

Because hardware registers may change at any time, and the compiler must not optimize register accesses.

---

# 15. Summary

Memory-Mapped I/O is the foundation of embedded programming.

The complete workflow is:

```
Read Reference Manual

↓

Find Peripheral Base Address

↓

Find Register Offset

↓

Calculate Register Address

↓

Read or Write Register

↓

Hardware Responds
```

Once you understand Memory-Mapped I/O, the same principles apply to GPIO, UART, SPI, I²C, Timers, DMA, ADC, and other peripherals.
