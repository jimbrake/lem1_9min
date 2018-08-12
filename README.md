# lem1_9min
Logic Emulation Machine
Four new variants of LEM1_9 are now implemented.  There is also the untested design LEM16_18 which combines a 16-bit accumulator
ISA with bit field access to memory.

LEM1_9 
Call/Return/Conditional branches implemented along with a return address stack. 
Parameterized program memory size, data size and return stack size. Data size limited to 32 bits. Instructions are 9 bits. 

LEM1_9ptr 
Adds four index registers to LEM1_9. Instruction set no longer uses absolute addressing. Indexing modes are indirect, indirect post increment, indirect pre decrement and indirect with offset. 
Parameterized program memory size, data size and return stack size. Data size limited to 512 bits. Instructions are 9/18 bits. 

LEM4_9 
Widens data to four bits, a single digit. Both binary and BCD arithmetic. 
Call/Return/Conditional branches implemented along with a return address stack. 
Parameterized program memory size, data size and return stack size. Data size limited to 32 digits. Instructions are 9 bits. 

LEM4_9ptr 
Widens data to four bits, a single digit. Both binary and BCD arithmetic. 
Adds four index registers to LEM4_9. Instruction set no longer uses absolute addressing. Indexing modes are indirect, indirect post increment, indirect pre decrement and indirect with offset. 
Parameterized program memory size, data size and return stack size. Data size limited to 512 digits. Instructions are 9/18 bits. 

LEM1_9min is now considered obsolete. 
Three bit op-code and six bit absolute address. Single bit at a time operations. No subroutines or branches. 
Execution starts at location zero and continues until a halt instruction. This repeats after the next start pulse. 

Description: 
Small soft core processors that operate on a single bit (or four bits) of data at a time. Can be used for logic simulation, software interrupt handlers, low resource soft-core processing and BCD calculators. 

Performance 
Processor Device LUT count Fmax(in MHz) 
LEM1_9min  - -Kintex-7-3 - - - - - -39 - - - - 350 - (in practice a ~20 bit counter needed and I/O ports adding ~40 LUTs) 
LEM1_9 - - - -Kintex-7-3 - - - - - -75 - - - - 171 - (currently limited to 32 bits of data RAM) 
LEM1_9ptr - - Kintex-7-3 - - - - - 105 - - - - 170 - (limited to 512 bits of data RAM) 
LEM4_9 - - - -Kintex-7-3 - - - - - 144 - - - - 195 - (currently limited to 32 digits of data RAM) 
LEM4_9ptr - - Kintex-7-3 - - - - - 151 - - - - 145 - (limited to 512 digits of data RAM) 
(all instructions execute in one clock cycle) 
(all versions use half of a block RAM for program memory): 
(a two stage pipeline increases Fmax) 

Implementation: 

The design regimen is single pipeline stage micro-controller. Instruction fetch via block RAM read. Data fetch via distributed RAM asynchronous read. Data store via synchronous write coinciding with next instruction fetch (and register updates). Two word instructions execute in one clock via using both ports of the block RAM, the second port providing the 2nd instruction word(9-bits). 

LEM1_9min files: 
Lem1_9min_defs.vhd - - - Instruction set 
Lem1_9min.vhd - - - - - - Core 
Lem1_9min_hw.vhd - - - - Test harness for Xilinx/Digilent Spartan-3 board 
Form1.cs - - - - - - - - - - C# source for “assembler” 
Lem1_9min_asm.csproj - -C# project for lem1_9min assembler, uses Windows form 
