# Programming Assignment (Readers and Writer)
Process communication using shared memory involves at least two processes by definition, often referred to as a Reader and a Writer.  The programming assignment for this lab is to implement a Writer program that creates a shared memory segment, then repeatedly accepts an arbitrary user input string from the terminal and writes it into shared memory.  You must also implement a corresponding Reader program; two instances of it will attach to the same shared memory segment.  The Readers will repeatedly read each new string from the shared memory and display the string to the screen.  The idea is similar to the "echo" command, or to the operation of the pipe Sample Program in Lab 3, except that your solution must implement two Reader processes.  In this sense, it functions a little like the typescript utility, which sends output to both the display and to a file.  Your program sends output to two different window displays.

Write separate programs for each process type and then run each process in its own terminal window.  This clearly demonstrates the communication mechanism involved in using shared memory.

# Observations:
* the Writer needs to know that the current string has been displayed, so that it may write a new string into shared memory (otherwise it would overwrite the first string).  The Readers need to know when a new string has been written into shared memory, so that they can display it (otherwise they would re-display the current string).  The solution to this problem does not require sophisticated synchronization mechanisms such as semaphores or mutexes (the topic of a future lab).  You should be able to synchronize for the desired behavior with simple elements such as flag(s) and/or turn variables, which must also be stored in shared memory so that all processes can access them.  Note that the functionality of this system constitutes a type of "lockstep" synchronization, which is acceptable in this case since that is the desired behavior of the programs (writer goes, then both readers go, then writer goes, etc.).
* the Readers need to know which segment to attach to.  The preferred method is for the Readers and Writer to agree upon a passkey that is used to get access to the resource.  See the man pages for information on using the function ftok() (file_to_key), used to generate a common key.
* your programs should conclude with a graceful shutdown, releasing all system resources before terminating (this would be a good use of the signal() mechanism introduced in a previous lab).
* _Important:_ don't lose your shared memory pointer!  Otherwise, you will not be able to properly detach.

# Extra Credit (Chat Server)
Extend your program to function as a Chat Server, albeit one that executes on a single machine.

The basic idea:  instead of using one writer and two readers, each instantiated process is both a writer and a reader.  Anything a user types into any process in any terminal window gets echoed to all other processes.  Note: this is an O.S. project.  Your solution must use shared memory for IPC (not sockets).

Reminder:  all extra credit is in addition to the regular assignment, not in place of it.
