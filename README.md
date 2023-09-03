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


<details>
  <summary> DAY-1 </summary>
<br>
  

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
</details>

<details>
  <summary> Day 2 </summary>
  <br>

  
# Application Binary Interface

- An Application Binary Interface (ABI) is a set of rules and conventions that dictate how different components of a software system communicate with each other at the binary level. ABI serves as a bridge between high-level programming languages and the machine-level instructions that computers understand.

- ABIs are essential for ensuring compatibility between different parts of a system, especially when those parts are developed by different parties or using different programming languages.

# Memory Allocation for Double Words

Length of a register in the RISCV architecture is 64 bits. The two different ways to load data into these registers:
  - Loading data directly into the registers
  - Loading data into memory and then into the registers.

64-bit number  can be loaded into memory in little-endian or big-endian format.

-Big-Endian:
In a big-endian system the most significant byte value is stored at the lowest memory address, while the least significant byte is stored at the highest memory address. 

-Little-Endian:
In a little-endian system the least significant byte value is stored at the lowest memory address, while the most significant byte is stored at the highest memory address. 

# Load, Add and Store Instructions

**Load Instruction**
Load instructions are used to transfer data from memory into registers.Load instructions are essential for bringing data into the processor's registers before it can be manipulated by other instructions.

```
ld  x6, 16(x7)
```

- ld: Load Doubleword. It indicates that the instruction is used to load a 64-bit value from memory.
- x6: This is the destination register.
- 16: This is the offset value. It specifies the displacement from the address in register x7.
- (x7): This indicates that the address from which to load the data is calculated using the value stored in register x7.


Execution 

![51665fdf-d62d-4c06-bb91-06365aa21656](https://github.com/ashlesh795/pes_asic_class/assets/127172774/a864e0d0-8f92-461f-a9f0-f8988d68aa91)

- funct3 and opcode stores the ld command
- Destination register is stored as 5 bits in rd.
- ource register is stored as 5 bits in rs1.
- Offset is stored as 12 bits in immediate

  
  

 
**Add Instruction**

 Assembly instruction add is used to perform addition between two registers and store the result in a destination register.

 ```
add  x1, x2,x3
```

- x1:destination register
- x2,x3:source registers containing the operands that are to be added.

Execution
![image](https://github.com/Anirudh-Ravi123/pes_asic_class/assets/142154804/505eedee-2f0c-4cf8-9cc6-db5d820eb327)

- funct3 funct7 and opcode stores the add command.
- destination register x1 is stored in rd.
- source registers x2 and x3 are stored in rs1 and rs2.


**Store Instruction**
Store instructions are used to transfer data from registers back to memory. Store instructions are necessary for updating memory with the results of computation carried out by the processor.

 ```
sd  x2, 8(x3)
```
- sd : store doubleword command
- x2 is the data register
- x3 is the source register
- 8 is offset

  ![image](https://github.com/Anirudh-Ravi123/pes_asic_class/assets/142154804/daa44b3e-d70f-4c1c-aa57-bdf84a27de51)

- funct3 and opcode stores the sd command
-  offest 8 is stored as immediate
-  data register x2 is stored in rs2
-  source register x3 in rs1

  # 32-Registers and their ABI Names
  In the RISC-V architecture, there are 32 integer registers, and they are commonly referred to by their numeric indices x0 through x31. 

 **ABI Names**
 These are the names a user uses to access the registers of the RISC-V CPU core.

 
 ![image](https://github.com/Anirudh-Ravi123/pes_asic_class/assets/142154804/e0125ca7-3f3f-40ae-b9b4-90b9c5d5d13d)


 

 # Sum of Numbers from 1 to n using ASM

 We write two programs here, one in C and one in assembly. Main part of the program is processed in ASM and result is desplayed through the C program.
 code:
+ 1to9_custom.c
 ```
#include<stdio.h>
extern int load(int x, int y);
int main(){
        int result=0 ;
        int count =9;
        result = load (0x0,count +1);
        printf("Sum of number from 1 to %d is %d \n ",count ,result);
}
```
+ load.S
```
.section .text
.global load
.type load, @function

load:
        add     a4, a0, zero    //init sum reg a4 with 0x0
        add     a2, a0, a1      //store count of 10 in reg a2. reg a1 loaded with 0xa from main
        add     a3, a0, zero    //init intermediate sum reg a3 by 0
loop:   add     a4, a3, a4      //incremental addition
        addi    a3, a3, 1       //increment intermediate register by 1
        blt     a3, a2, loop    //if a3<a2, branch to label named <loop>
        add     a0, a4, zero    //store final result to register a0 to be read by main
        ret
```

 

![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/0b680cd6-4a4e-421c-8ce6-d2711c98284b)

Execution 


![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/0e892dd0-6536-4806-aa04-f929e3a18dea)




![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/c3bb2d7a-4ebf-4f2e-beb5-354781830932)
</details>


<details>
  <summary> DAY-3 </summary>
<br>
  
# Introduction to verilog RTL Design and synthesis using SKY130
## Open-Source Simulator iVerilog 
**Simulator** is a tool for modeling the design. In order to evaluate the outputs, it searches for changes in the input signals. The simulator doesn't evaluate the outputs if the inputs remain the same. 

**Iverilog** is an open-source simulation and synthesis tool for Verilog that is used to create and test digital circuits. An integrated circuit and FPGA (Field-Programmable Gate Array) designs are examples of digital systems that are modeled and designed using the hardware description language (HDL) known as Verilog.

Simulation Flow
-  A design code is the Verilog or VHDL code that you write to define the logic and behavior of your digital circuit.
-   A test bench is a separate piece of code written to simulate and test your design. It creates input stimuli to the design, monitors the outputs, and checks if the design's behavior matches the expected results.
- The iverilog simulator  is going to look for changes in the input and then accordingly dump the changes in the output. The output of the simulator is going to be a VCD file.
-  Output waveforms generated can be viewed using Gtkwave.GTKWave is a open-source waveform viewer used in digital circuit design and simulation.GTKWave is a versatile tool that aids in the debugging and verification of digital designs. It's widely used by digital designers and engineers to gain insights into their designs' behavior, making it easier to ensure correctness and reliability before moving to hardware implementation.

 [263473358-407cd84a-dfb3-4d28-8cff-c6e3db310d4b](https://github.com/ashlesh795/pes_asic_class/assets/127172774/d7803312-b840-46b2-9b40-6932d0bdebf9)

## Lab using iVerilog and GTKwave
**Installation**
  + run the command in the ubuntu terminal  
`git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git`

the verilog_files directory contains all the source code and test-benches 
![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/ef740d11-872e-46bc-b18b-8f1efb9b9a33)

# Introduction to YOSYS 
Yosys is an open-source software framework for digital logic synthesis and formal verification. It's commonly used in digital design projects to convert high-level hardware description language (HDL) code, such as Verilog, into optimized gate-level representations.

   * a netlist is generated by yosys for the given design file
   * Iverlog is used to generate the vcd file from the netlist and testbench.
   * Output waveform is observed using gtkwave

   * Synthesis is transforming RTL code lower level implementation(gate level).
   * The RTL file and the front end library file is synthesized.
   * Some cells should be fast in order to meet the performance rates and we need some slow cells to meet the "hold" condition.
   * If we use too many fast cells, then the circuit may become bad in terms of power and area. There may also be hold time violations
   * If we use too many slow cells, the circuit may become sluggish and may not meet the required criteria.

## Lab using YOSYS and Sky130 PDKs
  use command  `yosys` in the directory _verilog_files_  
  + yosys interface
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/0b87b0ba-1f94-49c0-b2df-a59ca3be6fbe)
  + read library
    `read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/53033359-423f-4f3d-b488-7e646a1f2f20)
  + read _good_mux_
    `read_verilog good_mux.v`
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/cf718327-82d7-4bd9-b7b4-e3597740b164)
  + synthesize the design
      `synth -top good_mux`
      ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/623f1385-59a6-453f-a2cc-4cbeb43c4a00)

      ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/14e0f576-2c9b-4d6f-bccc-5effb370db2d)
  + generate netlist use `abc`
        ` abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`
        ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/6cdd172f-de6d-41f4-9a4c-c6856d4014ea)
  + `show `
        ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/33f3c685-9a85-4cca-b71b-4318aaab8a4e)


  + write netlist
    `write_verilog good_mux__netlist.v` then `!vim good_mux_netlist.v`
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/173370ee-e061-4dc6-9562-c2e5487030db)
    `write_verilog -noattr good_mux__netlist.v` then `!vim good_mux_netlist.v`

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/c91cfb07-7f42-47e1-b57d-a4a911d5cc03)

</details>

<details>
  <summary> DAY-4 </summary>
  <br>
 

# Introduction to timing dot libs  
  + to view the lib `vim ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

    
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/0c0a72cc-0371-489b-a5e2-9f47227d5a8d)

# Heirarchical Synthesis vs Flat Synthesis 

  + multiple modules in _verilog_files_ `vim multiple_modules.v`

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/89164ba3-e971-4709-ba31-482408df9d2e)
+ heirarchical synthesis 
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
```


  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/415102bb-4b71-436a-b40a-2b140dd2fefd)


  + `write_verilog -noattr multiple_modules_heir.v`
  + `!vim multiple_modules_heir.v`
  
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/c15ffeba-fac4-4d03-923f-866ea246e3f5)
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/70e9f686-4728-4cac-a74a-079cb3506118)
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/bc435bad-ed42-4a2d-ad7c-25538b418396)

+ flat synthesis
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
```
    show 
    
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/ea899f41-d09f-4be3-9fd5-0165616d7074)
    
    
    
    write_verilog -noattr multiple_modules_flat.v
    !vim multiple_modules_flat.v


  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/87aed911-6f5e-44cd-af16-5c2969958ee5)
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/bb3cedac-8337-448a-99ea-90dbf8ca19bc)
  
  
   + submodule level synthesis
     + AND submodule 
   ```
   yosys
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog multiple_modules.v
   synth -top sub_module1
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

       show

   ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/d324c23f-de4a-4389-afde-119f5d0b63d8)

##  Various Flop Coding Styles and optimization

  + asynchronous reset
    code
```
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
        if(async_reset)
                q <= 1'b0;
        else
                q <= d;
end
endmodule
```

    
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/b9f4d2dc-ada2-412e-9e20-e49fcac58f8d)


  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/a1e2d921-fc39-418e-9fd4-d8a4160553d8)


  + asynchronous set
    code
```
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
        if(async_set)
                q <= 1'b1;
        else
                q <= d;
end
endmodule
```

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/db40ae58-6a20-407b-950c-c46170a6ba08)


  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/95a70252-cf43-46c0-8c82-8f3c47f88fb7)

  + Asynchronous reset

```
   yosys
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog dff_asyncres.v
   synth -top dff_asyncres
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

    show

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/92976890-5fa9-48ac-97ec-76c8b668049e)

  + Asynchronous set
    
```
   yosys
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog dff_async_set.v
   synth -top dff_async_set
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
    show
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/4ca65794-96a5-47ba-a60a-859cdb40e65b)

 + synchronous reset

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/a037e22e-f409-4c05-b623-e063407f2c88)

  ## Interesting Optimizations

  ```
   yosys
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog mult_2.v
   synth -top mul2
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/070cbc12-9edf-41d8-bfb7-d20c1e92834a)


      show
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/84c598b8-2858-4e20-8ee8-aa6ff0265d1d)

</details>

<details>
  <summary> Day-5 </summary>
  <br>

# Combinational and sequential optmizations
## Combinations Optmizations 
  + opt_check

```
module opt_check (input a , input b , output y);
        assign y = a?b:0;
endmodule
```

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
    show


  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/022467bb-8551-4e78-b360-12413592c7c3)
  + opt_check2

```
module opt_check2 (input a , input b , output y);
        assign y = a?1:b;
endmodule
```


```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check2.v
synth -top opt_check2
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
    show
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/f6279f43-922a-4044-81ed-04ab28c76bcf)      

  + opt_check3

```
module opt_check3 (input a , input b, input c , output y);
        assign y = a?(c?b:0):0;
endmodule
```

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/d07559e9-9217-415e-aef9-eed624aced29)

  + opt_check4

```
module opt_check4 (input a , input b , input c , output y);
assign y = a?(b?(a & c ):c):(!c);
endmodule
```

![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/8dcb1c14-6db7-4c9f-ad6c-ef368d063eae)      ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/0e5ad8f4-f703-4581-bc2b-288f2922efc1)


  + multiple_module_opt

```
module sub_module1(input a , input b , output y);
assign y = a & b;
endmodule


module sub_module2(input a , input b , output y);
assign y = a^b;
endmodule


module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1);


endmodule
```


```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check2.v
synth -top opt_check2
flatten
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/d2538584-48a9-4920-a012-81ea659cded6)



## Sequential Logic Optimisations
  + dff_const1

        vim dff_const1

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/5e96eaa0-810c-4986-bbc2-c4236ee4fa97)

        iverilog dff_const1.v tb_dff_const1.v
        ./a.out
        gtkwave tb_dff_const1.vcd

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/6e9c88ef-8f2f-4f25-a0b2-416f5d1acd7a)


        yosys
        read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
        read_verilog dff_const1.v
        synth -top dff_const1
        dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
        abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
        show


    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/72a35d30-ad35-40b1-800e-7c44f25b4fa8)

  + dff_const2

          vim dff_const2.v
    
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/8e17934a-bb88-482a-b716-9bc0f4c537af)



          iverilog dff_const2.v tb_dff_const2.v
          ./a.out
          gtkwave tb_dff_const2.vcd

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/fc0d063c-b573-4c9d-8c6d-03e8f06b1bc7)


         
          read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
          read_verilog dff_const2.v
          synth -top dff_const2
          dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
          abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/2945631c-697f-4188-a16c-08bd81ac3696)

          show
          

![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/0da4610c-334b-4cff-8e44-3811afb55d34)

+ dff_const3

  `vim dff_const3`

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/81084a5e-c777-4562-a72c-46f856add56f)

  ```
    iverilog dff_const3.v tb_dff_const3.v
    /a.out
    gtkwave tb_dff_const3.vcd
  ```
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/04504d47-1e25-4ad2-99ba-6786881a5df7)

    + synthesis
 
      
   ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/6dac8ec5-56fe-4422-b751-00769128d7e0)


    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/54ed6adb-a029-472e-bb25-fd592910541b)
  

+ dff_const4

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/e72fd5ca-c77c-4771-b52b-4e5637e9ea68)

      iverilog dff_const4.v tb_dff_const4.v
      /a.out
      gtkwave tb_dff_const4.vcd

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/b35c5736-6589-43e0-adb8-4ac3855c87d7)


    + synthesis
 
  
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/399a2b03-cad6-47ef-b617-fd5cb8f98d58)

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/d518b43e-f9ab-4afd-a45e-fd9ddac53858)

+ dff_const5

        gvim dff_const5.v
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/dfad99a7-1655-4e3c-aeda-e52a67e5e2d6)

## Sequential Optimisations for Unused Outputs.
  + counter_opt

        vim counter_opt.v

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/87ea97a4-73b2-4d6b-97e9-f06af730ef95)

        read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
        read_verilog counter_opt.v
        synth -top counter_opt
        dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
        abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
        show

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/eda99cc2-4803-4579-8c3c-6dfb193825ba)

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/d9a8f9bc-6e91-4077-bcbf-bd265f215c29)

  + counter_opt2

        gvim counter_opt2.v

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/b643293f-328d-4c81-b88e-817d7551f004)
        
        read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
        read_verilog counter_opt2.v
        synth -top counter_opt
        dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
        abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
        show

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/ea441032-c502-4223-a331-9fc26fdd6355)


![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/e5d79db2-3374-4917-b9a4-1f67b7abec7b)


</details>

<details>
  <summary> Day-6  </summary>
  <br>
  
# GLS Concepts And Flow Using Iverilog 
  
 + **Gate Level Simualtion**
   - Gate-level simulation is a technique used in digital design and verification to validate the functionality of a digital circuit at the gate-level implementation.
   - It involves simulating the circuit using the actual logic gates and flip-flops that make up the design, as opposed to higher-level abstractions like RTL (Register Transfer Level) descriptions.
   - This type of simulation is typically performed after the logic synthesis process, where a high-level description of the design is transformed into a netlist of gates and flip-flops.
   - We perform this to verify logical correctness of the design after synthesizing it. Also ensuring the timing of the design is met.
  
<img width="608" alt="image" src="https://github.com/Veda1809/pes_asic_class/assets/142098395/6298b067-2f45-4dbc-ad25-762ac3d8be63">

+ **Synthesis-Simulation Mismatch**
  - A synthesis-simulation mismatch refers to a situation in digital design where the behavior of a circuit, as observed during simulation, doesn't match the expected or desired behavior of the circuit after it has been synthesized.
  - This discrepancy can occur due to various reasons, such as timing issues, optimization conflicts, and differences in modeling between the simulation and synthesis tools.
  - This mismatch is a critical concern in digital design because it indicates that the actual hardware implementation might not perform as expected, potentially leading to functional or timing failures in the fabricated chip.

+ **Blocking Statements**
  - Blocking statements are executed sequentially in the order they appear in the code and have an immediate effect on signal assignments.
  - Example:

  ``` 
   module BlockingExample(input A, input B, input C, output Y, output Z);
    wire temp;

    // Blocking assignment
    assign temp = A & B;

    always @(posedge C) begin
        // Blocking assignment
        Y = temp;
        Z = ~temp;
    end
   endmodule
  ```

+ **Non-Blocking Statements**
  - Non-blocking assignments are used to model concurrent signal updates, where all assignments are evaluated simultaneously and then scheduled to be updated at the end of the time step.
  - Example:
   ``` v
    module NonBlockingExample(input clock, input D, input reset, output reg Q);

    always @(posedge clock or posedge reset) begin
        if (reset)
            Q <= 0;  // Reset the flip-flop
        else
            Q <= D;  // Non-blocking assignment to update Q with D on clock edge
    end
  endmodule
   ```

+ **Caveats with Blocking Statements**
  + Blocking statements in hardware description languages like Verilog have their uses, but there are certain caveats and considerations to be aware of when working with them. Here are some important caveats associated with using blocking statements:
    - Procedural Execution: Blocking statements are executed sequentially in the order they appear within a procedural block (such as an always block). This can lead to unexpected behavior if the order of execution matters and is not well understood.
    - Lack of Parallelism: Blocking statements do not accurately represent the parallel nature of hardware. In hardware, multiple signals can update concurrently, but blocking statements model sequential behavior. As a result, using blocking statements for modeling complex concurrent logic can lead to incorrect simulations.
    - Race Conditions: When multiple blocking assignments operate on the same signal within the same procedural block, a race condition can occur. The outcome of such assignments depends on their order of execution, which might lead to inconsistent or unpredictable behavior.
    - Limited Representation of Hardware: Hardware systems are inherently concurrent and parallel, but blocking statements do not capture this aspect effectively. Using blocking assignments to model complex combinational or sequential logic can lead to models that are difficult to understand, maintain, and debug.
    - Combinatorial Loops: Incorrect use of blocking statements can lead to unintentional combinational logic loops, which can result in simulation or synthesis errors.
    - Debugging Challenges: Debugging code with many blocking assignments can be challenging, especially when trying to track down timing-related issues.
    - Not Suitable for Flip-Flops: Blocking assignments are not suitable for modeling flip-flop behavior. Non-blocking assignments (<=) are generally preferred for modeling flip-flop updates to ensure accurate representation of concurrent behavior.
    - Sequential Logic Misrepresentation: Using blocking assignments to model sequential logic might not capture the intended behavior accurately. Sequential elements like registers and flip-flops are better represented using non-blocking assignments.
    - Synthesis Implications: The behavior of blocking assignments might not translate well during synthesis, leading to potential mismatches between simulation and synthesis results.

## Labs on GLS and Synthesis-Simulation Mismatch 

  + ternary_operator_mux

        vim ternary_operator_mux.v
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/486e822c-e0e1-4438-b9ae-9b767779b893)



        iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
        ./a.out
        gtkwave tb_ternary_operator_mux.vcd

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/8ce3e2f1-a5b5-43d4-a774-d3a192c3b885)

    
        read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
        read_verilog ternary_operator_mux.v
        synth -top ternary_operator_mux
        abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
        show

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/83216b6a-c35d-4662-9983-5e2606059753)

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/19aca8a5-42c0-4467-80f3-a224491c2a21)


  ### GLS to Gate-Level Simulation

  + `iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v`
  + `./a.out`
  + `gtkwave tb_ternary_operator_mux.vcd`

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/88d597e9-f713-4f44-ad0a-faa21124258f)

+ bad_mux

       vim bad_mux.v
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/f5246e7c-b834-493b-b707-95e9fe7c5bf9)

  

      iverilog bad_mux.v tb_bad_mux.v
      ./a.out
      gtkwave tb_bad_mux.vcd

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/0e0b3b21-3644-4ec6-94b1-2ecd1a6a5d01)



  
      read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
      read_verilog bad_mux.v
      synth -top bad_mux
      abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
      show

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/90ee6402-a8bd-489f-aa17-09f070973311)

  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/62090349-8c98-488f-8ae5-23e7754d342e)



  ### GLS to Gate-Level-Simulation 
      
      iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
      ./a.out
      gtkwave tb_bad_mux.vcd
  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/d0c08702-b1f0-4eca-af11-f30a327e9aaf)


  ### Labs on Synth-Sim Mismatch for Blocking Statement
    + blocking_caveat.v

          vim blocking_caveat.v
    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/a6bbbba2-1c6d-4a9b-ab96-f941bb983156)


            
          iverilog blocking_caveat.v tb_blocking_caveat.v
          ./a.out
          gtkwave tb_blocking_caveat.vcd


    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/3c0171d9-4127-4263-84b9-6c7d1571e83a)


      
          read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
          read_verilog blocking_caveat.v
          synth -top blocking_caveat
          abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
          show

    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/ea08f0b0-417b-4082-a17a-4fa499233459)


  ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/d7e4821a-de36-4687-aa43-564d1f0d64d7)




   #### GLS to Gate-Level Simulation

      iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
      ./a.out
      gtkwave tb_blocking_caveat.vcd


    ![image](https://github.com/ashlesh795/pes_asic_class/assets/127172774/3f2afd6b-35ca-41f2-8ded-c9f4845d9489)

</details>




    
