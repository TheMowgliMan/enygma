# Enygma
Enygma is an emulated virtual machine on a custom 32-bit architecture. It is  very simple to be able to run in simple environments, with no regard paid to difficulty in hardware implementation.

## Functional Details
### Program Counter
The program counter is a 32-bit register that holds the address. It starts at 0x00000100 and increments by one after every instruction unless otherwise noted. It is unsigned; thus, it cannot have two's complement negativity.
### Specialty Registers
ACC is a special register placed on one side of the top of the ALU used to accumulate data.

TMP is a special register placed under the ALU for the same purpose.

DBUS is the data bus; while not technically a register, a value can be 'latched' to the bus.

FLG is the flag register. The bits:
1. Carry
### General Registers
Registers are big-endian and use two's complement arithmetic.
### Instruction Set

