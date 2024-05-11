## **RTEdbg Toolkit** - Real-time binary data logging/tracing library and tools [![Twitter](https://img.shields.io/twitter/url?style=social&url=https%3A%2F%2Fgithub.com%2FRTEdbg%2FRTEdbg)](https://twitter.com/intent/tweet?text=Wow:&url=https%3A%2F%2Fgithub.com%2FRTEdbg%2FRTEdbg)

![LogoRTEdbg–small](https://github.com/RTEdbg/RTEdbg/assets/144953452/e123f541-1d05-44ca-a85e-34a7abeded22)

#### NewsFlash: RTEdbg toolkit released &nbsp; &nbsp; &nbsp; &Rightarrow; View news in the **[Newsroom](Docs/NEWS.md)**

### Table of contents:
* [Introduction](#Introduction)
* [Getting Started](#Getting-Started)
* [RTEdbg schematic presentation](#RTEdbg-schematic-presentation)
* [Repository Structure](#Repository-Structure)
* [Demo Projects](#Demo-Projects)
* [Installation Instructions](#Installation-Instructions)
* [How to contribute or get help](#How-to-contribute-or-get-help)
* [About the Author](#About-the-Author)

## Introduction

**The toolkit includes a library of functions for minimally invasive code instrumentation (data logging/tracing), tools for decoding data on the host and demo code.** The toolkit helps embedded programmers test and debug C/C++ firmware, optimize system performance, reverse engineer poorly documented code, and find hard-to-reproduce problems. The RTEdbg library and tools are suitable for both large RTOS-based systems and small, resource-constrained systems. The tools provide better insight into the operation of real-time systems than traditional debuggers because real-time systems cannot be stopped. Restarting the system changes the conditions, leading to different results. In addition, loss of control can cause damage.

The solution was designed with a focus on maximum execution speed, low memory and stack requirements, and portability. It is essentially a timestamped fprintf() function that runs on the host instead of the embedded system. Flexible filtering and sorting of data into user-defined files helps manage the flood of data from the embedded system.
The new data logging/tracing solution is not a replacement for existing event-oriented solutions such as SystemView or Tracealyzer, as it is based on a different concept. It is optimized for minimally invasive logging and flexible data decoding. The code is optimized for 32-bit devices. For example: Only 35 CPU cycles and 4 bytes of stack are required to log a simple event on a device with a Cortex-M4 core. See the key benefits and features of the new toolkit in **[RTEdbg Presentation]([https://github.com/RTEdbg/RTEdbg/releases/download/Documentation/RTEdbg.Presentation.pdf)**.

Code instrumentation is minimally invasive because raw binary data is logged along with an automatically assigned format definition ID (transparent to the programmer). Any data type or entire structures/buffers can be logged, including bit fields and packed structures. The stack requirements for this solution are minimal, virtually eliminating the possibility of stack overflows after code instrumentation. In addition, there is no need for format strings or data formatting/tagging functions in the embedded system. Any debug probe, communication channel, or media can be used to transfer data to the host. Because data is logged in raw binary form, bandwidth requirements are low. The small number of functions makes the solution easy to use and learn.

The toolkit can be used for any type of project, including hard real-time or functional safety. Low stack requirements virtually eliminate the possibility of stack overflows after code instrumentation. Data logging functions are reentrant and do not disable interrupts if the microcontroller supports the mutex instructions. The code is optimized for 32-bit devices and can be integrated into C and C++ code.
See the [**RTEdbg Toolkit Suitability Guide**](Docs/Toolkit_Suitability_Guide.md) for examples where the toolkit could be used.

Using this library avoids all the problems of printf-style debugging modes. All format strings are stored only on the host computer, where the decoding of the logged data is performed. Since the data is recorded in binary form, the number of functions is very small and it is possible to learn how to use them quickly. Format definitions are printf-style strings, so they are familiar to programmers.

## Getting Started
Complete documentation can be found in the **[RTEdbg Manual](https://github.com/RTEdbg/RTEdbg/releases/download/Documentation/RTEdbg.library.and.tools.manual.pdf)**. See the **GETTING STARTED GUIDE** section for quick start instructions and **INTRODUCTION AND KEY FEATURES** for a look at the main features and benefits. 
A ZIP file containing the complete documentation and demo projects is available for download - go to the **[download page](https://github.com/RTEdbg/RTEdbg/releases)**.

## RTEdbg schematic presentation ##
![RTEdbg_presentation_medium](https://github.com/RTEdbg/RTEdbg/assets/144953452/84934611-ef20-4578-8ed6-3469d186c5d5)

The schematic shows how data logging can be integrated into embedded system firmware. Only a C source file with logging functions and some header files need to be added to the project. Any C or C++ code can be instrumented – including drivers, exception handlers, RTOS kernel. The logging functions store raw binary data in the circular buffer (no time-consuming data tagging or encoding). Any interface or media can be used to transfer data to the host for decoding and analysis, since only the contents of a data structure need be transferred. The RTEmsg utility performs offline decoding of binary data. Logged messages are decoded (printed) to files using fprintf-style format definitions (defined in C header files). If errors are found in the binary file or in the format definition files, they are reported in the Errors.log file.

Real-time systems generate a flood of data, and it is important to sort the data before analyzing it to quickly identify potential problems (data extremes, errors, warnings). Programmers specify how the logged data is printed and into how many output files it is sorted (printed). Important messages or values can be written to more than one output file, each value in a different format. Existing data and event analysis tools can be used because flexible data formatting allows adaptation to the needs of existing data/event analysis or graphing tools.

## Repository Structure
This repository contains the RTEdbg library only. The RTEmsg data decoding application and other tools will be released later after their documentation is completed. Some folders contain additional readme files with more detailed descriptions of the files in those folders. Links to other repositories will be added to this README file as new repositories are released.

The author puts great emphasis on robustness and low complexity of the RTEdbg library and RTEmsg data decoding application code. Please report bugs or suggest improvements / corrections. Most of the toolkit code is contained in host tools such as the RTEmsg (binary data decoding tool) application that runs on the host computer.

The following repositories belong to the RTEdbg project:
* [Binary data logging/tracing library](https://github.com/RTEdbg/RTElib)
* [Binary data decoding application](https://github.com/RTEdbg/RTEmsg)

## Installation Instructions

The full toolkit can only be downloaded from this repository - see **[download page](https://github.com/RTEdbg/RTEdbg/releases)**. The toolkit is currently available for Windows only. Extract the ZIP file into the "c:\RTEdbg folder". This will ensure that the correct paths are used in the demo projects.

## Demo Projects
Demo projects are currently only provided in the project distribution ZIP file. Complete settings are provided for the STM32CubeIDE, MCUXpresso, Keil MDK and IAR EWARM IDEs. Follow the instructions in the *RTEdbg Manual* &Rightarrow; *Demo Projects*.

## How to contribute or get help
Follow the [Contributing Guidelines](Docs/CONTRIBUTING.md) for bug reports and feature requests regarding the RTEdbg library. Source code for other parts of the toolkit will be released later, after their documentation has been translated to English. 
Please use **[RTEdbg.freeforums.net](https://rtedbg.freeforums.net/)** for general discussions about the RTEmsg application and the RTEdbg toolkit.

When asking a support question, be clear and take the time to explain your problem properly. If your problem is not strictly related to this project, we recommend that you use [Stack Overflow](https://stackoverflow.com/) or similar forums instead. First, check if the **[RTEdbg manual](https://github.com/RTEdbg/RTEdbg/releases/download/Documentation/RTEdbg.library.and.tools.manual.pdf)** already contains an answer to your question or a solution to your problem.

## About the Author
The author works in real-time control and power electronics design for battery-powered systems. He has been developing his own debugging solutions since learning to program on a Sinclair ZX80. This project is the author's first universal debugging tool. Credits go to Stefan Milivojčev, who participated in the development of the RTEmsg format definition parser as part of his master's thesis.

English is not the author's native language. Google Translate and DeepL Write were used to improve the documentation. Thanks in advance for your comments and contributions that will help improve both the tools and the documentation. This is the author's first open source project published on Github and his contribution to the open source community. Please be patient. The author is working on this project in his spare time.
