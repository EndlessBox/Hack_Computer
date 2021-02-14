                                             ### Hack_Computer

This is my generale purpose, 16bits computer, that was the result creating elementary logic gates, then use them to build an ALU(Arithmetic and Logical Unit) and finaly
 a RAM Chip.

This computer use the "Harverd Architecture" which is a slightly modified version of "Von Neumann Architecture", that separate the executable code, and other Ram Data,
which allow us to run a "fetch" and "execution" instructions in the same CPU Clock.

In the original "Von Neumann Architecture", the executable code and the Ram Data were both stored at one place, and with the same bus's connection to the CPU,
and this is a limitation, cause we can fetch an instruction and execute it in the same cycle.

This computer is capable of handling Keyboard interactions, and screen plotting. This IO management was made by allocating and mapping a space of Memory 
to each IO devices, then we follow the Protocol given to execute, we can add as much as our memory allow of IO devices.

## Hack Machine Laguage Specification

# Memory Address Spaces
The Hack programmer is aware of two distinct address spaces: an (instruction memory) and a (data memory). 
Both memories are 16-bit wide and have a 15-bit address space, meaning that the maximum addressable size of each memory is 32K 16-bit words.
The CPU can only execute programs that reside in the instruction memory. The instruction memory is a read-only device, and programs are loaded into it using some exogenous means.
For example, the instruction memory can be implemented in a ROM chip that is pre-burned with the required program. Loading a new program is done by replacing the entire ROM chip,
similar to replacing a cart ridge in a game console. 
In order to simulate this operation, hardware simulators of the Hack plat-form must provide a means to load the instruction memory from a text file contain-ing a machine language program.


# Registers 
The Hack programmer is aware of two 16-bit registers called D and A. These registers can be manipulated explicitly by arithmetic and logical instructions
like A=D-1 or D=!A (where ‘‘!’’ means a 16-bit Not operation). While D is used solely to store data values, A doubles as both a data register and an address register.
That is to say, depending on the instruction context, the contents of A can be inter-preted either as a data value, or as an address in the data memory, or as an address in the instruction memory,
as is now explain.
First, the A register can be used to facilitate direct access to the data memory(which, from now on, will be often referred to as ‘‘memory’’). Since Hack instructions are 16-bit wide,
and since addresses are specified using 15 bits, it is impossible to pack both an operation code and an address in one instruction.
Thus, the syntaxof the Hack language mandates that memory access instructions operate on an implicit memory location labeled ‘‘M’’, for example, D=M+1.
In order to resolve this address, the convention is that M always refers to the memory word whose address is the current value of the A register.
For example, if we want to effect the operation D = Memory[516] - 1, we have to use one instruction to set the A register to 516, and a subsequent instruction to specify D=M-1.
In addition, the hardworking A register is also used to facilitate direct access tothe instruction memory. 
Similar to the memory access convention, Hack jump instructions do not specify a particular address.
Instead, the convention is that any jump operation always effects a jump to the instruction located in the memory word addressed by A.
Thus, if we want to effect the operation 'goto 35', we use one in-struction to set A to 35, and a second instruction to code a go to command, without specifying an address.
This sequence causes the computer to fetch the instruction located in Instruction Memory[35] in the next clock cycle.


# The A-Instruction 
The A-instruction is used to set the A register to a 15-bit value:
      Example : @523 <==> [0]00001000001011
              : [0] = specify the instruction is A-Instruction.

# The C-Instruction 
The C-instruction is the programming workhorse of the Hack platform—the in-struction that gets almost everything done. 
The instruction code is a specificationthat answers three questions: 
(a) what to compute, (b) where to store the computedvalue, and (c) what to do next? 
Along with the A-instruction, these specifications determine all the possible operations of the computer.
      
Example: destination = computation;jump
       : MD=M+1 = [[1]11][1 1101 11][01 1][000]
       : [1] = specify the instruction is C-Instruction
       : [[]11] = those bits are ignored.
       : [1 1101 11] = computation bits.
       : [01 1] = destination bits.
       : [000] = jump bits.
       : another example : ==> @200 | D;JEQ

