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
Registers are big-endian and use two's complement arithmetic. There are 32 full-width general registers (denoted with register IDs R000-R01F), 64 16-bit registers subdivided from the above (denoted R020-R05F), and 128 8-bit registers subdivided from the above (denoted R060-R0DF).

Sixteen-bit registers are subdivided as follows:
R000(R020, R021), R001(R022, R023), etc

Eight-bit registers are subdivided as follows:
R000(R020(R060, R061), R021(R062, R063)), R001(R022(R064, R065), R023(R066, R067)), etc
### Instruction Set
Instructions are formatted like so:
 - Bits 0 through 15: instruction
 - Bits 16-22: flags
 - Bits 23-31: register

The flags are used to change the function of an instruction. What they mean, if anything, is defined by the instruction.
#### The ALU Instruction (0b1xxxxxxxxxxxxxxxxx)
##### ADD (0x8000)
Adds ACC and DBUS together and puts the result in TMP.
#### HLT (0x0000)
Halts the VM until any interrupt bit is 1.
#### DBUS (0x0001)
Puts the data in the next memory address onto the bus.
#### DACC (0x0002)
Puts the data in the next memory address into ACC.
#### DGRF (0x0003)
Puts the data in the next memory address into a general register. If a full-width register is used, it puts the whole word into the register. If subdivided, it puts the part of the word aligned with the register in the register (e.g. R021 would load bits 8-15, starting from 0).
