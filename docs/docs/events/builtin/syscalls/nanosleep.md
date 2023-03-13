
# nanosleep

## Intro
nanosleep - Suspend execution of the calling thread for whatever is specified by req.

## Description
nanosleep is used to suspend execution of the calling thread until either the time interval specified in the structure pointed to by req has passed or the delivery of a signal which triggers the invocation of a signal-catching function. This structure value remains in effect until nanosleep finishes, it will not currently adjust the timer in case of interrupted by a signal handler. There are some edge cases and inadequacies related to the timing of nanosleep and how it is affected by clock changes, system load and other processes.

## Arguments
* `req`:`const struct timespec*`[K] - Pointer to a structure that specifies an interval of time, of type struct timespec, to pause execution of the calling thread.
* `rem`:`struct timespec*`[KU] - Pointer to a structure that shall receive the time still remaining of the interval specified in req, of type struct timespec.

### Available Tags
* K - Originated from kernel-space.
* U - Originated from user space (for example, pointer to user space memory used to get it)
* TOCTOU - Vulnerable to TOCTOU (time of check, time of use)
* OPT - Optional argument - might not always be available (passed with null value)

## Hooks
### do_sys_nanosleep
#### Type
kprobe
#### Purpose
To find the overhead of nanosleep invocations.

## Example Use Case
nanosleep can be used to sleep for a specified length of time to produce a slowdown for the process that is being profiled. It can also be used to yield the CPU running time to other threads and processes.

## Issues
Since nanosleep is based on the system’s kernel jiffies counter, its accuracy can be affected by system load, other processes and clock changes. This may cause nanosleep to return earlier or later than requested.

## Related Events
syscall_entry (for nanosleep) - Event that is triggered when a syscall is entered.  
syscall_exit (for nanosleep) - Event that is triggered when a syscall is exited.

> This document was automatically generated by OpenAI and needs review. It might
> not be accurate and might contain errors. The authors of Tracee recommend that
> the user reads the "events.go" source file to understand the events and their
> arguments better.