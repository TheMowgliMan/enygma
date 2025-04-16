# Enygma
Enygma is an emulated virtual machine on a custom 32-bit architecture. It is  very simple to be able to run in simple environments, with no regard paid to difficulty in hardware implementation.

## Functional Details
### Program Counter
The program counter is a 32-bit register that holds the address. It starts at 0x00000100 and increments by one after every instruction unless otherwise noted. It is unsigned; thus, it cannot have two's complement negativity.
### General Registers
Registers are big-endian and use two's complement arithmetic.
### Instruction Set

