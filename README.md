# Enhanced XV6
**XV6 with Different Scheduling Algorithms**

This project enhances XV6 by implementing new scheduling algorithms and instrumentation. New system calls are added for retrieving parent/child process info and system call usage statistics. "Round-Robin", "Priority", and "Multilevel Queue" scheduling algorithms are added to the original os. Instrumentation tracks turnaround time, response time, and other metrics to analyze the new schedulers. Test cases compare scheduler performance using the new instrumentation. 

The project was completed as the final project for the operating system course at Amirkabir University of Technology. The full documentation for the project can be found in [instructions.pdf](docs/instructions.pdf) and [report.pdf](docs/report.pdf).

## Part 1: Adding New System Calls

### getParentID

This system call returns the process ID of the parent process of the calling process.

The `getParentIDtest.c` file is created to test this system call. It uses `fork()` to create child processes, and each process prints its own ID and its parent's ID.

Example output:

```
This is process 7 and the parent id is 5
```

### getChildren 

This system call returns a list of the PIDs of all child processes of the calling process. For example, if process 5 has child processes 6, 7, 8, and 9, it would return "6/7/8/9".

The `getChildrenTest.c` file tests this system call. The parent process prints out its children after forking multiple times.

Example output:

```
This is process 5 and children are 6/7/8/9
```

### getSyscallCounter

This system call gets a system call ID as input and returns the number of times that system call has been called by the current process. The system call IDs are defined in `syscall.h`. 

This is implemented by modifying the process data structure in `proc.h` to add counters for each system call.

The `getSyscallCounterTest.c` file tests it.

## Part 2: Adding Scheduling Algorithms 

### Round Robin Scheduling

Round Robin scheduling switches processes frequently, giving each process a time slice (quantum) before switching.

A `QUANTUM` parameter is added to `param.h` to configure the quantum.

### Priority Scheduling

In priority scheduling, each process is assigned a priority. Higher priority processes are scheduled first.

Priorities from 0 to 6 are supported. 3 is the default. Invalid priorities are changed to 5.

The process structure in `proc.h` is modified to store priority. A `setPriority` system call sets a process's priority.

### Multilevel Queue Scheduling 

In multilevel queue scheduling, processes are assigned to different queues based on priority. Each queue can use a different scheduling algorithm.

4 queues are created here. Queue 1 uses the default XV6 scheduler. Queues 2-4 use priority, reverse priority, and round robin scheduling respectively.

## Part 3: Testing Scheduling Algorithms

### changePolicy System Call

The `changePolicy` system call allows dynamically switching between the different scheduling algorithms implemented in this project.

It takes an integer parameter that specifies the scheduling algorithm:

- 0 - Default XV6 scheduling
- 1 - Round Robin
- 2 - Priority Scheduling
- 3 - Multilevel Queue

- This system call updates a global scheduler policy variable that the scheduler checks on each tick to determine which algorithm to use.

### Measuring Process Statistics

The goal of measuring process statistics like creation time, running time, etc is to be able to compare the performance of different scheduling algorithms.

The following fields are added to the proc struct in proc.h:

- createtime - System tick when process was created
- terminatetime - System tick when process terminated
- cputime - Total CPU time used by process
- readytime - Time process spent waiting in ready queue
- sleepingtime - Time process spent sleeping

On each timer interrupt, the scheduler updates these fields as needed.

The test cases will use these measurements to calculate and compare metrics like turnaround time, waiting time, and response time for the different scheduling algorithms.

### Test Cases

- `roundrobintest.c`
- `prioritySchedTest.c`  
- `multiLayeredQueueTest.c`

Test the new scheduling algorithms.

## Course Information
- **Course**: Operating Systems
- **University**: Amirkabir University of Technology  
- **Semester**: Fall 2020

Let me know if you have any questions!
