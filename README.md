# VLSI Physical Design for ASICs
## Objective
The objective of VLSI (Very Large Scale Integration) physical design for ASICs (Application-Specific Integrated Circuits) is to transform a logical design description (RTL - Register Transfer Level) into a physical layout that can be fabricated as an integrated circuit. This involves translating the high-level functional representation of the circuit into a physical implementation that meets design constraints, performance targets, and manufacturability requirements.

# SKILL OUTCOMES
+ Architectural Design
+ RTL Design / Behavioral Modeling
+ Floorplanning
+ placement
+ clock Tree Synthesis
+ Routing

# INSTALLATION
+ OS: Ubuntu 20.04 using wsl2
+ prerequisites -> gcc, build-essential, boostlib, and run `sudo apt-get update`
+ download `https://github.com/kunalg123/riscv_workshop_collaterals/blob/master/run.sh`
+ run the commands line by line while checking for warnings and errors
+ set path using `export PATH=~/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH` and `export PATH=~/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/riscv64-unknown-elf/bin:$PATH`
+ all done


# Introduction to Basic Keywords
## Introduction
- **ISA (Instruction Set Archhitecture)**
  - ISA defines the interface between a computer's hardware and its software, specifically how the processor and its components interact with the software instructions that drive the execution of tasks.
  - It encompasses a set of instructions, addressing modes, data types, registers, memory organization, and the mechanisms for executing and managing instructions.

- **RISC-V (Reduced Instruction Set Computing - Five)**.
  - It is an open-source Instruction Set Architecture (ISA) that has gained significant attention and adoption in the world of computer architecture and semiconductor design.
  - RISC architectures simplify the instruction set by focusing on a smaller set of instructions, each of which can be executed in a single clock cycle. This approach usually leads to faster execution of individual instructions. 

<img width="536" alt="image" src="https://github.com/Veda1809/pes_asic_class/assets/142098395/4eabe0b7-4581-419b-88e7-84c7ac1dac8e">

## From Apps to Hardware
1. **Apps:** Application software, often referred to simply as "applications" or "apps," is a type of computer software that is designed to perform specific tasks or functions for end-users.
2. **System software:** System software refers to a category of computer software that acts as an intermediary between the hardware components of a computer system and the user-facing application software. It provides essential services, manages hardware resources, and enables the execution of application programs. System software plays a critical role in maintaining the overall functionality, security, and performance of a computer system.'
3. **Operating System:** The operating system is a fundamental piece of software that manages hardware resources and provides various services for both users and application programs. It controls tasks such as memory management, process scheduling, file system management, and user interface interaction. Examples of operating systems include Microsoft Windows, macOS, Linux, and Android.

4. **Compiler:** A compiler is a type of software tool that translates high-level programming code written by developers into assembly-level language.

5. **Assembler:** An assembler is a software tool that translates assembly language code into machine code or binary code that can be directly executed by a computer's processor.

6. **RTL:** RTL serves as an abstraction level in the design process that represents the behavior of a digital circuit in terms of registers and the operations that transfer data between them.

 7. **Hardware:** Hardware refers to the physical components of a computer system or any electronic device. It encompasses all the tangible parts that make up a computing or electronic device and enable it to perform various tasks.

## Detail Description of Course Content
**Pseudo Instructions:** Pseudo-instructions are used to simplify programming, improve code readability, and reduce the number of explicit instructions a programmer needs to write. They are especially useful for common programming patterns that involve multiple instructions.
`Ex: li, mv`.

**Base Integer Instructions:** The term "base integer instructions" refers to the fundamental set of instructions that form the foundation for performing basic arithmetic, logical, and data movement operations.
`Ex: add, sub, and, or, xor, sll`.

**Multiply Extension Intructions:** The RISC-V architecture includes a set of multiply and multiply-accumulate (MAC) extension instructions that enhance the instruction set to perform efficient multiplication and multiplication-accumulate operations.
`Ex: mul, mulh, mulhu, mulhsu`.

**Single and Double Precision Floating Point Extension:** The RISC-V architecture includes floating-point extensions that provide support for both single-precision (32-bit) and double-precision (64-bit) floating-point arithmetic operations. These extensions are often referred to as the "F" and "D" extensions, respectively. Floating-point arithmetic is essential for handling real numbers with fractional parts and for performing accurate calculations involving decimal values.

**Application Binary Interface:** ABI stands for "Application Binary Interface." It is a set of rules and conventions that govern how software components interact with each other at the binary level. The ABI defines various aspects of program execution, including how function calls are made, how parameters are passed and returned, how memory is allocated and managed, and more.

**Memory Allocation and Stack Pointer** 
- Memory allocation refers to the process of assigning and managing memory segments for various data structures, variables, and objects used by a program. It involves allocating memory space from the system's memory pool and releasing it when it is no longer needed to prevent memory leaks.
- The stack pointer is a register used by a program to keep track of the current position of the program's execution on the call stack. 

# Labwork for RISCV Toolchain
+ ## Day-1
  
  + L1 (C-program)
    * Here is a simple test program sum1ton.c
    >use vim to write the code
    `vim sum1ton.c`
      ```
      #include <stdio.h>
    
      int main(){
            int i,n = 100;
            int sum = n*(n+1)/2;
            printf("SUm of numbers from 1 to %d is %d\n",n,sum);
            return 0;
      }
      ```
    * output `SUm of numbers from 1 to 100 is 5050`
    
  
  + L2 (RISC-V GCC Compiler and Dissemble)
    + command `riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c` to compile in -O1.
    + command `ls -ltr sum1ton.c` to verify 
    + command `riscv64-unknown-elf-objdump -d sum1ton.o | less ` to view object-dump
    
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/c146cbd1-8e56-40b1-9d1c-1794937ab5d9)
    
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/c6336b0c-c0f1-4ae4-9664-db8560e7c446)
    search for main use `/main`

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/7cbbd9c4-97b4-4e29-acc1-0487f416169b)
    total no of instructions called in main =  (101c0 - 10184)/4 = **15**
    
    + command `riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c`
    + command `ls -ltr sum1ton.c` to verify 
    + command `riscv64-unknown-elf-objdump -d sum1ton.o | less ` to view object-dump
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/ce8b4ebb-3177-4cca-a8c1-d52c0a6363b2)
        search for main use `/main`

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/f1cf3f94-4398-4981-9575-a0422b3601bb)
    total no of instructions is (100e0-100b0)/4 = 12

    - -Onumber : level of optimisation required
    - -mabi : specifies the ABI (Application Binary Interface) to be used during code generation according to the requirements
    - -march : specifies target architecture
    >In order to view the different options available for these fields, use the following commands go to the directory where riscv64-unkonwn-elf is present
    - -O1 : ``` riscv64-unkonwn-elf --help=optimizer```
    - -mabi : ```riscv64-unknown-elf-gcc --target-help```
    - -march : ```riscv64-unknown-elf-gcc --target-help```



  
  + L3 (Spike Simulation and Debug)
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/1aa999c5-26e4-4d05-87b4-c72c82822bef)

    + `spike pk sum1ton.o`
    + `spike -d pk sum1ton.o`
   
+ Lab for unsigned and signed integers:
  + unsigned
 ```
#include<stdio.h>
#include<math.h>

int main(){
        unsigned long long int max = (unsigned long long int)(pow(2,64) -1);
        printf("highest number represented by unsigned long long int is %llu\n", max);
        return 0;
}
 ```
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/0d0c3ce7-b88f-4daa-92ab-960db5bf1913)
  >here the power of 2 is 64.

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/37a4dc4f-eff1-41cd-9530-52a82f02a7f3)
  >here the power of 2 is 127 which is more than the capacity of 64 bit representaion, but still we can see that the maxium has not changed from before
>
  + signed
```
#include<stdio.h>
#include<math.h>
int main(){
        long long int max = (long long int )(pow(2,10) -1);
        printf("the highest integer that can be represented is %lld\n",max);
        return 0;
}
```
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/4a1718f1-be93-4c69-916c-b3ea82535856)
  > notice in code the power of 2 is 10 and type of max is long long int

   ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/156d7044-372c-45b0-8133-4cb3f9912e04)
   > the max and min that can be represented in signed int



  





  


