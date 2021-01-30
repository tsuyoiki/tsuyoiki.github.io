### Chapter 2 - Operating System Structures

We can view an operating system from several vantage points. One view focuses on the services that the system provides; another, on the interface that it makes available to users and programmers; a third, on its components and their interconnections. All three will be covered here.

#### OS services designed to help the user:

1. User interface (UI). Most commonly a graphical UI is used (GUI), touch-screen interfaces are used on mobile systems, command-line interface is another option. 
2. Program execution. Loading the program into memory and running it, the program must be able to end normally (or abnormally if there is an error)
3. I/O operations.
4. File-system manipulation. Programs need to read, write, create, delete, search for files and directories and list information. Some OSs include permissions management among other features as well. 
5. Communications. Processes need to be able to communicate with each other and exchange information. Communications may be implemented through a shared memory (read and write to a section of memory that is shared between the processes) or message passing (packets of information are moved between the processes by the OS).
6. Error detection. The OS must detect and correct errors. It must be able to determine the type of error that has occurred, take the necessary steps to correct it, halt the system if necessary. 

#### OS services designed to make sure it is being used efficiently:

1. Resource allocation. With many processes running simultaneously the OS will determine the best way to allocate resources to each of them. 
2. Logging. Keeping track of the usage statistics. This information can be really valuable to system administrators who would like to reconfigure the system and make it more efficient. 
3. Protection and security. One process should not interfere with another process or the OS. Protection ensures that access to system resources is controlled, users are authenticating themselves, and the OS is keeping track of invalid login attempts. 

#### Users can interface with the OS in a few ways:

1. Command interpreters. (terminal in Kali Linux for example uses a command interpreter)
2. Graphical User Interface (GUI). (Windows, Apple)
3. Touch-Screen Interface. (Most mobile systems)

#### System calls 
provide an interface to the services made available by the OS. Most system calls are accessed by an Application Program Interface (API) such as Win32, POSIX, Java. There are six major categories of system calls:

1. Process control
2. File management
3. Device management
4. Information maintenance 
5. Communications
6. Protection

#### System services (or system utilities) 
provide an environment for program development and execution. Some of them are quite simple - user interfaces to system calls, some are more complex. They include:

1. File management (access and manipulate files and directories)
2. Status information (asking the system for the date and time, amount of free memory, logging, debugging information, ect)
3. File modification (create and modify contents of files)
4. Programming-language support (compilers, debuggers, assemblers for common programming languages such as C are often provided with the OS or available to easily download)
5. Program loading and execution (When the program is ready it must be loaded into memory for execution) 
6. Communications (these allow creating virtual connections between processes, users, and computer systems)
7. Background services (constantly running system-program processes known as services, subsystems or daemons. For example, process schedulers that start a process at a specific time, or a system error monitoring service)


#### Linkers and loaders

The linker combines the relocatable object files and libraries into a single binary executable file. The loader then loads the binary into memory to run it. To run the loader all you need to do is enter the name of the executable in CLI. What happens is, a new process gets created with the fork() system call (on UNIX). The shell then invokes the loader with the exec() system call, with the name of the executable passed to exec(). The loader loads it into memory using the address space of the newly created process. 

In the above process all libraries are linked into the executable and loaded into memory. You can also link the libraries dynamically when the program is being loaded. These are called DLLs. This avoids linking and loading libraries that might not be used, which saves memory.

Fundamentally, programs compiled on one OS will not work on a different OS. There are a few reasons for this: different binary formats for program executables (ELF for Linux, PE for Windows… ), different instruction sets for different CPUs, system calls that vary from one OS to the next. Developing cross-platform applications is still possible but quite challenging.


#### OS design and implementation 

There are two fundamental requirements for a good system design: user goals and system goals. These goals determine the system’s policies that are implemented through specific mechanisms. 

The simplest structure is no structure: all functionality of the kernel exists in a static binary file that runs in a single address space. This is called monolithic structure and is a common technique when designing OSs. (example - original UNIX OS) Despite its simplicity, monolithic kernels are difficult to implement. They have a large performance advantage though (little overhead in the system-call interface makes communication within the kernel really fast) therefore we still see evidence of this structure in the UNIX, Linux and Windows OSs. 

A loosely coupled system is one divided into separate components that have limited functionality. All of the components together make up the kernel. The advantage of this approach is that changes made to one component are not affecting any of the others, allowing for more freedom in modifying the inner workings of the system. Creating such a system can be accomplished in many ways, one of which is a layered approach. The system is broken down into layers, the bottom one (0) being the hardware, the highest one being the user interface. (Similar to TCP/IP model in networking). The main advantage of this approach is simplicity of construction and debugging. It is not ideal for OS design though due to performance problems. 

The microkernel approach structures the OS by removing all nonessential components from the kernel and implementing them as user-level programs that reside in separate address spaces resulting in a smaller kernel. Communication happens via message passing. (Darwin (the kernel component of macOS and iOS) is the best-known example of a microkernel OS). Performance of microkernels suffers because of large system-function overhead (Windows for instance went from microkernel to a more monolithic approach)

LKMs - Loadable Kernel Modules are a common design in modern implementations of UNIX (Linux, macOS, Solaris) as well as Windows. The kernel provides core services, and other services are dynamically implemented during kernel runtime. For example, CPU scheduling and memory management algorithms are built directly into the kernel and support for different file systems is added with loadable modules. The result resembles a layered system but is more flexible because any module can call any other module, and it is also similar to the microkernel system because the primary module has only core functions. It is more efficient than either of them though because modules do not need to do message passing in order to communicate. 

In reality, OSs usually combine the above mentioned approaches and form hybrid systems. Linux is monolithic but also modular. Windows is to a large extent monolithic too, with some microkernel features and LKMs. 

A boot loader loads the OS into memory and performs initialization, followed by system execution. The performance of an OS can be monitored through counters (collection of system-wide (netstat, vmstat, iostat on Linux) or per-process (ps, top on Linux) statistics) or tracing (following the execution of a program through the OS. Examples on Linux - strace, tcpdump). 
Practice Exercises

#### Questions 

##### 2.1 What is the purpose of system calls? 

System calls provide an interface to the services made available by an operating system.

##### 2.2 What is the purpose of the command interpreter? Why is it usually separate from the kernel? 

A command interpreter allows the user to interact with a program using commands in the form of text lines. It was frequently used before but is nowadays often replaced by GUIs. More experienced users still prefer CLI to GUI though because it is more customizable and efficient. It is separate from the kernel because it is subject to changes. How it works is it reads commands from the user and transforms them into system calls.

##### 2.3 What system calls have to be executed by a command interpreter or shell in order to start a new process on a UNIX system?

Fork() and exec(). Fork() creates a new process by copying the parent process’ image. Exec() overlays a new process based on a different executable over the calling process. 
More on those: https://www.geeksforgeeks.org/difference-fork-exec/

##### 2.4 What is the purpose of system programs?

System programs are groups of useful system calls. They are very valuable because with their help users don’t have to create their own programs to solve issues and the user process is a lot more convenient. It is like a bridge between the user interface and the system calls.

##### 2.5 What is the main advantage of the layered approach to system design? What are the disadvantages of the layered approach?

The main advantage is simplicity of construction and debugging.The main disadvantage lies in performance problems due to large overhead.

##### 2.6 List five services provided by an operating system, and explain how each creates convenience for users. In which cases would it be impossible for user-level programs to provide these services? Explain your answer.

OS services designed to create convenience for the user are: 
 
1. Program execution. Loading the program into memory and running it. A user level program can’t be trusted to allocate CPU resources properly.
2. I/O operations. User level programs can’t be responsible for only accessing the devices they should have access to, so it is managed by the OS. 
3. File-system manipulation. Programs need to read, write, create, delete, search for files and directories and list information. Some OSs include permissions management among other features as well. User programs can’t handle permission management thus this is also managed by the OS.
4. Communications. Processes need to be able to communicate with each other and exchange information. Communications may be implemented through a shared memory (read and write to a section of memory that is shared between the processes) or message passing (packets of information are moved between the processes by the OS). User programs might not coordinate packets destined to a network device or know what to do with packets coming to or from a different process so the OS takes care of that also.
5. Error detection. The OS must detect and correct errors. It must be able to determine the type of error that has occurred, take the necessary steps to correct it, halt the system if necessary. Errors may be process independent such as corruption of data on a disk. Therefore the OS must take care of these errors.

##### 2.7 Why do some systems store the operating system in firmware, while others store it on disk?

For some devices, a disk might not be available to store the system on so they have to store it in firmware, for example - mobile devices.

##### 2.8 How could a system be designed to allow a choice of operating systems from which to boot? What would the bootstrap program need to do?

This can be achieved with a boot manager. Instead of booting any system, the boot manager will run at system startup and determine which system to boot into, allowing the user to choose the preferred one or booting into a default one if no choice was made. 

##### 2.9 The services and functions provided by an operating system can be divided into two main categories. Briefly describe the two categories, and discuss how they differ. 

Services designed for the system and services designed for the user. First category includes resource allocation, protection and security, and logging. Second category includes communications, error detection, i/o operations, file system management, and program execution.

##### 2.10 Describe three general methods for passing parameters to the operating system.

Parameters can be passed in registers
If there are more parameters than registers, they can be all stored in a block in memory and the address of this block is passed as a parameter to the register.
Parameters can be pushed onto the stack or popped off the stack by the OS.

##### 2.11 Describe how you could obtain a statistical profile of the amount of time a program spends executing different sections of its code. Discuss the importance of obtaining such a statistical profile.

You could set periodic timer interrupts and analyze what sections of the code were being executed during the interrupt. Once you get the statistics, you can improve the sections of the code that were consuming a lot of CPU resources and optimize everything.

##### 2.12. What are the advantages and disadvantages of using the same system call interface for manipulating both files and devices? 

Advantage - when you use the same system call interface for files and devices, any new device will be treated as a file and adding new devices will be straightforward and simple. 
Disadvantage - it is hard to implement all functionality of certain devices into the file system, so performance might be limited and some functionality might be lost.

##### 2.13 Would it be possible for the user to develop a new command interpreter using the system-call interface provided by the operating system?

Yes. A command interpreter allows the user to create processes, see how they communicate with each other, stop them if necessary. All of this functionality can be accessible to a user-level program using system calls, so it is possible for the user to develop their own CLI. 

##### 2.14 Describe why Android uses ahead-of-time (AOT) rather than just-in-time (JIT) compilation.

AOT compilation allows more efficient application execution as well as reduced power consumption, which is crucial for mobile systems.

##### 2.15 What are the two models of interprocess communication? What are the strengths and weaknesses of the two approaches?

IPC is a mechanism that allows the processes to communicate with each other and synchronize their actions. The two models are:

- Memory sharing
- Message passing

Memory sharing strengths and weaknesses:

System calls are made only to establish the shared memory thus communication is really fast and efficient 
Good to use on a single or multiprocessor systems, the processes are residing on the same machine and can share a common address space
Need to make sure that the processes are not writing to the same address space simultaneously 
Could compromise protection and synchronization between the processes sharing memory

Message passing strengths and weaknesses:

Good to use in distributed environments where the communicating processes are residing on remote machines connected through a network
Good to use to exchange small amounts of data
Easier to implement than shared memory 
Slower because messages will be shared through system calls

##### 2.16 Contrast and compare an application programming interface (API) and an application binary interface (ABI). 

An ABI defines how data structures or computational routines are accessed in machine code, which is a low-level, hardware-dependent format. 
An API defines this access in source code, which is a relatively high-level, hardware-independent, often human-readable format. 

API - public types, variables, functions that you expose from your library/application. In C this is what you write in the header. ABI - how the compiler builds the application, how parameters are passed to functions, who cleans parameters from the stack, how exceptions are made, and so on. 

Better explanation: https://lists.debian.org/debian-user/2004/02/msg00648.html

##### 2.17 Why is the separation of mechanism and policy desirable?

Policy and mechanisms should be separated so that they don’t dictate each other what to do. For example: using card keys to open a locked door. The mechanism (card reader, lock, connection to the security server) has no effect on entrance policy (who and when may enter) - these decisions are made by the security server according to a policy. Changes can be made by updating the policy. In contrast, with physical keys you can’t change the policy - you have to change all the locks and keys every time you need to limit one person from entry. This is much more inconvenient, therefore policy and mechanisms should be separated making the system more flexible.

##### 2.18 It is sometimes difficult to achieve a layered approach if two components of the operating system are dependent on each other. Identify a scenario in which it is unclear how to layer two system components that require tight coupling of their functionalities.

The virtual memory and storage subsystems are usually tightly coupled and it is hard to put them on different layers. Most systems allow files to be mapped into the virtual memory space of an executing process, while the virtual memory relies on the storage system to provide backup for pages not currently in memory. Moreover, file system updates are usually buffered in physical memory requiring careful management of the memory usage between the file system and virtual memory. 

##### 2.19 What is the main advantage of the microkernel approach to system design? How do user programs and system services interact in a microkernel architecture? What are the disadvantages of using the microkernel approach?
The microkernel approach structures the OS by removing all nonessential components from the kernel and implementing them as user-level programs that reside in separate address spaces resulting in a smaller kernel. Communication happens via message passing. 

The main advantages are:

Architecture is small and isolated thus it is more efficient
The expansion of the system can happen without disrupting the kernel 
Microkernels are modular, different modules can be modified without disrupting the kernel 
Fewer system crashed compared to monolithic systems due to a simpler design
Increased security as most programs will run in user mode

The biggest disadvantage is large system overhead tied to IPC, need for frequent system calls in order to allow for the user process and the system service to communicate. 

##### 2.20 What are the advantages of using loadable kernel modules? 

LKMs - Loadable Kernel Modules are a common design in modern implementations of UNIX (Linux, macOS, Solaris) as well as Windows. The kernel provides core services, and other services are dynamically implemented during kernel runtime. For example, CPU scheduling and memory management algorithms are built directly into the kernel and support for different file systems is added with loadable modules. The result resembles a layered system but is more flexible because any module can call any other module, and it is also similar to the microkernel system because the primary module has only core functions. It is more efficient than either of them though because modules do not need to do message passing in order to communicate

##### 2.21 How are iOS and Android similar? How are they different?

The main similarities between iOS and Android are:

Basic functions are almost the same (messaging, calling, Internet browsing, ect…)
User interfaces are similar (possibility of swiping, zooming in, ect on the screen)
4G cellular network is supported by both
The status bar offers similar info (battery life, wifi, time, notifications, ect…)
Privacy settings are similar

The main differences lie in what we can’t see on the screen:

Android is open source, iOS is closed source
Android is developed by Google and apps can be found in GooglePlay, iOS is developed by Apple and apps can be found in the Apple app store
Android software is available for many manufacturers thus quality control is more difficult, iOS is strictly controlled by Apple thus quality may be higher
iOS apps are developed in Objective-C, Android in Java
iOS executes code natively, Android uses a VM

##### 2.22 Explain why Java programs running on Android systems do not use the standard Java API and virtual machine.

Because they were developed for desktop, not mobile devices. Google developed a separate API and VM for mobile devices.

##### 2.23 The experimental Synthesis operating system has an assembler incorporated in the kernel. To optimize system-call performance, the kernel assembles routines within kernel space to minimize the path that the system call must take through the kernel. This approach is the antithesis of the layered approach, in which the path through the kernel is extended to make building the operating system easier. Discuss the pros and cons of the Synthesis approach to kernel design and system-performance optimization.

The performance of Synthesis is really impressive due to fast communication. The problem is that it is difficult to debug problems within the kernel due to fluidity of the code, and a new compiler must be written for each architecture making the system hard to port. 
