# RTEdbg Toolkit Suitability Guide

Debuggers provide only very slow real-time monitoring of variable values (e.g. live view). Some tools allow sampling of some global variables with a frequency of up to a few kHz, but only asynchronously. In this case we do not know if two values are from the same control cycle or not. Instrumenting the code allows synchronous data logging, but requires some additional effort. 

To analyze and optimize a complex system, it is necessary to log the values processed by the firmware (e.g., control algorithm data, requests received via communication, firmware decisions, sequencer state changes, alarms). It makes a lot of sense to include the logging of exceptional events, because in such a case the dump information of the processor's working and system registers, as well as the history of previously logged events, is very useful. In the demo, there is an example of exception logging for processors of the Cortex-M4 and M7 families.

System architects and programmers need to think about which method makes sense for testing or debugging. The decision depends on the complexity of the project. For smaller projects, code instrumentation makes sense, for example, so that we have test points in the code to enable testing even when the debugger probe is not connected to the embedded system. Function libraries, operating systems, peripheral drivers, etc. can be equipped with logging and used in a larger number of projects. 

The logs included in the project are like test plugs for the hardware. Each group of messages is like a communication channel through which certain types of information are available. We can only connect those that are of interest to us during the current test. The more channels that are active at the same time, the more different data there is, but also the shorter the history. Both the amount of memory for the circular buffer and the bandwidth for data transfer to the host are limited. Flexible on-the-fly message filtering allows the tester to select the most relevant data for the current test.

**The RTEdbg toolkit is well suited for:**
* Projects that require **minimally intrusive instrumentation** - including hard real-time and functional safety projects.
* **Resource-constrained systems** or systems where the **programmer wishes to avoid print-style testing** due to slow execution, memory constraints, and re-entrancy issues.
* Projects where we are primarily interested in the **custom data processed by the firmware** (e.g., control system optimization). Any type of data can be logged and decoded, from control algorithms to exception handlers.
* Projects where the **code execution should not be stopped** - e.g. real-time control or communication systems where our system must respond within a timeout window (most communication protocols have timeouts).
* Projects where we want to **export the results directly to a file for automated processing** (e.g. CSV file for automated regression testing).
* Optimization and analysis of complex embedded systems where it is difficult to find the root cause of performance degradation.
* Projects where we want to **keep logging in production code** so that problem analysis is possible even after the systems are in the field.
* **IoT projects** where we want to enable debugging even after delivery to customers (e.g. execution history, post-mortem debugging). Only binary data is logged and the logging memory consumption is low, so the data consumption for paid data transfer is low.
* Project for which **flexible post-mortem analysis** is required. Custom triggers can be used to stop logging. Logging can be frozen after an event that causes the system to shut down. System can be restarted and continue operation, and service technician or designer can connect to system upon arrival at site or remote connection to retrieve logged data (embedded system operates normally, only logging is frozen). Logged content can be preserved while code continues to run normally.

<br>

**Low stack requirements virtually eliminate the possibility of stack overflows when instrumenting code.** This makes the toolkit ideal for:
* **Analyzing complex, difficult-to-reproduce problems** in existing projects.
* **Reverse engineering poorly documented code.**
* **Analyzing third-party code** - for example, analyzing software stacks (black boxes) in application code. Logged data helps us understand the behavior of the tested code, the overall system, and influences from the environment or other systems.

<br>

The following list contains **cases where the RTEdbg toolkit is currently less suitable** because RTOS support and support for commercial event analysis tools are not yet implemented:
* For projects where we want to use event analysis tools like Tracealyzer or SystemView. 
* Projects where we want to include ready-made RTOS trace hooks. RTEdbg is a new tool and so far support and demo for popular RTOS and for event analysis tools like Eclipse Compass is not yet done.

Solutions such as the Tracealyzer or SystemView toolkits have their own powerful event analysis tools. Their data logging functions are slower and consume more stack space. The slower code execution is usually not a problem for most projects. However, additional stack space is required for each RTOS task from which the data logging functions are called, as well as for interrupt handlers and drivers. These tools are optimized for event analysis, but are not flexible for logging and analyzing application-specific data.

<br>

**Note:** The project never envisioned the development of tools for viewing and analyzing data and events. Flexible data formatting allows viewing and analysis with most existing tools that support ASCII or UTF-8 based file formats. Generation of binary output files is not supported. The exception is writing binary data from messages directly to a binary output file.