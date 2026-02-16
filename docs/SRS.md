# Software Requirements Specification (SRS)
# Virtual Speedometer - AUTOSAR Application Layer Simulation

**Document Version:** 1.0
**Date:** 2026-02-16
**Status:** Approved
**Authors:** Development Team
**Classification:** Educational/Portfolio Project

---

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | 2026-02-16 | Development Team | Initial release |

---

## Table of Contents

1. [Introduction](#1-introduction)
   - 1.1 [Purpose](#11-purpose)
   - 1.2 [Scope](#12-scope)
   - 1.3 [Definitions, Acronyms, and Abbreviations](#13-definitions-acronyms-and-abbreviations)
   - 1.4 [References](#14-references)
   - 1.5 [Overview](#15-overview)

2. [Overall Description](#2-overall-description)
   - 2.1 [Product Perspective](#21-product-perspective)
   - 2.2 [Product Functions](#22-product-functions)
   - 2.3 [User Characteristics](#23-user-characteristics)
   - 2.4 [Constraints](#24-constraints)
   - 2.5 [Assumptions and Dependencies](#25-assumptions-and-dependencies)

3. [Specific Requirements](#3-specific-requirements)
   - 3.1 [Functional Requirements](#31-functional-requirements)
   - 3.2 [Architecture Requirements](#32-architecture-requirements)
   - 3.3 [Interface Requirements](#33-interface-requirements)
   - 3.4 [User Interface Requirements](#34-user-interface-requirements)
   - 3.5 [Performance Requirements](#35-performance-requirements)
   - 3.6 [Testing Requirements](#36-testing-requirements)
   - 3.7 [Build System Requirements](#37-build-system-requirements)
   - 3.8 [Configuration Requirements](#38-configuration-requirements)

4. [Appendices](#4-appendices)
   - 4.1 [Traceability Matrix](#41-traceability-matrix)

---

## 1. Introduction

### 1.1 Purpose

This Software Requirements Specification (SRS) defines the functional and non-functional requirements for the Virtual Speedometer project, an educational simulation of an AUTOSAR (AUTomotive Open System ARchitecture) application layer. The document follows IEEE 830-1998 and ISO/IEC/IEEE 29148:2018 standards for requirements specification.

This document is intended for:
- Software developers implementing the system
- Quality assurance engineers validating the implementation
- Automotive engineers evaluating AUTOSAR concepts
- Portfolio reviewers and technical interviewers
- Future contributors to the project

### 1.2 Scope

The Virtual Speedometer is a demonstration software system that simulates a vehicle dashboard with the following key characteristics:

**In Scope:**
- Simulation of AUTOSAR application layer architecture
- Implementation of six Software Components (SW-Cs) with defined responsibilities
- Runtime Environment (RTE) for inter-component communication
- Basic Software (BSW) layer including task scheduler and COM layer
- Dual user interface modes: terminal-based (ncurses) and graphical (Qt)
- Unit testing framework with mocked RTE interfaces
- XML/JSON-based configuration system
- Diagnostic Trouble Code (DTC) management

**Out of Scope:**
- Production-ready automotive software certification (ISO 26262, ASPICE)
- Real hardware integration (actual CAN bus, ECU deployment)
- Complete AUTOSAR Basic Software (e.g., real MCAL, CAN stack)
- Safety-critical features requiring formal verification
- Real-time operating system (RTOS) integration
- Persistent storage implementation (NvM simulation only planned)

**Product Benefits:**
- Educational demonstration of AUTOSAR architectural patterns
- Portfolio showcase of C/C++ development capabilities
- Testing ground for embedded systems concepts
- Interview preparation for automotive software positions

### 1.3 Definitions, Acronyms, and Abbreviations

#### AUTOSAR-Specific Terms

| Term | Definition |
|------|------------|
| ARXML | AUTOSAR XML - Configuration file format used in AUTOSAR systems |
| BSW | Basic Software - Low-level software layer providing services to application layer |
| COM | Communication - BSW module handling signal packing/unpacking and network communication |
| C/S | Client-Server - Synchronous, service-oriented communication pattern |
| DTC | Diagnostic Trouble Code - Standardized fault code for vehicle diagnostics |
| MCAL | Microcontroller Abstraction Layer - Hardware abstraction layer in BSW |
| NvM | Non-volatile Memory Manager - BSW module for persistent data storage |
| RTE | Runtime Environment - Middleware layer routing signals between SW-Cs |
| Runnable | Executable function within a SW-C, triggered by AUTOSAR OS |
| S/R | Sender-Receiver - Asynchronous, data-oriented communication pattern |
| SW-C | Software Component - Independent application module with defined interfaces |

#### General Terms

| Term | Definition |
|------|------------|
| API | Application Programming Interface |
| CAN | Controller Area Network - Automotive communication protocol |
| DEM | Diagnostic Event Manager |
| ECU | Electronic Control Unit |
| GUI | Graphical User Interface |
| MISRA-C | Motor Industry Software Reliability Association C coding standard |
| OS | Operating System |
| UI | User Interface |
| XML | Extensible Markup Language |
| JSON | JavaScript Object Notation |

#### Project-Specific Terms

| Term | Definition |
|------|------------|
| Periodic Runnable | Runnable executed at fixed time intervals |
| On-Data-Reception Runnable | Runnable triggered when new data arrives on required port |
| Provided Port | Output interface of a SW-C offering data or services |
| Required Port | Input interface of a SW-C consuming data or services |
| Scheduler Tick | Minimum time resolution of task scheduler (10ms) |

### 1.4 References

1. **AUTOSAR Release 4.4.0 Specifications**
   - AUTOSAR_SWS_RTE - Specification of RTE
   - AUTOSAR_TPS_SoftwareComponentTemplate - Software Component Template

2. **Standards**
   - IEEE Std 830-1998 - IEEE Recommended Practice for Software Requirements Specifications
   - ISO/IEC/IEEE 29148:2018 - Systems and software engineering - Life cycle processes - Requirements engineering
   - MISRA C:2012 - Guidelines for the use of the C language in critical systems

3. **Development Tools**
   - CMake 3.12+ Documentation
   - Google Test Framework Documentation
   - Qt 5.12+ Framework Documentation
   - ncurses Library Documentation

4. **Project Documentation**
   - README.md - Project overview and getting started guide
   - ARCHITECTURE.md - Detailed system architecture
   - AUTOSAR_CONCEPTS.md - AUTOSAR concepts explanation
   - API_REFERENCE.md - Component API reference

### 1.5 Overview

This SRS is organized as follows:

**Section 2 - Overall Description:** Provides context for the system including product perspective, high-level functions, user characteristics, constraints, and assumptions.

**Section 3 - Specific Requirements:** Contains detailed functional and non-functional requirements organized by category with unique requirement identifiers, priorities, and verification methods.

**Section 4 - Appendices:** Includes traceability matrix mapping requirements to implementation components.

---

## 2. Overall Description

### 2.1 Product Perspective

The Virtual Speedometer is an educational simulation project designed to demonstrate AUTOSAR application layer architecture and modern C/C++ development practices. The system exists as a standalone desktop application running on Linux platforms.

#### System Context

The Virtual Speedometer operates in the following context:

```
┌─────────────────────────────────────────────────────────────┐
│                     Host Operating System                    │
│                          (Linux)                             │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │         Virtual Speedometer Application                │  │
│  │                                                         │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │        Application Layer (SW-Cs) - C++          │  │  │
│  │  │  [WheelSpeed] [VehicleMode] [SpeedDisplay]      │  │  │
│  │  │  [SpeedLimiter] [WarningLight] [DiagManager]    │  │  │
│  │  └─────────────────┬───────────────────────────────┘  │  │
│  │                    │                                    │  │
│  │  ┌─────────────────▼───────────────────────────────┐  │  │
│  │  │        Runtime Environment (RTE) - C            │  │  │
│  │  │    [S/R Routing] [C/S Calls] [Trigger Logic]    │  │  │
│  │  └─────────────────┬───────────────────────────────┘  │  │
│  │                    │                                    │  │
│  │  ┌─────────────────▼───────────────────────────────┐  │  │
│  │  │        Basic Software (BSW) - C                 │  │  │
│  │  │    [Scheduler] [COM] [Config Loader]            │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  │                                                         │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │         UI Layer - C++/Qt or ncurses            │  │  │
│  │  │      [Terminal UI] or [Qt GUI Dashboard]        │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

#### Relationship to AUTOSAR

This project is a **simulation** of AUTOSAR Classic Platform application layer concepts:

- **Simulated:** SW-C architecture, RTE communication, runnable execution model, BSW abstractions
- **Simplified:** No MCAL, no real ECU hardware, no actual CAN frames, no production OS
- **Educational Focus:** Demonstrates architectural patterns, not complete AUTOSAR implementation

#### System Interfaces

The system interfaces with:
1. **User:** Via terminal keyboard controls or Qt GUI interactions
2. **Configuration Files:** XML system description and JSON RTE routing configuration
3. **File System:** For reading configuration, potential DTC persistence (future)
4. **Operating System:** For scheduling, terminal/GUI rendering, file I/O

### 2.2 Product Functions

The Virtual Speedometer provides the following high-level functions:

1. **Vehicle Speed Simulation**
   - Simulate wheel rotation sensor data
   - Calculate and display vehicle speed in km/h
   - User-controllable speed via keyboard input

2. **Vehicle Mode Management**
   - Support three vehicle modes: Park, Drive, Reverse
   - User-selectable mode transitions
   - Mode-dependent system behavior

3. **Speed Limiting**
   - Enforce maximum speed limits based on vehicle mode
   - Provide speed limit information to other components
   - Configurable limit values per mode

4. **Warning System**
   - Monitor vehicle speed against configured limits
   - Activate visual warning when speed exceeds threshold
   - Deactivate warning when speed returns to safe range

5. **Diagnostic Management**
   - Capture and store Diagnostic Trouble Codes (DTCs)
   - Retrieve stored DTCs for display
   - User-triggerable fault injection for testing

6. **User Interface**
   - Real-time display of speed, mode, limits, warnings, and DTCs
   - Terminal mode with text-based dashboard
   - Qt GUI mode with graphical speedometer and controls

7. **System Configuration**
   - Load SW-C composition from XML configuration
   - Load RTE routing from JSON configuration
   - Support runtime configuration loading

8. **Testing Support**
   - Unit testing with mocked RTE interfaces
   - Component isolation for independent testing
   - Automated test execution via CMake/CTest

### 2.3 User Characteristics

#### Primary Users

1. **Software Developers**
   - **Expertise Level:** Intermediate to advanced C/C++ developers
   - **Usage:** Study code structure, understand AUTOSAR patterns, contribute enhancements
   - **Expectations:** Well-commented code, clear architecture, modular design

2. **Automotive Engineers**
   - **Expertise Level:** Familiar with AUTOSAR concepts and automotive software
   - **Usage:** Evaluate AUTOSAR implementation approach, assess architectural decisions
   - **Expectations:** Correct use of AUTOSAR terminology, realistic SW-C interactions

3. **Technical Interviewers / Portfolio Reviewers**
   - **Expertise Level:** Varies; may be automotive specialists or general software engineers
   - **Usage:** Assess candidate's C/C++ skills, understanding of embedded concepts, code quality
   - **Expectations:** Professional code quality, comprehensive documentation, working demo

4. **Students / Learners**
   - **Expertise Level:** Beginner to intermediate, learning embedded systems or AUTOSAR
   - **Usage:** Study example implementation, experiment with modifications
   - **Expectations:** Educational documentation, clear examples, runnable demos

#### User Skills and Knowledge

Users are expected to have:
- Basic command-line proficiency (Linux terminal)
- Understanding of C and C++ programming languages
- Familiarity with CMake build systems
- Optional: Basic knowledge of AUTOSAR concepts (documentation provided for learners)

### 2.4 Constraints

#### Technology Constraints

1. **Platform:**
   - Primary target: Linux operating systems (Ubuntu, Fedora, Arch)
   - No support for Windows or macOS in initial release
   - Requires X11 or Wayland display server for Qt GUI mode

2. **Programming Languages:**
   - BSW and RTE layers must be implemented in C (MISRA-C guidelines)
   - SW-Cs and UI layers implemented in C++17
   - No use of other languages (Python, Rust, etc.)

3. **Dependencies:**
   - CMake 3.12 or higher required
   - GCC 7+ or Clang 6+ compiler required
   - ncurses library required for terminal mode
   - Qt 5.12+ required for GUI mode (optional build)
   - Google Test for testing (optional, auto-downloaded)

#### Design Constraints

1. **Architecture:**
   - Must follow AUTOSAR layered architecture (Application, RTE, BSW, UI)
   - SW-Cs must communicate only through RTE (no direct calls)
   - RTE must be implemented in C, not C++

2. **Memory Management:**
   - No dynamic memory allocation in RTE and BSW layers (MISRA-C compliance)
   - Static allocation for signal buffers and data structures
   - C++ SW-Cs may use dynamic allocation where appropriate

3. **Timing:**
   - Simulation only - not real-time guaranteed
   - Scheduler tick resolution: 10ms minimum
   - No hard real-time constraints

#### Regulatory Constraints

1. **Not production software:**
   - Not ISO 26262 compliant
   - Not ASPICE compliant
   - Not certified for use in vehicles

2. **Licensing:**
   - Open-source under MIT License
   - All dependencies must have compatible licenses

### 2.5 Assumptions and Dependencies

#### Assumptions

1. **Execution Environment:**
   - System runs on Linux with kernel 4.x or higher
   - Sufficient CPU resources for 10ms scheduler resolution
   - Display available for terminal or GUI output

2. **User Expertise:**
   - Users can build C/C++ projects using CMake
   - Users have basic understanding of terminal operations
   - Users can install required dependencies via package manager

3. **Configuration:**
   - Configuration files (XML/JSON) are well-formed and valid
   - Configuration files are present in expected locations
   - Default configurations are sufficient for demonstration purposes

4. **Simulation Accuracy:**
   - Simulation behavior is sufficient for educational purposes
   - Exact real-time behavior is not required
   - Simplified models acceptable (e.g., instant acceleration, no physics)

#### Dependencies

1. **External Libraries:**
   - ncurses library for terminal UI rendering
   - Qt framework for graphical UI (optional)
   - Google Test framework for unit testing (optional)
   - Standard C and C++ libraries

2. **Build Tools:**
   - CMake build system
   - GNU Make or Ninja build tool
   - GCC or Clang compiler toolchain

3. **Runtime Dependencies:**
   - POSIX-compliant operating system
   - pthread library for potential threading (future)
   - File system access for configuration files

4. **Development Dependencies:**
   - Git version control system
   - Text editor or IDE supporting C/C++
   - Static analysis tools (optional: cppcheck, clang-tidy)

---

## 3. Specific Requirements

### 3.1 Functional Requirements

#### 3.1.1 WheelSpeedSensor Component

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-FUNC-001 | The WheelSpeedSensor SW-C shall simulate wheel rotation sensor data and provide calculated vehicle speed. | Must | Test |
| SRS-FUNC-002 | The WheelSpeedSensor shall execute its runnable periodically at 10ms intervals. | Must | Test, Inspection |
| SRS-FUNC-003 | The WheelSpeedSensor shall provide speed data via a Sender-Receiver provided port. | Must | Test |
| SRS-FUNC-004 | The WheelSpeedSensor shall calculate speed in kilometers per hour (km/h) based on simulated wheel rotation rate. | Must | Test |
| SRS-FUNC-005 | The WheelSpeedSensor shall accept user input to increase or decrease simulated wheel speed. | Must | Test |
| SRS-FUNC-006 | The WheelSpeedSensor shall output speed values in the range 0-200 km/h. | Should | Test |
| SRS-FUNC-007 | The WheelSpeedSensor shall initialize speed to 0 km/h at system startup. | Must | Test |

#### 3.1.2 VehicleModeManager Component

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-FUNC-010 | The VehicleModeManager SW-C shall manage vehicle operating modes: Park, Drive, and Reverse. | Must | Test |
| SRS-FUNC-011 | The VehicleModeManager shall execute its runnable periodically at 10ms intervals. | Must | Test, Inspection |
| SRS-FUNC-012 | The VehicleModeManager shall provide current mode via a Sender-Receiver provided port. | Must | Test |
| SRS-FUNC-013 | The VehicleModeManager shall accept user commands to transition between modes. | Must | Test |
| SRS-FUNC-014 | The VehicleModeManager shall initialize to Park mode at system startup. | Must | Test |
| SRS-FUNC-015 | The VehicleModeManager shall support mode transitions: Park to Drive, Park to Reverse, Drive to Park, Reverse to Park. | Must | Test |
| SRS-FUNC-016 | The VehicleModeManager shall not allow direct transitions between Drive and Reverse without entering Park. | Should | Test |

#### 3.1.3 SpeedDisplay Component

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-FUNC-020 | The SpeedDisplay SW-C shall receive vehicle speed data from WheelSpeedSensor via required port. | Must | Test |
| SRS-FUNC-021 | The SpeedDisplay runnable shall be triggered on data reception (speed data arrival). | Must | Test, Inspection |
| SRS-FUNC-022 | The SpeedDisplay shall format speed data for presentation to the UI layer. | Must | Test |
| SRS-FUNC-023 | The SpeedDisplay shall provide formatted speed data via a provided port accessible to UI. | Must | Test |
| SRS-FUNC-024 | The SpeedDisplay shall display speed with integer precision (no decimal places). | Should | Test, Inspection |
| SRS-FUNC-025 | The SpeedDisplay shall update displayed speed within one scheduler tick of receiving new data. | Should | Test |

#### 3.1.4 SpeedLimiter Component

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-FUNC-030 | The SpeedLimiter SW-C shall receive vehicle speed and mode data via required ports. | Must | Test |
| SRS-FUNC-031 | The SpeedLimiter runnable shall be triggered on data reception (speed or mode data arrival). | Must | Test, Inspection |
| SRS-FUNC-032 | The SpeedLimiter shall apply mode-specific speed limits: Park = 0 km/h, Reverse = 10 km/h, Drive = 120 km/h (default). | Must | Test |
| SRS-FUNC-033 | The SpeedLimiter shall provide current speed limit via a Sender-Receiver provided port. | Must | Test |
| SRS-FUNC-034 | The SpeedLimiter shall provide a Client-Server interface for querying the current speed limit. | Must | Test |
| SRS-FUNC-035 | The SpeedLimiter shall allow configuration of speed limit values per mode via configuration files. | Should | Inspection |
| SRS-FUNC-036 | The SpeedLimiter shall respond to Client-Server requests within the same scheduler tick. | Should | Test |
| SRS-FUNC-037 | The SpeedLimiter Client-Server interface shall return the limit value and an error status code. | Must | Test |

#### 3.1.5 WarningLight Component

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-FUNC-040 | The WarningLight SW-C shall receive vehicle speed and speed limit data via required ports. | Must | Test |
| SRS-FUNC-041 | The WarningLight runnable shall be triggered on data reception (speed or limit data arrival). | Must | Test, Inspection |
| SRS-FUNC-042 | The WarningLight shall activate warning status when vehicle speed exceeds the configured speed limit. | Must | Test |
| SRS-FUNC-043 | The WarningLight shall deactivate warning status when vehicle speed is at or below the configured speed limit. | Must | Test |
| SRS-FUNC-044 | The WarningLight shall provide warning status (ON/OFF) via a Sender-Receiver provided port. | Must | Test |
| SRS-FUNC-045 | The WarningLight shall apply a threshold tolerance of +5 km/h before activating warning (e.g., 125 km/h for 120 km/h limit). | Should | Test |
| SRS-FUNC-046 | The WarningLight shall send a fault signal to DiagManager when warning is activated. | Should | Test |

#### 3.1.6 DiagManager Component

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-FUNC-050 | The DiagManager SW-C shall store Diagnostic Trouble Codes (DTCs) received from other SW-Cs. | Must | Test |
| SRS-FUNC-051 | The DiagManager runnable shall be triggered on data reception (fault signal arrival). | Must | Test, Inspection |
| SRS-FUNC-052 | The DiagManager shall support a maximum DTC storage capacity of 50 DTCs. | Must | Test |
| SRS-FUNC-053 | The DiagManager shall assign unique DTC codes to each fault event. | Must | Test |
| SRS-FUNC-054 | The DiagManager shall provide a Client-Server interface for retrieving stored DTCs. | Must | Test |
| SRS-FUNC-055 | The DiagManager shall provide DTC count via a Sender-Receiver provided port. | Must | Test |
| SRS-FUNC-056 | The DiagManager shall accept user-triggered fault injection for testing purposes. | Should | Test |
| SRS-FUNC-057 | The DiagManager shall implement a DTC aging mechanism (oldest DTCs replaced when storage full). | Should | Test |
| SRS-FUNC-058 | Each DTC entry shall include: DTC code, timestamp, fault source component, and status. | Should | Inspection, Test |

### 3.2 Architecture Requirements

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-ARCH-001 | The system shall implement a four-layer architecture: Application (SW-Cs), RTE, BSW, and UI. | Must | Inspection |
| SRS-ARCH-002 | All SW-Cs shall be independent modules with no direct function calls between them. | Must | Inspection, Test |
| SRS-ARCH-003 | SW-Cs shall communicate exclusively through the RTE using defined port interfaces. | Must | Inspection, Test |
| SRS-ARCH-004 | The RTE layer shall be implemented in C language. | Must | Inspection |
| SRS-ARCH-005 | The BSW layer (RTE, Scheduler, COM) shall be implemented in C language. | Must | Inspection |
| SRS-ARCH-006 | SW-Cs shall be implemented in C++ language. | Must | Inspection |
| SRS-ARCH-007 | The BSW layer shall follow MISRA-C:2012 guidelines where practical. | Should | Inspection, Analysis |
| SRS-ARCH-008 | The RTE shall follow MISRA-C:2012 guidelines where practical. | Should | Inspection, Analysis |
| SRS-ARCH-009 | Each SW-C shall be replaceable without requiring modifications to other SW-Cs. | Must | Test, Inspection |
| SRS-ARCH-010 | The UI layer shall be decoupled from SW-C business logic. | Must | Inspection |
| SRS-ARCH-011 | UI components shall only read data from RTE; they shall not invoke SW-C runnables directly. | Must | Inspection |
| SRS-ARCH-012 | The system shall support multiple UI implementations (terminal, Qt GUI) without changing SW-C code. | Must | Test |
| SRS-ARCH-013 | No dynamic memory allocation shall be used in the RTE layer. | Must | Inspection, Analysis |
| SRS-ARCH-014 | No dynamic memory allocation shall be used in the BSW layer. | Must | Inspection, Analysis |
| SRS-ARCH-015 | Signal buffers in RTE shall be statically allocated with fixed sizes. | Must | Inspection |

### 3.3 Interface Requirements

#### 3.3.1 Sender-Receiver Interfaces

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-IF-001 | The system shall support Sender-Receiver (S/R) communication pattern for asynchronous data exchange. | Must | Test |
| SRS-IF-002 | S/R interfaces shall be data-element based (typed data values). | Must | Inspection |
| SRS-IF-003 | S/R communication shall be asynchronous; sender does not block waiting for receiver. | Must | Test |
| SRS-IF-004 | The RTE shall route S/R signals from provided ports to connected required ports. | Must | Test |
| SRS-IF-005 | S/R data elements shall support the following types: uint8, uint16, uint32, int32, float. | Must | Inspection |
| SRS-IF-006 | The RTE shall maintain separate signal buffers for each S/R connection. | Must | Inspection |
| SRS-IF-007 | S/R write operations (Rte_Write) shall overwrite previous values in the signal buffer. | Must | Test |
| SRS-IF-008 | S/R read operations (Rte_Read) shall retrieve the most recent value from the signal buffer. | Must | Test |
| SRS-IF-009 | S/R interfaces shall support 1-to-N communication (one sender, multiple receivers). | Should | Test |

#### 3.3.2 Client-Server Interfaces

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-IF-010 | The system shall support Client-Server (C/S) communication pattern for synchronous service requests. | Must | Test |
| SRS-IF-011 | C/S interfaces shall be operation-based (remote procedure call style). | Must | Inspection |
| SRS-IF-012 | C/S communication shall be synchronous; client blocks until server responds. | Must | Test |
| SRS-IF-013 | The RTE shall route C/S calls from required ports to provided port server operations. | Must | Test |
| SRS-IF-014 | C/S operations shall support input parameters (IN), output parameters (OUT), and return values. | Must | Test |
| SRS-IF-015 | C/S operations shall return a status code indicating success or error. | Must | Test |
| SRS-IF-016 | The RTE shall execute C/S server operations immediately upon client call. | Must | Test |
| SRS-IF-017 | C/S interfaces shall support timeout mechanisms (future enhancement). | Could | N/A |

#### 3.3.3 COM Layer Interfaces

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-IF-020 | The COM layer shall provide signal packing functions to serialize data elements. | Must | Test |
| SRS-IF-021 | The COM layer shall provide signal unpacking functions to deserialize data elements. | Must | Test |
| SRS-IF-022 | The COM layer shall simulate CAN frame structure with signal layout. | Should | Inspection |
| SRS-IF-023 | COM signal packing shall handle byte ordering (endianness) consistently. | Should | Test |
| SRS-IF-024 | COM shall support signal groups (multiple signals packed into one frame). | Could | N/A |

#### 3.3.4 RTE API

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-IF-030 | The RTE shall provide Rte_Write_<port>_<data> API for sending S/R signals. | Must | Inspection |
| SRS-IF-031 | The RTE shall provide Rte_Read_<port>_<data> API for receiving S/R signals. | Must | Inspection |
| SRS-IF-032 | The RTE shall provide Rte_Call_<port>_<operation> API for invoking C/S operations. | Must | Inspection |
| SRS-IF-033 | RTE APIs shall follow AUTOSAR naming conventions. | Should | Inspection |
| SRS-IF-034 | All RTE APIs shall return a status code (Std_ReturnType). | Must | Inspection |
| SRS-IF-035 | RTE APIs shall be declared with extern "C" linkage for C++ SW-Cs. | Must | Inspection |

### 3.4 User Interface Requirements

#### 3.4.1 Terminal Mode (ncurses)

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-UI-001 | The terminal UI shall display vehicle speed in km/h. | Must | Test |
| SRS-UI-002 | The terminal UI shall display current vehicle mode (Park, Drive, Reverse). | Must | Test |
| SRS-UI-003 | The terminal UI shall display current speed limit. | Must | Test |
| SRS-UI-004 | The terminal UI shall display warning light status (ON/OFF). | Must | Test |
| SRS-UI-005 | The terminal UI shall display count of stored DTCs. | Must | Test |
| SRS-UI-006 | The terminal UI shall accept arrow key input to increase/decrease simulated speed. | Must | Test |
| SRS-UI-007 | The terminal UI shall accept 'p' key to switch to Park mode. | Must | Test |
| SRS-UI-008 | The terminal UI shall accept 'd' key to switch to Drive mode. | Must | Test |
| SRS-UI-009 | The terminal UI shall accept 'r' key to switch to Reverse mode. | Must | Test |
| SRS-UI-010 | The terminal UI shall accept 'i' key to inject a diagnostic fault. | Must | Test |
| SRS-UI-011 | The terminal UI shall accept 'q' key to quit the application. | Must | Test |
| SRS-UI-012 | The terminal UI shall refresh display at minimum 10 frames per second. | Should | Test |
| SRS-UI-013 | The terminal UI shall use ncurses library for rendering. | Must | Inspection |
| SRS-UI-014 | The terminal UI shall display data within a formatted dashboard layout. | Should | Inspection |

#### 3.4.2 Qt GUI Mode

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-UI-020 | The Qt GUI shall display an analog speedometer gauge with rotating needle. | Must | Test |
| SRS-UI-021 | The Qt GUI shall display digital speed readout in km/h. | Must | Test |
| SRS-UI-022 | The Qt GUI shall provide a vehicle mode selector with Park, Drive, Reverse options. | Must | Test |
| SRS-UI-023 | The Qt GUI shall display warning indicator lights. | Must | Test |
| SRS-UI-024 | The Qt GUI shall provide a DTC viewer panel showing stored diagnostic codes. | Must | Test |
| SRS-UI-025 | The Qt GUI shall display current speed limit value. | Must | Test |
| SRS-UI-026 | The Qt GUI speedometer gauge shall update smoothly with needle animation. | Should | Inspection |
| SRS-UI-027 | The Qt GUI shall refresh display at minimum 30 frames per second. | Should | Test |
| SRS-UI-028 | The Qt GUI shall use Qt 5.12+ framework. | Must | Inspection |
| SRS-UI-029 | The Qt GUI shall provide buttons or controls for fault injection. | Should | Inspection |
| SRS-UI-030 | The Qt GUI shall highlight warning indicators when warning status is active. | Must | Test |

#### 3.4.3 UI Architecture

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-UI-040 | The application shall support a --gui command-line flag to launch Qt mode. | Must | Test |
| SRS-UI-041 | The application shall launch terminal mode by default when no flags are provided. | Must | Test |
| SRS-UI-042 | UI components shall be decoupled from SW-C logic via RTE data access only. | Must | Inspection |
| SRS-UI-043 | UI layer shall not directly invoke SW-C runnable functions. | Must | Inspection |
| SRS-UI-044 | UI layer shall read data from RTE signal buffers for display. | Must | Inspection, Test |
| SRS-UI-045 | UI layer shall write user inputs to RTE signal buffers for SW-C consumption. | Must | Inspection, Test |

### 3.5 Performance Requirements

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-PERF-001 | The task scheduler shall operate with a minimum tick resolution of 10 milliseconds. | Must | Test |
| SRS-PERF-002 | Periodic runnables scheduled at 10ms shall execute with ±2ms jitter tolerance. | Should | Test |
| SRS-PERF-003 | S/R signal propagation through RTE shall complete within one scheduler tick (10ms). | Must | Test |
| SRS-PERF-004 | C/S operation calls shall complete within 1ms under normal conditions. | Should | Test |
| SRS-PERF-005 | The terminal UI shall refresh at minimum 10 frames per second. | Must | Test |
| SRS-PERF-006 | The Qt GUI shall refresh at minimum 30 frames per second. | Must | Test |
| SRS-PERF-007 | The system shall support execution on single-core CPUs with 1 GHz or higher clock speed. | Should | Test |
| SRS-PERF-008 | Memory footprint for RTE signal buffers shall not exceed 10 KB. | Should | Analysis |
| SRS-PERF-009 | The scheduler shall support round-robin scheduling for same-priority tasks. | Must | Test |
| SRS-PERF-010 | The scheduler shall support priority-based preemptive scheduling for different-priority tasks. | Should | Test |
| SRS-PERF-011 | On-data-reception runnables shall execute within 5ms of data arrival. | Should | Test |
| SRS-PERF-012 | DTC storage operations shall complete within 1ms. | Should | Test |

### 3.6 Testing Requirements

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-TEST-001 | The project shall include unit tests for each SW-C component. | Must | Inspection |
| SRS-TEST-002 | Unit tests shall use mocked RTE interfaces to isolate SW-C logic. | Must | Inspection |
| SRS-TEST-003 | The project shall include RTE routing tests for S/R signal delivery. | Must | Inspection |
| SRS-TEST-004 | The project shall include RTE routing tests for C/S operation calls. | Must | Inspection |
| SRS-TEST-005 | The project shall include scheduler behavior tests for periodic runnables. | Must | Inspection |
| SRS-TEST-006 | The project shall include scheduler behavior tests for on-data-reception runnables. | Must | Inspection |
| SRS-TEST-007 | The project shall use Google Test framework for unit testing. | Must | Inspection |
| SRS-TEST-008 | Tests shall be buildable via CMake with ENABLE_TESTS option. | Must | Test |
| SRS-TEST-009 | All tests shall be executable via CTest command. | Must | Test |
| SRS-TEST-010 | Test coverage shall include positive test cases (expected behavior). | Must | Inspection |
| SRS-TEST-011 | Test coverage shall include negative test cases (error handling). | Should | Inspection |
| SRS-TEST-012 | Mock RTE shall provide APIs matching actual RTE function signatures. | Must | Inspection |
| SRS-TEST-013 | Tests shall validate correct runnable execution frequency for periodic components. | Should | Inspection |
| SRS-TEST-014 | Tests shall validate DTC storage and retrieval operations. | Must | Inspection |

### 3.7 Build System Requirements

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-BUILD-001 | The project shall use CMake version 3.12 or higher for build configuration. | Must | Inspection |
| SRS-BUILD-002 | The project shall compile with GCC version 7 or higher. | Must | Test |
| SRS-BUILD-003 | The project shall compile with Clang version 6 or higher. | Must | Test |
| SRS-BUILD-004 | The project shall use C++17 language standard for C++ code. | Must | Inspection |
| SRS-BUILD-005 | The project shall use C11 language standard for C code. | Should | Inspection |
| SRS-BUILD-006 | CMake shall provide ENABLE_QT_GUI option to enable/disable Qt GUI support. | Must | Test |
| SRS-BUILD-007 | CMake shall provide ENABLE_TESTS option to enable/disable unit test build. | Must | Test |
| SRS-BUILD-008 | The project shall support Debug build type with debug symbols and no optimization. | Must | Test |
| SRS-BUILD-009 | The project shall support Release build type with full optimization (-O3). | Must | Test |
| SRS-BUILD-010 | The build system shall require ncurses library for terminal mode. | Must | Test |
| SRS-BUILD-011 | The build system shall require Qt 5.12 or higher when ENABLE_QT_GUI is enabled. | Must | Test |
| SRS-BUILD-012 | The build system shall download Google Test automatically if not found on system. | Should | Test |
| SRS-BUILD-013 | The build system shall generate executable named VirtualSpeedometer. | Must | Test |
| SRS-BUILD-014 | The build system shall support out-of-source builds. | Must | Test |
| SRS-BUILD-015 | CMake configuration shall fail with clear error message if required dependencies are missing. | Should | Test |

### 3.8 Configuration Requirements

| Requirement ID | Description | Priority | Verification Method |
|----------------|-------------|----------|---------------------|
| SRS-CFG-001 | The system shall support XML-based system configuration file (ARXML-inspired). | Must | Inspection |
| SRS-CFG-002 | The system configuration shall define SW-C composition (list of components). | Must | Inspection |
| SRS-CFG-003 | The system configuration shall define port connections between SW-Cs. | Must | Inspection |
| SRS-CFG-004 | The system configuration shall define runnable mappings for each SW-C. | Must | Inspection |
| SRS-CFG-005 | The system configuration shall specify runnable trigger types (Periodic, OnDataReceived). | Must | Inspection |
| SRS-CFG-006 | The system configuration shall specify execution period for periodic runnables. | Must | Inspection |
| SRS-CFG-007 | The system shall support JSON-based RTE routing configuration. | Must | Inspection |
| SRS-CFG-008 | The RTE configuration shall define S/R signal routing (sender to receiver mappings). | Must | Inspection |
| SRS-CFG-009 | The RTE configuration shall define C/S operation routing (client to server mappings). | Must | Inspection |
| SRS-CFG-010 | Configuration files shall be loaded at system initialization time. | Must | Test |
| SRS-CFG-011 | The system shall validate configuration files and report errors if invalid. | Should | Test |
| SRS-CFG-012 | Configuration file paths shall be configurable via command-line arguments or environment variables. | Could | N/A |
| SRS-CFG-013 | The system configuration shall define speed limit values per vehicle mode. | Should | Inspection |
| SRS-CFG-014 | The RTE configuration shall support 1-to-N S/R connections (one sender, multiple receivers). | Should | Inspection |

---

## 4. Appendices

### 4.1 Traceability Matrix

This section provides traceability between requirements and implementation components.

#### 4.1.1 Functional Requirements to SW-C Mapping

| Requirement ID(s) | SW-C Component | Implementation File(s) |
|-------------------|----------------|------------------------|
| SRS-FUNC-001 to SRS-FUNC-007 | WheelSpeedSensor | src/swc/WheelSpeedSensor.cpp, WheelSpeedSensor.h |
| SRS-FUNC-010 to SRS-FUNC-016 | VehicleModeManager | src/swc/VehicleModeManager.cpp, VehicleModeManager.h |
| SRS-FUNC-020 to SRS-FUNC-025 | SpeedDisplay | src/swc/SpeedDisplay.cpp, SpeedDisplay.h |
| SRS-FUNC-030 to SRS-FUNC-037 | SpeedLimiter | src/swc/SpeedLimiter.cpp, SpeedLimiter.h |
| SRS-FUNC-040 to SRS-FUNC-046 | WarningLight | src/swc/WarningLight.cpp, WarningLight.h |
| SRS-FUNC-050 to SRS-FUNC-058 | DiagManager | src/swc/DiagManager.cpp, DiagManager.h |

#### 4.1.2 Architecture Requirements to Layer Mapping

| Requirement ID(s) | Layer/Component | Implementation File(s) |
|-------------------|-----------------|------------------------|
| SRS-ARCH-001 to SRS-ARCH-003 | Overall Architecture | All src/ directories |
| SRS-ARCH-004, SRS-ARCH-007, SRS-ARCH-013 | RTE Layer | src/bsw/rte/rte.c, rte.h, rte_internal.h |
| SRS-ARCH-005, SRS-ARCH-008, SRS-ARCH-014 | BSW Layer | src/bsw/os/scheduler.c, src/bsw/com/com.c |
| SRS-ARCH-006 | Application Layer | src/swc/*.cpp |
| SRS-ARCH-009 | SW-C Architecture | All src/swc/ components |
| SRS-ARCH-010 to SRS-ARCH-012 | UI Layer | src/gui/, src/terminal/ |
| SRS-ARCH-015 | RTE Implementation | src/bsw/rte/rte.c |

#### 4.1.3 Interface Requirements to RTE Mapping

| Requirement ID(s) | Interface Type | Implementation File(s) |
|-------------------|----------------|------------------------|
| SRS-IF-001 to SRS-IF-009 | Sender-Receiver | src/bsw/rte/rte.c (S/R routing logic) |
| SRS-IF-010 to SRS-IF-017 | Client-Server | src/bsw/rte/rte.c (C/S routing logic) |
| SRS-IF-020 to SRS-IF-024 | COM Layer | src/bsw/com/com.c, com.h |
| SRS-IF-030 to SRS-IF-035 | RTE API | src/bsw/rte/rte.h |

#### 4.1.4 User Interface Requirements to UI Mapping

| Requirement ID(s) | UI Component | Implementation File(s) |
|-------------------|--------------|------------------------|
| SRS-UI-001 to SRS-UI-014 | Terminal UI | src/terminal/terminal_ui.cpp, terminal_ui.h |
| SRS-UI-020 to SRS-UI-030 | Qt GUI | src/gui/MainWindow.cpp, SpeedometerWidget.cpp, DTCViewer.cpp |
| SRS-UI-040 to SRS-UI-045 | UI Architecture | src/main.cpp, src/gui/, src/terminal/ |

#### 4.1.5 Performance Requirements to Component Mapping

| Requirement ID(s) | Component | Implementation File(s) |
|-------------------|-----------|------------------------|
| SRS-PERF-001, SRS-PERF-002, SRS-PERF-009, SRS-PERF-010 | Task Scheduler | src/bsw/os/scheduler.c, scheduler.h |
| SRS-PERF-003, SRS-PERF-004, SRS-PERF-011 | RTE | src/bsw/rte/rte.c |
| SRS-PERF-005 | Terminal UI | src/terminal/terminal_ui.cpp |
| SRS-PERF-006 | Qt GUI | src/gui/MainWindow.cpp |
| SRS-PERF-008 | RTE Buffers | src/bsw/rte/rte_internal.h |
| SRS-PERF-012 | DiagManager | src/swc/DiagManager.cpp |

#### 4.1.6 Testing Requirements to Test Mapping

| Requirement ID(s) | Test Component | Implementation File(s) |
|-------------------|----------------|------------------------|
| SRS-TEST-001, SRS-TEST-002 | SW-C Unit Tests | tests/test_wheel_speed.cpp, test_vehicle_mode.cpp, test_speed_limiter.cpp, test_diag_manager.cpp |
| SRS-TEST-003, SRS-TEST-004 | RTE Tests | tests/test_rte_routing.cpp (planned) |
| SRS-TEST-005, SRS-TEST-006 | Scheduler Tests | tests/test_scheduler.cpp (planned) |
| SRS-TEST-007 to SRS-TEST-014 | Test Framework | tests/mocks/rte_mock.h, CMakeLists.txt |

#### 4.1.7 Build System Requirements to Build Mapping

| Requirement ID(s) | Build Component | Implementation File(s) |
|-------------------|-----------------|------------------------|
| SRS-BUILD-001 to SRS-BUILD-015 | CMake Build System | CMakeLists.txt (root and subdirectories) |

#### 4.1.8 Configuration Requirements to Config Mapping

| Requirement ID(s) | Configuration Component | Implementation File(s) |
|-------------------|-------------------------|------------------------|
| SRS-CFG-001 to SRS-CFG-006, SRS-CFG-013 | System Config | config/system_config.xml |
| SRS-CFG-007 to SRS-CFG-009, SRS-CFG-014 | RTE Config | config/rte_config.json |
| SRS-CFG-010 to SRS-CFG-012 | Config Loader | src/bsw/config_loader.c (planned) |

---

## Document Approval

This Software Requirements Specification has been reviewed and approved by:

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Lead | TBD | ___________ | 2026-02-16 |
| Lead Developer | TBD | ___________ | 2026-02-16 |
| Quality Assurance | TBD | ___________ | 2026-02-16 |

---

**End of Software Requirements Specification**

*Document Control: SRS-Virtual-Speedometer-v1.0*
*Prepared for: Virtual Speedometer AUTOSAR Simulation Project*
*License: MIT License*
