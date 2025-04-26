# Enygma
Enygma is an emulated virtual machine on a custom 32-bit architecture. It is very simple to be able to run in simple environments, with no regard paid to the difficulty of a hardware implementation.

## Functional Details
### Program Counter
The program counter is a 32-bit register that holds the address. It starts at 0x0000A000 and increments by one after every instruction unless otherwise noted. It is unsigned; thus, it cannot have two's complement negativity.
### Specialty Registers
ACC is a special register placed on one side of the top of the ALU used to accumulate data.

TMP is a special register placed under the ALU for the same purpose.

DBUS is the data bus; while not technically a register, a value can be 'latched' to the bus.

FLG is the flag register. The bits:
1. Carry: indicates a carry is left over from an ADD instruction; 1 if so, 0 otherwise.
2. Zero: indicates the result of an instruction was zero; 1 if so, 0 otherwise.
3. Sign: indicates if the result of an instruction was negative; 1 if so, 0 otherwise.
### General Registers
Registers are big-endian and use two's complement arithmetic. There are 32 full-width general registers (denoted with register IDs R000-R01F), 64 16-bit registers subdivided from the above (denoted R020-R05F), and 128 8-bit registers subdivided from the above (denoted R060-R0DF).

Sixteen-bit registers are subdivided as follows:
R000(R020, R021), R001(R022, R023), etc

Eight-bit registers are subdivided as follows:
R000(R020(R060, R061), R021(R062, R063)), R001(R022(R064, R065), R023(R066, R067)), etc

Note: the first eight 32-bit general registers (R000-R007) and the registers subdivided from them do not reset with an interrupt, but the others are stored and set to zero, to be restored with a RET instruction.
### Instruction Set
Instructions are formatted like so:
 - Bits 0 through 15: instruction
 - Bits 16-22: flags
 - Bits 23-31: register

The flags are used to change the function of an instruction. What they mean, if anything, is defined by the instruction.
#### The ALU Instruction (0b1xxxxxxxxxxxxxxxxx)
##### ADD (0x8000)
 - Version >= Enygma-v1

Adds ACC and DBUS together and puts the result in TMP.
#### HLT (0x0000)
 - Version >= Enygma-v1

Halts the VM until any interrupt bit is 1.
#### HASFPU (0x0001)
 - Version >= Enygma-v1

Sets R000 to 1 if there is a floating-point unit, and to 0 if not.
#### SYSVER (0x0002)
 - Version >= Enygma-v1

Sets R000 to the version of Enygma-VM.
 - Enygma-v1: 0x0000
#### DATA (0x0003)
 - Version >= Enygma-v1

Puts the data in the next memory address onto the bus.
#### BGRF (0x0004)
 - Version >= Enygma-v1

Sets a general register to the value on the bus. If a subdivided register is used and flag bit 16 is zero, the part of the word aligned with the register is put in the register. If flag bit 16 is one and a 16-bit register is used, then the right half (16-31) of the bus is used if flag bit 17 is one, and the other half if flag bit 17 is zero. If an eight-bit register is used, then the rightmost quarter of the bus is used if flag bits 17 and 18 are both one, the middle-right quarter if 17 is one and 18 is zero, etc.
#### BACC (0x0005)
 - Version >= Enygma-v1

Sets ACC to the value on the bus.
#### TBUS (0x0006)
 - Version >= Enygma-v1

Sets the bus to the value in TMP.
#### RBUS (0x0007)
 - Version >= Enygma-v1

Sets the bus to the value in a general register. Always puts it on the bottom of the bus.
#### LOAD (0x0008)
 - Version >= Enygma-v1

Loads a word from memory based on the address of a full-width register and puts it on the bus. Throws an exception if the register is not full-width.
#### STOR (0x0009)
 - Version >= Enygma-v1

Takes the data on the bus and stores it in the memory location given in a full width register. Throws an exception if the register is not full-width.
