## Real-time binary data logging/tracing toolkit &nbsp; [![Twitter](https://img.shields.io/twitter/url?style=social&url=https%3A%2F%2Fgithub.com%2FRTEdbg%2FRTEdbg)](https://twitter.com/intent/tweet?text=New%20data%20logging%20%2F%20tracing%20toolkit%20released:&url=https%3A%2F%2Fgithub.com%2FRTEdbg%2FRTEdbg)

![LogoRTEdbg–small](https://github.com/RTEdbg/RTEdbg/assets/144953452/e123f541-1d05-44ca-a85e-34a7abeded22)

**The toolkit includes a library of functions for minimally intrusive code instrumentation (data logging/tracing), tools for transferring data to the host, a tool for decoding data on the host, and demo code.
The solution helps embedded programmers test and debug C/C++ firmware, optimize system performance, reverse engineer poorly documented code, and find hard-to-reproduce problems.**

The RTEdbg library and tools are suitable for both large, hard-real-time RTOS-based systems and small, resource-constrained systems. The tools provide better insight into the operation of real-time systems than traditional debuggers because real-time systems cannot be stopped. Restarting the system changes the conditions, leading to different results. In addition, loss of control can cause damage.

**This is the main repository used to distribute the RTEdbg toolkit only.** See a **[List of repositories](#Repository-Structure)** that are part of the RTEdbg toolkit.

View the **[RTEdbg Presentation](https://github.com/RTEdbg/RTEdbg/releases/download/Documentation/RTEdbg.Presentation.pdf)** to learn about the key benefits and to see the basic features.<br>

### **Newsflash** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&Rightarrow; See also: **[News](https://github.com/RTEdbg/RTEdbg/blob/master/docs/NEWS.md)**
* Timestamp [timer driver](https://github.com/RTEdbg/RTElib/tree/master/Portable/Timer/ESP32/rtedbg_timer_esp32.h) for the ESP32 and [instructions](https://github.com/RTEdbg/RTElib/tree/master/Portable/Timer/ESP32/Readme.md) added.

## Table of contents:
* [Introduction](#Introduction)
* [Getting Started](#Getting-Started)
* [RTEdbg Schematic Presentation](#RTEdbg-Schematic-Presentation)
* [Repository Structure](#Repository-Structure)
* [Articles on Code Instrumentation, Logging, and Tracing](#articles-on-code-instrumentation-logging-and-tracing)
* [Demo Projects](#Demo-Projects)
* [Installation Instructions](#Installation-Instructions)
* [How to Contribute or Get Help](#How-to-Contribute-or-Get-Help)
* [About the Author](#About-the-Author)

## Introduction
**The solution was designed with a focus on maximum execution speed, low memory and stack requirements, and portability. It is essentially a reentrant timestamped *fprintf()* function that runs on the host instead of the embedded system.** Flexible filtering and sorting of data into user-defined files helps manage the flood of data from the embedded system.
The new data logging/tracing solution is not a replacement for existing event-oriented solutions such as SystemView or Tracealyzer, as it is based on a different concept. It is optimized for minimally intrusive logging and flexible data decoding. Flexible decoding (printing) of data enables analysis of log files with existing tools (log viewers, graphing and event analysis tools). The code is optimized for 32-bit devices. For example: Only 35 CPU cycles and 4 bytes of stack are required to log a simple event on a device with a Cortex-M4 core. The total program memory footprint is only about 1 kB (using all functions, not including function calls and function parameter preparation). For the [SystemView](https://www.segger.com/downloads/free-utilities/UM08027) nearly 200 CPU cycles and a maximum of 150 to 510 bytes of stack are required for event generation and encoding. Even faster execution and lower stack usage can be achieved while using the inline versions of the data logging functions at the expense of higher program memory usage - for example, the inline versions can be used in the most time critical parts of the code only.

Code instrumentation is minimally intrusive because raw binary data is logged along with an automatically assigned format definition ID (transparent to the programmer) and timestamp. Raw data logging also minimizes circular buffer requirements. **Any data type or entire structures/buffers can be logged and decoded, including bit fields and packed structures.** In addition, there is no need for format strings or data formatting/tagging functions in the embedded system. **Any debug probe, communication channel, or media can be used to transfer data to the host.** Because data is logged in raw binary form, bandwidth requirements are low. The [**RTEgetData**](https://github.com/RTEdbg/RTEgetData) tool for transferring binary log data to a host using the debug probe GDB server is part of the RTEdbg project.

**The toolkit can be used for any type of project**, from small resource-constrained to large RTOS-based, including hard real-time or functional safety. Low stack requirements virtually eliminate the possibility of stack overflows after existing code instrumentation. Data logging functions are reentrant and do not disable interrupts if the microcontroller supports the exclusive access (mutex) instructions.
See the [**RTEdbg Toolkit Suitability Guide**](https://github.com/RTEdbg/RTEdbg/blob/master/docs/Toolkit_Suitability_Guide.md) for examples where the toolkit could be used.

Using this library avoids all the problems of non-reentrant printf-style debugging modes. All format strings are stored only on the host computer, where the decoding of the logged data is performed. Since the data is recorded in binary form, the number of functions is very small and it is possible to learn how to use them quickly. Format definitions are printf-style strings, so they are familiar to programmers.

## Getting Started
Complete documentation can be found in the **[RTEdbg Manual](https://github.com/RTEdbg/RTEdbg/releases/download/Documentation/RTEdbg.library.and.tools.manual.pdf)**. See the **GETTING STARTED GUIDE** section for quick start instructions and **INTRODUCTION AND KEY FEATURES** for a look at the main features and benefits. 
A ZIP file containing the complete documentation and demo projects is available on the **[download page](https://github.com/RTEdbg/RTEdbg/releases)**.

## RTEdbg Schematic Presentation ##
![RTEdbg_presentation](https://github.com/RTEdbg/RTEdbg/assets/144953452/0b84dfc4-6e0f-4aec-aa1c-3fe04686e99e)

The schematic shows how data logging can be integrated into embedded system firmware. Only a C source file with logging functions and some header files need to be added to the project. Any C or C++ code can be instrumented – including drivers, exception handlers, RTOS kernel. The logging functions store raw binary data in the circular buffer (no time-consuming data tagging or encoding). Any interface or media can be used to transfer data to the host for decoding and analysis, since only the contents of a data structure need be transferred. The captured data can be transferred to the host, decoded, and analyzed while the target code is running normally. 

The RTEmsg utility performs offline decoding of binary data. Logged messages are decoded (printed) to files using fprintf-style format definitions (defined in C header files). The data logged with a log function call can hold multiple values, and each of them can be printed to any number of files using different format definitions (different print strings). This allows the decoded information to be sorted so that the important information is collected together and can be quickly reviewed by the tester (rather than having to search through a long main log file). This tool not only decodes and sorts the recorded data, but also generates statistical report for the decoded values and times, allowing testers to quickly find extreme values. If errors are found in the binary file or in the format definition files, they are reported in the Errors.log file.

Real-time systems generate a flood of data, and it is important to sort the data before analyzing it to quickly identify potential problems (data extremes, errors, warnings). Programmers specify how the logged data is printed and into how many output files it is sorted (printed). Important messages or values can be written to more than one output file, each value in a different format. Existing data and event analysis tools can be used because flexible data formatting allows adaptation to the needs of existing data/event analysis or graphing tools.

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
| **[ARM Cortex-M0/M0+ <br> Exception handler demo](https://github.com/RTEdbg/RTEcomLib_NUCLEO_C071RB_Demo/blob/master/Exception_handler_Cortex-M0.md)** | The demo shows a simple way to log the ARM Cortex-M0/M0+ registers and the stack when a system exception or an unhandled interrupt has occurred. |
| **[ARM Cortex-M4/M7 <br> Exception handler demo](https://github.com/RTEdbg/RTEdbgDemo/blob/master/Simple_STM32H743/RTEdbg/Demo_code/Cortex_M4-M7_fault_handler.md)**  | The demo shows how to log the ARM Cortex-M4/M7 CPU core and system registers when a system exception or an unhandled interrupt has occurred. |
| | |

Some of the demo projects are also included in the distribution ZIP file - see **[Releases](https://github.com/RTEdbg/RTEdbg/releases)**. Each demo folder for the STM32 CPU family contains the complete setup for the STM32CubeIDE (main folder), IAR EWARM (EWARM subfolder) and for the Keil MDK (MDK-ARM subfolder) toolchains. Not all compiler/linker settings are the same for different IDEs. The demo projects also include a demonstration of how to transfer data from the embedded system to the host using the RTEgetData utility (see \"TEST RTEgetData\" folders) or using the tools that are part of the Debug Probe software.

## Installation Instructions

The full toolkit can only be downloaded from **[download page](https://github.com/RTEdbg/RTEdbg/releases)**. The toolkit is currently available for Windows only. Extract the ZIP file into the `c:\RTEdbg` folder. This will ensure that the correct paths are used in the demo projects.

## How to Contribute or Get Help
Follow the [Contributing Guidelines](https://github.com/RTEdbg/RTEdbg/blob/master/docs/CONTRIBUTING.md) for bug reports and feature requests regarding the RTEdbg library. Follow the [Discussions Guidelines](https://docs.github.com/en/discussions/guides/best-practices-for-community-conversations-on-github) for general discusion about this repository.

When asking a support question, be clear and take the time to explain your problem properly. If your problem is not strictly related to this project, we recommend that you use [Stack Overflow](https://stackoverflow.com/), [r/Embedded](https://www.reddit.com/r/embedded/) or similar question-and-answer website instead. First, check if the **[RTEdbg manual](https://github.com/RTEdbg/RTEdbg/releases/download/Documentation/RTEdbg.library.and.tools.manual.pdf)** already contains an answer to your question or a solution to your problem.

## About the Author
The author works in real-time control and power electronics design for battery-powered systems. He has been developing his own debugging solutions since learning to program on a Sinclair ZX80. This project is the author's first universal toolkit. Credits go to Stefan Milivojčev, who participated in the development of the RTEmsg format definition parser as part of his master's thesis and helped with the Github workflows.

English is not the native language of the author. Google Translate and DeepL Write were used to improve the documentation. Thanks in advance for your comments and contributions that will help improve the library, tools, and documentation. This is the author's first open source project published on Github and his contribution to the open source community. Please be patient. The author is working on this project in his spare time.

## Other data logging/tracing solutions

See a list of some other data logging, tracing, and debugging solutions in [Other_data_logging_solutions.md](./docs/Other_data_logging_solutions.md).
