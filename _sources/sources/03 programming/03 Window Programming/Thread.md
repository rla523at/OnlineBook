# Thread

* Thread Stack Size
  * https://learn.microsoft.com/en-us/windows/win32/procthread/thread-stack-size
  * Each new thread or fiber receives its own stack space **consisting of both reserved and initially committed memory**.
  * The reserved memory size represents the total stack allocation in virtual memory.

* _beginthreadex 
  * https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/beginthread-beginthreadex?view=msvc-170
  * STACK_SIZE_PARAM_IS_A_RESERVATION flag to use stack_size as the initial reserve size of the stack in bytes; if this flag isn't specified, stack_size specifies the commit size.

* https://learn.microsoft.com/en-us/windows/win32/procthread/creating-threads

* Delegate
  * https://www.codeproject.com/Articles/1170503/The-Impossibly-Fast-Cplusplus-Delegates-Fixed



