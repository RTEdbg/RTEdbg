# RTEdbg project to-do list

See if you can contribute to any of the activities listed or suggest new ones.

## General to-do list
- Documentation clarification and improvement.
- Short version of the RTEdbg manual explaining the basic features.
- Process automation and continuous integration.
- Porting the RTEdbg data acquisition library to other CPU cores and toolchains (writing drivers, tests, demo code, etc.).
- Testing of full functionality including RTEmsg PC utility for data decoding, sorting and statistics.
- Demo project improvements and additional demo projects.
- Development of tools for streaming mode data transfer to the host (see list below).
- Enhancements to the RTEmsg binary data decoding application (see list below).
- Integration with other tools to support testing and debugging.
- Instructions on how to use trace and log data tools to analyze or verify data obtained from data capture and decoding.
- Trace support for popular RTOS and demo code (FreeRTOS, ThreadX, Zephyr, etc.).
- Instructions and examples for preparing data for trace analysis - e.g. for [Eclipse Trace Compass](https://projects.eclipse.org/projects/tools.tracecompass) using the [BTF](https://wiki.eclipse.org/images/e/e6/TA_BTF_Specification_2.1.3_Eclipse_Auto_IWG.pdf) or similar trace format.

### Tools for transferring data from the embedded system to the host
Existing solutions only allow the transfer of logged binary data from the embedded system via a debugger probe. Only snapshot or single-shot data logging is available, not streaming data logging.

Streaming (continuous) data acquisition requires solutions for transferring (or recording) data via, for example:
- Serial port (e.g. VCP over USB connection)
- GDB protocol (TCP/IP)
- General TCP/IP
- Debug probes such as J-LINK, ST-LINK, etc.
- SD card
- USB flash drive
- CAN bus

### RTEmsg binary data decoding application
- Streaming mode decoding support
- Support for binary files containing multiple singleshot and snapshot data
- Translation of Messages.txt file to other languages

### **In progress**
- Instructions for integrating event logging into an RTOS with examples for FreeRTOS.
- Application for transferring data from an embedded system using the GDB protocol.
- Translate the source code of the RTEmsg binary decoding application into English and publish it on Github.
