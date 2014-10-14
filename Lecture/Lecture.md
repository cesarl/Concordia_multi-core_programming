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

----------------------------------------------------------------------------

-------------------------------------------

# Week 3

## Lecture

About the assignement :
- Observations we make with this assignement will be usefull for the nexts assignements
- The purpose is to realize difficulties you encounter coding with threads (and not Cuda / OpenCL / Intel TBB)

Once we design a MT program we have to see if we can improve the performance.
Improve performance in MC environement is a tricky issue. Some sort of things can work other not.

Factors which affect performances :
- Coverage
	- When you decide to parlise and what part of the code ? Is it possible to improve the performance with MT design ? How much parallelism you will implement ? Choose the right extend of parallism.
- Granularity
	- Once you choose your coverage ? How can I split my algorythm / architecture in multiple threads ?
- Locality
	- You choose the approriate algo and granularity, you now have to choose right locality. You compute on multiple cores but at the end you have to put the ressources at the same place. Their'll be some communication between threads (messages, or shared data).
	- You have to balance computation and communication properly ?
	- You have to worry how the threads are actually executed. You cannot control where the threads run.

### Coverage

Example of raytracing : it does not require synchronisation between threads. We can call that type of architecture : embarrassingly parallel.

But in real life all the computationnal problems are not so easy that raytracing. They're not all embarrassingly parallel. There is some sequential part.
The ratio between sequential part and parallel part is the amount of parallelism you can add to your program.
Sequential code will always limit the performance improvements. If sequential code take 50 % of the time, even with 100 thread you'll not be able to multiply the performance by two.

### Granularity

Granularity is the ration to computation to communication. If communication is small, the ratio will be high, so threads can do a lot of computation.
Synchronisation is necessary for data cohesion but bad for performances.

- Fine-grain paralleism :
	- Small computation, lots of communication
	- Lots of memory access needed
	- GPUs are good at that
- Coarse grain computation :
	- Large computation, small communication
	- Can execute lots of computation before needed memory access
	- The problem is that usualy making sure every core is used equaly is very difficult. We can be sure of that only if we are sure that every core are doing the same computation
	- CPUs are good at that

- Load balancy :
	- It's trying to avoid that some threads finish before others. In that case threads have to wait others have finished.

- Static load balancing :
	- The programmer decide at the moment of coding what each thread has to do.
	- Static load balancing work well when all the core are similar (so only for homogenous processor, like CPUs for example).

- Dynamic load balancing :
	- Thread choose at runtime what work he will do.

Ganularity and performance tradeoffs:
- Load Balancing :
	-  How well is work distributed among cores ?
-  Synchronization
-  Communication

### Overlapping messages (communication) and computation

A pipeline. When you perform computation on a piece of data you access the next piece of data for futur computation. But you have to be sure that the time to get from memory should be about the same as to precess data -> but in that case, your program is not portable.

There is no way to get all the data into the core. Cores will have to get data into chunk.

### Locality of memory accesses

(see slide 43)
If all the access 0, 4, 8, 12 come from the same memory banks, only one thread will be able to write.

###

In shared memory archi there is different ... :
- UMA
	- Every processor can access the memory at the same time
	- So it doesnt matter wigh processor access the memory
	- We can name it SMP
- NUMA
	- We have to know which processor is accessing which data


