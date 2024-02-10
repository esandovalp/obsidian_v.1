#CS 
![[Pasted image 20240210123851.png]]
## The Core Components

- **Control Unit (CU):** The boss of the CPU. It fetches instructions from memory, decodes them (figures out what they mean), and then orchestrates the other components to carry out those instructions.
- **Arithmetic Logic Unit (ALU):** The mathematical and logical workhorse. It performs basic arithmetic operations (like addition and subtraction) and logical comparisons (like greater than, less than, or equal to).
- **Registers:** Super-fast, tiny memory storage locations directly within the CPU. They hold data the CPU is immediately working with, instructions, and memory addresses. Some important registers include:
- **Cache Memory:** Multiple levels (L1, L2, and sometimes L3) of extremely fast memory built directly into the CPU. Cache acts as a buffer between the registers and main memory, holding frequently used data for quick access.

## How the CPU Works: The Fetch-Decode-Execute Cycle

1. **Fetch:** The Control Unit grabs the next instruction from memory (using the address in the Program Counter) and places it in the Instruction Register.
2. **Decode:** The Control Unit figures out what the instruction in the Instruction Register is telling the CPU to do.
3. **Execute:** The Control Unit directs the ALU, registers, and other CPU components to carry out the instruction. This might involve:
    - Doing math or logical operations (ALU)
    - Loading data from memory (Registers, MAR, MDR)
    - Saving results back to memory (Registers, MAR, MDR)
    - Jumping to a different part of the program (changing the PC)

## Other Important CPU Features

- **Clock:** A tiny oscillator that sets the pace for the CPU, synchronizing all its operations. The clock speed (measured in Hertz - Hz) is one indicator of a CPU's processing power.
- **Cache Memory:** A small but super-fast type of memory built into the CPU. It stores frequently used instructions and data for lightning-quick access, minimizing trips to slower main memory.
- **Pipelining:** A technique where the CPU starts working on a new instruction before the previous one is completely finished, allowing multiple instructions to be processed simultaneously.
- **Multiple Cores:** Modern CPUs often have multiple processing cores within a single chip. These cores can genuinely work on tasks in parallel, boosting overall performance.