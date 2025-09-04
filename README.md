## Real-time binary data logging/tracing toolkit

![LogoRTEdbg–small](https://github.com/RTEdbg/RTEdbg/assets/144953452/e123f541-1d05-44ca-a85e-34a7abeded22)

**RTEdbg is a real-time firmware analysis, testing, and debugging toolkit.** It provides a lightweight library for minimally intrusive code instrumentation (logging and tracing), utilities for transferring recorded data to the host, and a host-side decoder for interpreting the data. Demo applications are included to illustrate typical use cases. The toolkit supports logging of any data type - from simple events and status flags to complex data structures and even full core dumps.

This solution empowers embedded programmers to efficiently test and debug C/C++ firmware. It addresses a wide range of complex challenges, from optimizing system performance and identifying bottlenecks to performing detailed timing analysis. Beyond conventional debugging, it is invaluable for reverse engineering poorly documented code and capturing elusive, hard-to-reproduce issues that traditional methods often miss. By streamlining development cycles and enhancing code reliability, it helps you deliver robust, high-quality products faster.

The **RTEdbg** library and tools are designed for both large, hard real-time RTOS-based systems and small, resource-constrained devices. Unlike traditional debuggers, they offer deeper visibility into real-time system behavior without halting execution. Stopping and restarting a real-time system alters its operating conditions, often producing inconsistent results—and in some cases, loss of control may even lead to system damage. 

View the **[RTEdbg Presentation](https://github.com/RTEdbg/RTEdbg/releases/download/Documentation/RTEdbg.Presentation.pdf)** to learn about the key benefits and to see the basic features.

**Note:** This is the main repository used to distribute the RTEdbg toolkit. <br> See the **[List of repositories](#Repository-Structure)** that are part of the RTEdbg toolkit. <br> These repositories contain the logging library, tools, and demo code.

### **Newsflash** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&Rightarrow; See also: **[News](https://github.com/RTEdbg/RTEdbg/blob/master/docs/NEWS.md)**
* Documentation updated
* Added [code size optimized ARM Cortex-M4/M7 fault handler](https://github.com/RTEdbg/RTEdbgDemo/blob/master/STM32L433/RTEdbg/Demo_code/Simple_Cortex_M4-M7_fault_handler.md) demo.

## Table of contents:
* [Introduction](#Introduction)
* [Getting Started](#Getting-Started)
* [RTEdbg Schematic Presentation](#RTEdbg-Schematic-Presentation)
* [A Practical Guide to Using the RTEdbg Toolkit](#a-practical-guide-to-using-the-rtedbg-toolkit)
* [Articles on Code Instrumentation, Logging, and Tracing](#articles-on-code-instrumentation-logging-and-tracing)
* [Repository Structure](#Repository-Structure)
* [Demo Projects](#Demo-Projects)
* [Installation Instructions](#Installation-Instructions)
* [How to Contribute or Get Help](#How-to-Contribute-or-Get-Help)
* [About the Author](#About-the-Author)

## Introduction
The solution was designed with a focus on **maximum execution speed, low memory and stack requirements, and portability**. It is essentially a **reentrant timestamped *fprintf()* function that runs on the host instead of the embedded system.** Flexible filtering and sorting of data into user-defined files helps manage the flood of data from the embedded system. Any part of the logged data (message) can be printed to any number of output files in any format, from an ordinary log file to CSV. For example, the data can be directly exported to one or more CSV files for graphing and/or data analysis using spreadsheet software.

The new data logging/tracing solution is not a direct replacement for existing event-oriented solutions such as SystemView or Tracealyzer, as it is based on a different concept. It is optimized from ground up for minimally intrusive logging and flexible data decoding. Flexible decoding (printing) of data enables analysis of log files with existing tools (log viewers, graphing and event analysis tools). The code is optimized for 32-bit devices. For example: On a Cortex-M4 device, logging a simple event with the standard logging function version requires only about 35 CPU cycles and 4 bytes of stack. For the [SystemView](https://www.segger.com/downloads/jlink/UM08027_SystemView.pdf) nearly 200 CPU cycles and a maximum of 150 to 510 bytes of stack are required for event generation and encoding. 

The **inline data logging function versions enable faster execution and reduced stack usage** compared to the regular function versions. However, this comes at the cost of increased program memory usage. A practical approach is to use the inline versions only in the most time-critical sections of the code and the regular versions elsewhere.

The **RTEdbg library** imposes only a minimal program memory footprint. For a typical instrumented embedded application, the overhead amounts to a few hundred bytes. Logging also introduces extra function calls for data capture and parameter preparation (typ. 8 bytes for a simple event on a Cortex-M device), though both are optimized to minimize instruction count and execution latency. Format strings for printf-style output are not stored on the target; they reside on the host, where the binary log data decoding (printing) is performed.

**Code instrumentation is minimally intrusive** because raw binary data is logged along with an automatically assigned format definition ID (transparent to the programmer) and timestamp. Raw binary data logging also minimizes circular buffer requirements. **Any data type or entire structures/buffers can be logged and decoded, including bit fields and packed structures.**

**Data logging can be integrated in any part of the code**, including **interrupt routines** and the **RTOS kernel**. If the processor core supports **atomic operations**, it is safe to include logging in **NMI** and **fatal exception handlers** as well.

**Any debug probe, communication channel, or media can be used to transfer data to the host.** Because data is logged in raw binary form, bandwidth requirements are low. The [**RTEgetData**](https://github.com/RTEdbg/RTEgetData) tool for transferring binary log data to a host using the debug probe GDB server is part of the RTEdbg project.

**The toolkit can be used for any type of project**, from small resource-constrained to large RTOS-based, including hard real-time and functional safety. Low stack requirements virtually eliminate the possibility of stack overflows after existing code instrumentation. Data logging functions are reentrant and do not disable interrupts if the microcontroller core supports the exclusive access instructions (atomic operations). 
Refer to the [**A Practical Guide to Using the RTEdbg Toolkit**](#a-practical-guide-to-using-the-rtedbg-toolkit) for information on how and where the toolkit can be used.

Using this library avoids all the problems of non-reentrant printf-style debugging modes. All format strings are stored only on the host computer, where the decoding of the logged data is performed. Since the data is recorded in binary form, the number of functions is very small and it is possible to learn how to use them quickly. Format definitions are printf-style strings, so they are familiar to programmers. The format strings implement a superset of standard printf functionality; syntax extensions enable printing of packed structures, bitfields, core dumps, statistics, timing analysis, value scaling, etc.

## Getting Started
Complete documentation can be found in the **[RTEdbg Manual](https://github.com/RTEdbg/RTEdbg/releases/download/Documentation/RTEdbg.library.and.tools.manual.pdf)**. See the **GETTING STARTED GUIDE** section for quick start instructions and **INTRODUCTION AND KEY FEATURES** for a look at the basic features and benefits. The basic features are all that's necessary for simple projects. To use the toolkit to its full potential, check the manual to see what options are available.
A ZIP file containing the complete toolkit, documentation and selected demo projects is available on the **[download page](https://github.com/RTEdbg/RTEdbg/releases)**. See also **[List of demo code](#Demo-Projects)**.

## RTEdbg Schematic Presentation ##
![RTEdbg_presentation](https://github.com/RTEdbg/RTEdbg/assets/144953452/0b84dfc4-6e0f-4aec-aa1c-3fe04686e99e)

The schematic shows how data logging can be integrated into embedded system firmware. Only a C source file with logging functions and some header files need to be added to the project. Any C or C++ code can be instrumented – including drivers, exception handlers, and RTOS kernel. The logging functions save raw binary data in a data structure in RAM without using time- or buffer-consuming data tagging and/or encoding. The core of this data structure is a buffer that operates either as a linear buffer in single-shot logging mode or as a circular buffer otherwise. All data is processed and stored in 32-bit chunks. This design ensures that the operation is extremely fast and efficient on 32-bit processors.

Any interface or media can be used to transfer data to the host for decoding and analysis, since only the contents of a data structure need be transferred. The captured data can be transferred to the host, decoded, and analyzed while the target code is running normally. 

The **RTEmsg utility** performs offline decoding of binary data. Logged messages are decoded (printed) to one or several files using fprintf-style format definitions (the are defined in C header files). The data logged with a log function call can hold multiple values, and each of them can be printed to any number of files using different format definitions (different print strings). This allows the decoded information to be sorted so that the important information is collected together (e.g. 'warnings.log') and can be quickly reviewed by the tester (rather than having to search through a long main log file). This tool not only decodes and sorts the recorded data, but also generates statistical report for the decoded values and times, allowing testers to quickly find extreme values. If errors are found in the binary file or in the format definition files, they are reported in the 'Errors.log' file.

Real-time systems generate a flood of data, and it is important to sort the data before analyzing it to quickly identify potential problems (data extremes, errors, warnings). Programmers specify how the logged data is printed and into how many output files it is sorted (printed). The printf output goes to the 'Main.log' file by default. Important messages or values can be written to more than one output file, each value in a different format. Existing data and event analysis tools can be used because flexible data formatting allows adaptation to the needs of existing data/event analysis or graphing tools.

**Note:** In the RTEdbg documentation, the term **message** refers to any type of data or dataset written to the data logging buffer via a data logging function call.

## A Practical Guide to Using the RTEdbg Toolkit

Debuggers only provide very slow real-time monitoring of variable values (e.g., live view). Some tools allow sampling of global variables at frequencies up to a few kHz, but only asynchronously and with limited history. With this method, it is unclear whether two values are from the same control cycle. Although instrumenting the code requires additional effort, it allows synchronous data logging and plenty of history.

To analyze and optimize a complex system, it is necessary to log the values processed by the firmware (e.g., control algorithm data, requests received via communication, firmware decisions, sequencer state changes, alarms). It makes a lot of sense to include the logging of exceptional events, because in such a case the dump information of the processor's working and system registers, as well as the history of previously logged events, is very useful. In the demo code, there are examples of exception logging for processors of the Cortex-M0, M0+, M4 and M7 families. They can easily be adapted for other CPU cores.

System architects and programmers must consider which method is best for testing and debugging. This decision depends on the complexity of the project. Even for smaller projects, code instrumentation is useful because it provides test points in the code, enabling testing and debugging even when the debugger probe is not connected to the embedded system. Function libraries, operating systems, peripheral drivers, and so on can be equipped with logging and used in a large number of projects. 

The logs included in the project are like test plugs for the hardware. Each group of messages is like a communication channel through which certain types of information are available. Up to 32 groups can be defined, enabled, or disabled on the fly using the message filter. This allows you to choose between a shorter, more detailed history and a longer, less detailed one. We can only connect those that are of interest to us during the current test. Both the amount of memory for the circular buffer and the bandwidth for data transfer to the host are limited. Flexible on-the-fly message filtering allows the tester to select the most relevant data for the current test.

**The RTEdbg toolkit is well suited for:**
* **Bare-metal** and **RTOS-based systems** projects that require **minimally intrusive instrumentation** - including hard real-time and functional safety projects.
* **Resource-constrained systems** or systems where the **programmer wishes to avoid print-style testing** due to slow execution, memory constraints, and re-entrancy issues. The code has been optimized for a minimal total program memory footprint.
* Projects where the focus is on the **custom data processed by the firmware** (e.g., control system optimization). Any type of data can be logged and decoded, including control algorithms and exception handlers (core dumps). 
* Projects where the **code execution should not be stopped** - e.g. real-time control or communication systems where our system must respond within a timeout window (most communication protocols have timeouts).
* **Timing analysis** since avery logged message is also a timing marker (start and/or stop). The time between events of the same or different types can easily be printed and statistically processed to obtain minimum and maximum values.
* Projects where we want to **export the results directly to a file for automated processing** (e.g. CSV file for automated regression testing).
* Projects that produce a **flood of data**, since the `fprintf()` functionality enables the data to be sorted into separate files, allowing for faster problem-finding and analysis. The most important information can be exported to several machine- or human-readable files. <br>
With the **message filter**, it is possible to enable up to 32 groups of messages on the fly. Messages can be selectively logged or discarded. This allows for either a shorter, more detailed history or a longer, less detailed one.
* **Optimization and analysis of complex embedded systems** where it is difficult to find the root cause of performance degradation.
* Projects where we want to **keep logging in production code** so that problem analysis is possible even after the systems are in the field.
* **IoT systems** where we want to enable debugging even after delivery to customers (e.g. execution history, post-mortem debugging). Only binary data is logged and the logging memory consumption is low, so the data consumption for paid data transfer is low.
* Project for which **flexible post-mortem analysis** is required. Custom triggers can be used to stop logging. Logging can be frozen after an event that causes the system to shut down. System can be restarted and continue operation, and service technician or designer can connect to system upon arrival at site or remote connection to retrieve logged data (embedded system operates normally, only logging is frozen). Logged content can be preserved while code continues to run normally.

<br>

**Low stack requirements virtually eliminate the possibility of stack overflows when instrumenting code.** The data logging code has also been optimized to minimize the total memory footprint of the program, thereby reducing the burden of code instrumentation. <br> This makes the toolkit ideal for:
* **Analyzing complex, difficult-to-reproduce problems** in existing projects.
* **Reverse engineering poorly documented code.**
* **Analyzing third-party code** - for example, analyzing software stacks (black boxes) in application code. Logged data helps us understand the behavior of the tested code, the overall system, and influences from the environment or other systems.

<br>

The following list contains **cases where the RTEdbg toolkit is currently less suitable** because RTOS support and support for commercial event analysis tools are not yet implemented:
* For projects where we want to use event analysis tools like Tracealyzer or SystemView. 
* Projects where we want to include ready-made RTOS trace hooks. RTEdbg is a new tool and so far support and demo for popular RTOS and for event analysis tools like Eclipse Compass is not yet done.

Solutions such as the Tracealyzer or SystemView toolkits have their own powerful event analysis tools. Their data logging functions are slower and consume more stack space. The slower code execution is usually not a problem for most projects. However, additional stack space is required for each RTOS task from which the data logging functions are called, as well as for interrupt handlers and drivers. These tools are optimized for event analysis, but are less flexible for logging and analyzing application-specific data.

<br>

**Note:** The project never envisioned the development of tools for viewing and analyzing log data and events. Flexible data formatting allows viewing and analysis with most existing tools that support ASCII or UTF-8 based file formats. Generation of binary output files is not supported. The exception is writing binary data from messages directly to a binary output file.
## Articles on Code Instrumentation, Logging, and Tracing

* [Improving Firmware Quality with Instrumentation - Part 1: Benefits and Limitations](https://www.embedded.com/improving-firmware-quality-with-instrumentation-part-1-benefits-and-limitations/) &rarr; *embedded.com*
* [Improving Firmware Quality with Instrumentation - Part 2: The RTEdbg Toolkit](https://www.embedded.com/improving-firmware-quality-with-instrumentation-part-2-the-rtedbg-toolkit/) &rarr; *embedded.com*

<br>

## Repository Structure
The code and documentation for the libraries and tools can be found in the following repositories.

| Repository | Description |
|:---:|:-----------|
| [**RTEdbg**](https://github.com/RTEdbg/RTEdbg) | Main repository used to distribute the RTEdbg toolkit - see the **[Releases - download page](https://github.com/RTEdbg/RTEdbg/releases)**. This repository does not contain any source code. See other repositories below for library and tool source code. |
| [**RTElib**](https://github.com/RTEdbg/RTElib) | A re-entrant library of functions for minimally intrusive code instrumentation (data logging and tracing). Functions collect data in a circular buffer in RAM.  | 
| [**RTEgetData**](https://github.com/RTEdbg/RTEgetData) | A tool for transferring binary log data to a host using a COM port or a GDB server. This utility replaces RTEgdbData. Other ways to transfer data to the host are described in the RTEdbg manual.|
| [**RTEmsg**](https://github.com/RTEdbg/RTEmsg) | Offline binary data decoding application that runs on the host. |
| [**RTEcomLib**](https://github.com/RTEdbg/RTEcomLib) | Functions for log data transfer to host via serial channel. |
| | |

## Demo Projects
The code and additional documentation for demo projects and exception handlers can be found in the following repositories.

| Repository | Description |
|:---:|:-----------|
| **[Simple_STM32H743 demo](https://github.com/RTEdbg/RTEdbgDemo/tree/master/Simple_STM32H743)** | Simplest version of demo code for the NUCLEO-H743ZI/ZI2 (ARM Cortex-M7) - STM32CubeIDE, IAR EWARM, and Keil MDK IDE. <br> This repository was used as a reference to demonstrate which **RTEdbg library** files need to be added to a project and what changes must be made to the project settings. For more details, refer to the project description in the **RTEdbg manual**, section **Simple Demo Project**. |
| **[STM32H743 demo](https://github.com/RTEdbg/RTEdbgDemo/tree/master/STM32H743)** | Demo code for the NUCLEO-H743ZI/ZI2 (ARM Cortex-M7) - STM32CubeIDE, IAR EWARM, and Keil MDK IDE |
| **[STM32L433 demo](https://github.com/RTEdbg/RTEdbgDemo/tree/master/STM32L433)** | Demo code for the NUCLEO-L433 (ARM Cortex-M4) - STM32CubeIDE, IAR EWARM, and Keil MDK IDE |
| **[STM32L053 demo](https://github.com/RTEdbg/RTEdbgDemo/tree/master/STM32L053)** | Demo code for the NUCLEO-L053 (ARM Cortex-M0+) - STM32CubeIDE, IAR EWARM, and Keil MDK IDE |
| **[lpcxpresso54628_hello_world](https://github.com/RTEdbg/RTEdbgDemo/tree/master/lpcxpresso54628_hello_world)** | Demo code for the NXP LPCXpresso54628 (ARM Cortex-M0+) - MCUXpresso IDE |
| [**RTEcomLib_NUCLEO_C071RB_Demo**](https://github.com/RTEdbg/RTEcomLib_NUCLEO_C071RB_Demo) | Demo code for the RTEcomLib library running on the STM NUCLEO-C071RB demo board (Cortex-M0+). The **RTEcomLib** library allows the transfer of log data to a host system via a serial communication channel. The provided demo includes a simple and efficient exception handling/logging demonstration for devices based on the Cortex-M0/M0+ core. |
| **[ARM Cortex-M0/M0+ <br> Exception handler demo](https://github.com/RTEdbg/RTEcomLib_NUCLEO_C071RB_Demo/blob/master/Exception_handler_Cortex-M0.md)** | The demo shows a simple way to log the ARM Cortex-M0/M0+ registers and the stack when a system exception or an unhandled interrupt has occurred. This core dump version is suitable for bare-metal projects (projects without a RTOS). Only 40 bytes of additional code are needed for a core and stack dump. |
| **[ARM Cortex-M4/M7 <br> Exception handler demo](https://github.com/RTEdbg/RTEdbgDemo/blob/master/Simple_STM32H743/RTEdbg/Demo_code/Cortex_M4-M7_fault_handler.md)**  | The demo shows how to log the ARM Cortex-M4/M7 CPU core and system registers when a system exception or an unhandled interrupt has occurred. This core dump version is suitable for both bare-metal and RTOS-based projects. |
| **[Code size optimized ARM Cortex-M4/M7 <br> Exception handler demo](https://github.com/RTEdbg/RTEdbgDemo/blob/master/STM32L433/RTEdbg/Demo_code/Simple_Cortex_M4-M7_fault_handler.md)**  | The demo shows how to log the ARM Cortex-M4/M7 CPU core and system registers when a system exception or an unhandled interrupt has occurred. This core dump version is suitable for bare-metal projects (projects without a RTOS). Only 32 bytes of additional code are needed for a complete core dump. |
| | |

Some of the demo projects are also included in the distribution ZIP file - see **[Releases](https://github.com/RTEdbg/RTEdbg/releases)**. Each demo folder for the STM32 CPU family contains the complete setup for the STM32CubeIDE (main folder), IAR EWARM (EWARM subfolder) and for the Keil MDK (MDK-ARM subfolder) toolchains. Not all compiler/linker settings are the same for different IDEs. The demo projects also include a demonstration of how to transfer data from the embedded system to the host using the RTEgetData utility (see \"TEST RTEgetData\" folders) or using the tools that are part of the Debug Probe software.

## Installation Instructions

The full toolkit can only be downloaded from **[download page](https://github.com/RTEdbg/RTEdbg/releases)**. The toolkit is currently available for Windows only. Extract the ZIP file into the `c:\RTEdbg` folder. This will ensure that the correct paths are used in the demo projects.

## How to Contribute or Get Help
Follow the [Contributing Guidelines](https://github.com/RTEdbg/RTEdbg/blob/master/docs/CONTRIBUTING.md) for bug reports and feature requests regarding the RTEdbg library. Follow the [Discussions Guidelines](https://docs.github.com/en/discussions/guides/best-practices-for-community-conversations-on-github) for general discusion about this repository. Please report anything in the toolkit or documentation that is unclear, missing, or not working as expected.

When asking a support question, be clear and take the time to explain your problem properly. If your problem is not strictly related to this project, we recommend that you use [Stack Overflow](https://stackoverflow.com/), [r/Embedded](https://www.reddit.com/r/embedded/) or similar question-and-answer website instead. First, check if the **[RTEdbg manual](https://github.com/RTEdbg/RTEdbg/releases/download/Documentation/RTEdbg.library.and.tools.manual.pdf)** already contains an answer to your question or a solution to your problem.

## Feature Requests & Ideas
If a feature that's important to you is missing from the toolkit, or if you have an idea for a new functionality, feel free to open an issue under Issues or start a conversation under Discussions. Your feedback is welcome and helps improve the project!

## About the Primary Author
The author works in real-time control and power electronics design for battery-powered systems. He has been developing his own debugging solutions since learning to program on a Sinclair ZX80. This project is the author's first universal toolkit. Credits go to Stefan Milivojčev, who participated in the development of the RTEmsg format definition parser as part of his master's thesis and helped with the Github workflows.

English is not the native language of the author. Google Translate and DeepL Write were used to improve the documentation. Thanks in advance for your comments and contributions that will help improve the library, tools, and documentation. This is the author's first open source project published on Github and his contribution to the open source community. Please be patient. The author is working on this project in his spare time.

## List of data logging/tracing solutions

See a list of some data logging, tracing, and debugging solutions in [Data_logging_solutions.md](./docs/Data_logging_solutions.md).
