# The Abstraction: The Process [CHAPTER]

The definition of a process, informally, is quite simple: it is a running program.


## Crux [CRUX]

Although there are only a few physical CPUs available, how can the
OS provide the illusion of a nearly-endless supply of said CPUs?


### Mechanisms [TERMIN]
 
Mechanisms are low-level methods or protocols that implement a needed piece of functionality.

- Context Switching mechanism;
- Time-sharing mechanism;


### Policy [TERMIN]

On top of these mechanisms resides some of the intelligence in the OS, in the form of policies.

- Scheduling policy;


## The Abstraction: A Process [SUBCHAPTER]


### Machine State [TERMIN]

Machine state constitutes a process.

- Memory (address space);
- Registers (PC, SP, BP, …)
  - PC (program counter) tells us which instruction of the process will execute next;
- I/O information
  - List of opened files of process;


## Process API [SUBCHAPTER]

- Create – create new process;
- Destroy – destroy process;
- Wait – wait till process is stopped;
- Miscellaneous Control – suspend process, resume process execution;
- Status – get some information of process;


## Process Creation: A Little More Detail [SUBCHAPTER]

1. OS must load program's code and any static data into memory (address space);
  - In early OS the loading process is done eagerly – before running the program;
  - Modern OSes perform the process lazily – only when they needed during program execution;
2. Memory allocation for stack, heap.
3. Some I/O stuff.


## Process States [SUBCHAPTER]

- Running – executing the instruction;
- Ready – process id ready to run but OS hasn't scheduled it yet;
- Blocked – process had performed some kid of operation that makes it not ready to run until some other event takes place;


## Data Structures [SUBCHAPTER]


### Process List [TERMIN]

Process list keeps track of processes.


### Process Control Block [TERMIN]

Process Control Block (PCB, Process descriptor) – structure with information about process.

Example of PCB in xv6 OS:
```c
struct context {
  uint edi;
  uint esi;
  uint ebx;
  uint ebp;
  uint eip;
};

enum procstate { UNUSED, EMBRYO, SLEEPING, RUNNABLE, RUNNING, ZOMBIE };

// Per-process state
struct proc {
  uint sz;                     // Size of process memory (bytes)
  pde_t* pgdir;                // Page table
  char *kstack;                // Bottom of kernel stack for this process
  enum procstate state;        // Process state
  int pid;                     // Process ID
  struct proc *parent;         // Parent process
  struct trapframe *tf;        // Trap frame for current syscall
  struct context *context;     // swtch() here to run process
  void *chan;                  // If non-zero, sleeping on chan
  int killed;                  // If non-zero, have been killed
  struct file *ofile[NOFILE];  // Open files
  struct inode *cwd;           // Current directory
  char name[16];               // Process name (debugging)
};
```
