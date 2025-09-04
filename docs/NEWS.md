## **RTEdbg project news**

### 2025-09-04
* Updated documentation
* Added [code size optimized ARM Cortex-M4/M7 fault handler](https://github.com/RTEdbg/RTEdbgDemo/blob/master/STM32L433/RTEdbg/Demo_code/Simple_Cortex_M4-M7_fault_handler.md) demo

### 2025-05-23
* Timestamp [timer driver](https://github.com/RTEdbg/RTElib/tree/master/Portable/Timer/ESP32/rtedbg_timer_esp32.h) for the ESP32 and [instructions](https://github.com/RTEdbg/RTElib/tree/master/Portable/Timer/ESP32/Readme.md) added.

### 2025-05-12
RTEdbg library, tools and demo code version 1.01.00A released.
* The new RTEgetData data transfer utility replaces the RTEgdbData and RTEcomData
* Updated `RTEcomLib` and `RTEcomLib_NUCLEO_C071RB_Demo` repositories
* Various smaller documentation updates

### 2025-02-16
RTEdbg library, tools and demo code version 1.01.00 released.
* Added logging of messages on unlined addresses for processors that do not support unlined addressing in hardware.
* Added conditional translation option for an even smaller data logging code footprint.
* Updated RTEgdbData and RTEmsg utilities
* Updated documentation

### 2024-12-09
RTEdbg library, tools and demo code version 1.00.03 released.
* RTEmsg utility source code released
* Improved compile time check
* Added `rtedbg_generic_non_reentrant.h` buffer space reservation driver
* Added `rtedbg_zero_timer.h` timestamp timer driver for projects that do not need timestamps
* Added `rtedbg_inline.h` file with inline versions of data logging functions
* Updated Readme.md files.
* RTEmsg command line argument -ts added

### 2024-11-17
RTEdbg library, tools and demo code version 1.00.02 released.
* Added RTEcomLib repository - functions for log data transfer to host via serial channel.
* Added RTEcomLib_NUCLEO_C071RB_Demo repository - demo code for the RTEcomLib library.
* Added the `rtedbg_generic_atomic_smp.h` buffer space reservation driver
* Fixed a bug in the STM32 TIM2 timestamp driver.
* RTEmsg utility: Fixed a bug in the search for the next long timestamp after detecting a probable message dropout.

### 2024-10-12
RTEdbg library, tools and demo code version 1.00.01 released.
* Improved data logging library - see the [Library History](https://github.com/RTEdbg/RTElib/blob/master/History.md). The toolkit can now be used on almost all microcontrollers with 32-bit cores.
* New utility for transferring data from the embedded system to the host using Debug Probes GDB server - see the [RTEgdbData utility Readme](https://github.com/RTEdbg/RTEgdbData/blob/master/Readme.md).
* Improved and supplemented instructions - see the RTEdbg Manual &rarr; section *Revision History*.
* Improved data decoding utility RTEmsg, etc.

### 2024-05-11
* RTEdbg library, tools and demo code version 1.00.00 released.
