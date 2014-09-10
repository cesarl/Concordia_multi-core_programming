# MC Lecture 01

## 02/09/14

### Introduction

Changes :

- No more SPU (PS3) 
- We program on whatever we want. (laptop, etc, etc)
- New textbook (OpenCL in action)

We are going to look at :

- CPUs - ~12 threads
	- Identical cores == homogenious MC processors
	- Tutorials during labs ( C and C++ )
	- Tools used : TBB (_Intel_)
- GPUs - ~Thousands on cores == many cores systems
	- CUDA (_Nvidia_)
- Both GPU and GPU at the same time == heterogenious multi-core programing
	- We'll use OpenCL

------------------------------------------

### Course

The CPU is run by a clock, the fastest the clock is the fastest the CPU'll be.

Pipelining to reduce clock period :
	- When an instruction come to CPU it has to pass through multiple state (instruction fetch, instruction decode, fetch data in memory, process data, return processed data to memory, etc).
	- In the old day, all the steps are done in the same clock cycle. So a clock cycle should be long enought to do allt steps in one cycle.
	- So what they did is to enable multiple registers (see slide 3). Multiple instructions are done in the same clock cycle.
	- So each register are doing specific action, differents that other register.
	- So clock cycle can be reduce because their is less work per register at each clock cycle.

C1    |C2    |C3    |C4    |
A     |B     |C     |D     |
      |A     |B     |C     |
      |      |A     |B     |
      ...

By pipelining the processing we can reduce the clock rate delay, so the speed increase.

The next idea to improve speed is Instruction Level Parallelism :
	- Execute multiple instructions in the the same time (three instrucitons in place of one in the past).
	- For example, 2 integer instruction and one floating point instrction at the same time.
	- So instructions are dependent between each other
	- Your compiler is in charge to prepare this, you don't have to code it manualy

The next idea after increasing the instruction register is to increase the data register.
So we can use the same instruction on multiple datas == vector processing.

The next idea is why not execute two stream of instructions at the same time == Hardware multithreading.
	- No need of context switch

After that nothing more can be done. And the memory cannot support the speed of the CPU.

Multi threading can help the memory wall, running two thread at the time, sitching : one is getting the memory, the other is executing and vice versa.

So they were blocked, and thats when the MC idea came up ! The idea : keep the CPU simple, don't rededign it, just put more CPUs.
The multicore system came as a practical way to solve the CPU limits.

Advantages on MC CPUs :
	- Short distances between cores offer better bandwith and fastest data transfer.
	- Reduce power consumption
	- Can share cache between cores
	- ---> Improve performances !!!

Disavantages / Challenges :
	- Require a compatible operating system
		- Windows can handle 4 cores okay, but more, it'll not
		- Linux can handle 64 cores at maximum
	- You have to adapt your programming to the hardware.

Multicore programs are similar to parallel programs but there is different kind of parallel systems (Flynn's Taxonomy):
- MISD -> Multiple instruction, Single Data
- MIMD -> Multiple instruction, Multiple Data
	- for example with multiple computers working on multiple datas.
	- Bus interconection structure (TODO : Google it)
- SIMD -> Single instruction, Single Data
	- == Vector processing

The difference between some MIMD architecture and multicore is when each core are in a separated chip in MIMD and MC, core are in the same chip. When cores are in the same chip, data transfer between cores is a lot faster.

Multi-core processor :
- All the cores are on the same chip
- Multi-cores are MIMD architecture
- Multi-core are shared memory architectured chips

...

Coarse grained computation means that each cores should do heavy computation, not simple operation. Coarse grained computation permit to reduce context switch.