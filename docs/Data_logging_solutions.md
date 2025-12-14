# A List of Solutions for Data Logging, Tracing, Testing, and Debugging

Note that this list is not exhaustive. If you are aware of robust logging or testing solutions for real-time embedded systems, or if you notice a mistake in this list, please contribute information to this file.

|||
|:---|:----|
| [RTEdbg toolkit](https://github.com/RTEdbg/RTEdbg) | A fast, flexible C/C++ logging toolkit for real-time embedded system testing and debugging. Optimized for minimal resource use (RAM, program memory, stack). Functions as a reentrant, timestamped `fprintf()` running on the host.|
| [Segger SystemView](https://www.segger.com/products/development-tools/systemview/) |		Commercial tool tied to J-Link debug probes. <br> Data logging via J-Link and SEGGER RTT. Optimized for event logging. Event viewer software included. |
| [Keil MDK Event <br> Recorder](https://www.keil.com/pack/doc/compiler/EventRecorder/html/index.html) |	Commercial tool optimized for event logging and less useful for general embedded system data logging. Includes support for MDK middleware, Keil RTX5. |
| [Percepcio <br> Tracealyzer](https://percepio.com/tracealyzer/) |Commercial tool with powerful data analysis tools. Optimized for event logging. Less suited for real-time control system data logging and analysis.|
|[VxWorks System Viewer](https://learning.windriver.com/vxworks-7-system-viewer) | Commercial tool used to collect instances of pre-configured times tamped events and display them in graphical or tabular format.|
|[Linux Trace Toolkit <br> LTT, LTTng](https://en.wikipedia.org/wiki/Linux_Trace_Toolkit) | Various tools designed to log program execution details from a patched Linux kernel and then perform various analyses on them. Several versions available:  [Dtrace](https://en.wikipedia.org/wiki/DTrace), [ftrace](https://en.wikipedia.org/wiki/Ftrace), [strace](https://en.wikipedia.org/wiki/Strace), etc.|
|[Spdlog](https://github.com/gabime/spdlog)	| Fast C++ logging library (Linux, Windows, Android, macOS, etc.). |
|[RTEMS Trace](https://docs.rtems.org/docs/main/user/tracing/introduction.html) | Logging for RTOS-based systems.|
|[Microsoft TraceX](https://learn.microsoft.com/en-us/azure/rtos/tracex/overview-tracex) | Windows-based system analysis tool for Microsoft Azure RTOS.|
|[QP/Spy <br> Software Tracing](https://www.state-machine.com/qtools/qpspy.html) | Software trace and test system designed specifically for embedded systems.|
|[TRICE](https://github.com/rokath/trice) | Open-source tool and library for efficient logging in real-time embedded systems.|
|[Embedded Logger](https://github.com/martinribelotta/elog)	| Logging solution that moves fixed strings outside the final binary.|
|[μP7 library](https://baical.net/up7.html) | Lightweight C library for sending logs to the host.|
|[NanoLog](https://github.com/PlatformLab/NanoLog)| Fast logging system for C++ with a simple printf-like API - see [article](https://www.usenix.org/system/files/conference/atc18/atc18-yang.pdf). |
| [MCU Logging](https://docs.memfault.com/docs/mcu/logging) | The Memfault SDK offers a simple RAM based logging buffer. See also [Compact Logs](https://docs.memfault.com/docs/mcu/compact-logs). |
|[MCUViewer](https://github.com/klonyyy/MCUViewer) | Open-source GUI debug tool for microcontrollers (Global variable and Trace Viewer).|
|[ESP IDF](https://github.com/espressif/esp-idf/blob/v5.2.1/docs/en/api-guides/app_trace.rst) | ESP Application Level Tracing Library.|
|[Dictionary-based <br> Logging](https://docs.zephyrproject.org/3.1.0/services/logging/index.html#dictionary-based-logging) | Zephyr Dictionary-based Logging.|
|[elog](https://github.com/martinribelotta/elog) | Logging for embedded systems with mininmal resource utilization. |
|[COVESA DLT](https://github.com/COVESA/dlt-daemon)	| Log and trace based on the protocol from the AUTOSAR Classic Platform.|
|[Traces](https://github.com/yotamr/traces) | API tracing framework for Linux C/C++ applications.|
| [defmt](https://github.com/knurling-rs/defmt) | A highly efficient logging framework that targets resource-constrained devices, like microcontrollers. |
| [embedded diagnostic logger](https://github.com/binarymaker/embedded-diagnostic-logger) | Lightweight logger framework for small microcontroller based projects. |
| [EasyLogger](https://github.com/armink/EasyLogger) | A ultra-lightweight, high-performance C/C++ log library. |
| [embedded log](https://github.com/to9/embedded-log) | A  small and beautiful embedded log library for mcu. |
| [ulog](https://github.com/rdpoor/ulog) | Lightweight logging for embedded microcontrollers. |
| [log.c](https://github.com/rxi/log.c) | A simple logging library implemented in C99. |
| [pw_tokenizer](https://pigweed.dev/pw_tokenizer/) | Compress strings to shrink logs. |
| [quill](https://github.com/odygrd/quill) | Asynchronous Low Latency C++ Logging Library |
| | |

Logging tools differ significantly in their **flexibility**, **intrusiveness**, **reentrancy**, **resource usage**, suitability for **bare-metal** and/or **RTOS-based systems**, **cost**, and more. Many of these tools are primarily designed for event logging, making them less suitable for general embedded system data logging. Additionally, they are often only partially configurable.

Many `printf-like` logging solutions offload data decoding to a host system instead of performing it directly on the embedded device. This approach often requires transferring the printf-like strings to the host, which increases the demand on the data-logging buffer and can slow down the logging process. Additionally, many logging functions are not reentrant or they require a significant amount of stack space. Some solutions also use inefficient data encoding, which further contributes to the need for larger data-logging buffers.

The list includes tools that are not full-fledged logging solutions but can assist with the implementation of logging or testing of embedded systems.

**Use a solution that meets your requirements.** Some solutions are more optimal but require a bit more learning. This initial effort is often worthwhile, as the same solution can be applied to a large number of projects.