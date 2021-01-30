### Chapter 1 - Introduction

1.1 What are the three main purposes of an operating system?

  Efficiently manage the  computer’s resources (memory, CPU, ect)
	Make the system easy to understand and use for people
	Execute programs conveniently and quickly 

1.2 We have stressed the need for an operating system to make efficient use of the computing hardware. When is it appropriate for the operating system to forsake this principle and to “waste” resources? Why is such a system not really wasteful?

For instance, the goal is to create a good user experience. When the system is not being used we’ll see a screensaver on the screen. It is a “waste” and is not optimal, but people like them and find them enjoyable. Another example - a large calculation that needs to be performed in the shortest time possible. All the resources will be reallocated to perform the calculation and other things will be put on hold. (For instance, in weather forecasting systems - if there is a hurricane approaching the calculation of it’s timing and vector will take precedence over the weather forecast calculations for the next ten days)

1.3 What is the main difficulty that a programmer must overcome in writing an operating system for a real-time environment? 

The main difficulty is making sure that real time tasks are being executed within the desired time frame. If it is not so, the whole system may break down. Testing for this is hard, but absolutely essential

1.4 Keeping in mind the various definitions of operating system, consider whether the operating system should include applications such as web browsers and mail programs. Argue both that it should and that it should not, and support your answers.

It should not: Developers of web browsers and mail programs need to be able to compete. If all these things are already included into the OS, it is basically becoming a monopoly and prevents other players from competing. The OS developers should focus on creating the best OS possible, focus primarily on that. The web browser developers should focus on creating the best web browser, directing their energy there. In this case, everybody has a task at which they want to excel and the two teams benefit and help each other. 
It should: Users don’t want to have to install a bunch of programs and for most people convenience and simplicity is the goal when it comes to technology. If you buy a computer and it already has an OS, a browser, a mail client, a music player, ect - you don’t have to look for any programs and install or configure anything, everything is ready for you to use. 

1.5 How does the distinction between kernel mode and user mode function as a rudimentary form of protection (security)?

Some instructions are only executable in kernel mode. Not all users should have access to those and if a user tries to execute a privileged instruction it will trigger a trap (error)

1.6 Which of the following instructions should be privileged? 
a. Set value of timer. 
b. Read the clock. 
c. Clear memory. 
d. Issue a trap instruction. 
e. Turn off interrupts. 
f. Modify entries in device-status table. 
g. Switch from user to kernel mode. 
h. Access I/O device.

Set value of timer. It is an important component of a system and misconfiguring it will affect everything. 
Clear memory. If the user is committing crime with the help of a computer and clears the memory afterwards it might be hard to hold the user accountable. 
Turn off interrupts. Interrupts are crucial to the correct functionality of the system. Turning them off should not be allowed to any user.
Modify entries in device-status table. Rerouting traffic should obviously be privileged.
Access I/O device. If access to I/O is allowed to a malicious process it can seriously damage the system. Thus access to I/O is privileged and can only be allowed via a system call from a user. 

1.7 Some early computers protected the operating system by placing it in a memory partition that could not be modified by either the user job or the operating system itself. Describe two difficulties that you think could arise with such a scheme.

With this setup all passwords and other data that should be protected would have to be stored in unprotected memory thus being accessible to anyone, which is a huge security risk. Moreover, the system would not be updateable and it would be impossible to patch it since it is not modifiable. 

1.8 Some CPUs provide for more than two modes of operation. What are two possible uses of these multiple modes?

The first two modes of operation are user mode and kernel (privileged) mode. Multiple modes could mean multiple user modes or multiple kernel modes. Multiple user mode will provide a tailored security policy, with not all users being “equal”. For instance, a certain group of users has access and modification permissions  to data other users can’t access or modify. Multiple kernel mode is similar, with programs utilizing it being able to avoid switching into kernel mode and running in a quasi user/kernel mode as a result.

1.9 Timers could be used to compute the current time. Provide a short description of how this could be accomplished.

Assuming that through a system call a process can obtain current time, you can start a process and set a timer for later, then let the process sleep. When the process is awakened with an interrupt it will update it’s local state. This can continue as long as interrupts are occurring. 

