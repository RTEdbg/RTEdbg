# Overview of other data logging/tracing and debugging solutions

The table below lists some of the data logging and analysis tools (mostly open source).

|||
|:---|:----|
| [Segger SystemView](https://www.segger.com/products/development-tools/systemview/) |		Commercial tool tied to J-Link debug probes. <br> Data logging via J-Link and SEGGER RTT. Optimized for event logging. Event viewer software included. |
| [Keil MDK Event <br> Recorder](https://www.keil.com/pack/doc/compiler/EventRecorder/html/index.html) |	Commercial tool optimized for event logging and less useful for general embedded system data logging. Includes support for MDK middleware, Keil RTX5. |
| [Percepcio <br> Tracealyzer](https://percepio.com/tracealyzer/) |Commercial tool with powerful data analysis tools. Optimized for event logging. Less suited for real-time control system data logging and analysis.|
|[National Instruments <br> Real-Time Trace](https://www.ni.com/docs/en-US/bundle/labview-2021-real-time-module/page/lvtracehelp/lv_tracetoolkit_help.html) | Commercial tool. The Real-Time Trace Viewer displays the time and event data, or trace session, on the host computer.|
|[VxWorks System Viewer](https://learning.windriver.com/vxworks-7-system-viewer) | Commercial tool used to collect instances of pre-configured times tamped events and display them in graphical or tabular format.|
|[Linux Trace Toolkit <br> LTT, LTTng](https://en.wikipedia.org/wiki/Linux_Trace_Toolkit) | Various tools designed to log program execution details from a patched Linux kernel and then perform various analyses on them. Several versions available:  [Dtrace](https://en.wikipedia.org/wiki/DTrace), [ftrace](https://en.wikipedia.org/wiki/Ftrace), [strace](https://en.wikipedia.org/wiki/Strace), etc.|
|[Spdlog](https://github.com/gabime/spdlog)	| Fast C++ logging library|
|[RTEMS Trace](https://docs.rtems.org/branches/master/user/tracing/introduction.html) | Logging for RTOS-based systems|
|[Microsoft TraceX](https://learn.microsoft.com/en-us/azure/rtos/tracex/overview-tracex) | Windows-based system analysis tool for Microsoft Azure RTOS|
|[QP/Spy <br> Software Tracing](https://www.state-machine.com/qtools/qpspy.html) | Software trace and test system designed specifically for embedded systems.|
|[TRICE](https://github.com/rokath/trice) | Open-source tool and library  for efficient logging in real-time embedded systems.|
|[Embedded Logger](https://github.com/martinribelotta/elog)	| Logging solution that moves fixed strings outside the final binary|
|[μP7 library](https://baical.net/up7.html) | Lightweight C library for sending logs to the host|
|[NanoLog](https://github.com/PlatformLab/NanoLog)| Fast logging system for C++ with a simple printf-like API - see [article](https://www.usenix.org/system/files/conference/atc18/atc18-yang.pdf) |
|[MCUViewer](https://github.com/klonyyy/MCUViewer) | Open-source GUI debug tool for microcontrollers (Global variable and Trace Viewer)|
|[ESP IDF](https://github.com/espressif/esp-idf/blob/v5.2.1/docs/en/api-guides/app_trace.rst) | ESP Application Level Tracing Library|
|[Dictionary-based <br> Logging](https://docs.zephyrproject.org/3.1.0/services/logging/index.html#dictionary-based-logging) | Zephyr Dictionary-based Logging|
|[elog](https://github.com/martinribelotta/elog) | Logging for embedded systems with mininmal resource utilization |
|[COVESA DLT](https://github.com/COVESA/dlt-daemon)	| Log and trace based on the protocol from the AUTOSAR Classic Platform|
|[Traces](https://github.com/yotamr/traces) | API tracing framework for Linux C/C++ applications|
| | |

Most of these tools are primarily designed for event logging, making them less suitable for general embedded system data logging. Additionally, they are often only partially configurable. Many toolkits offload printf-like data decoding to the host system rather than performing it directly within the embedded system. However, in most cases, the printf-like strings need to be transferred to the host, which increases the demand on the data-logging buffer and slows down the logging process. Furthermore, many logging functions require significant stack space. Some solutions also employ inefficient data encoding, leading to the need for larger data-logging buffers. Most of these tools offer only basic print-style formatting support.