# Virtual Speedometer - AUTOSAR Application Layer Simulation

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)](#)
[![C++](https://img.shields.io/badge/C++-17-blue.svg)](#)
[![Qt](https://img.shields.io/badge/Qt-5.x-green.svg)](#)

## Description

The **Virtual Speedometer** is a portfolio project demonstrating professional C/C++ development and AUTOSAR (AUTomotive Open System ARchitecture) application layer concepts. This educational simulation implements a digital vehicle dashboard with multiple Software Components (SW-Cs) communicating through a simulated Runtime Environment (RTE), following AUTOSAR architectural principles.

This project showcases:
- AUTOSAR application layer architecture with SW-Cs, ports, and interfaces
- Mixed C/C++ development (C for BSW/RTE layers following MISRA-C guidelines, C++ for application components)
- Real-time embedded systems concepts (task scheduling, inter-component communication)
- Modern build systems (CMake) and testing practices (unit tests with mocked RTE)
- Dual UI modes: ncurses terminal dashboard and Qt-based graphical interface

**Note:** This is a demonstration/learning project and not production-ready automotive software.

## Features

### Core AUTOSAR Concepts
- **Software Components (SW-Cs)**: Independent, reusable application components
- **Simulated Runtime Environment (RTE)**: Middleware layer routing signals between components
- **Sender-Receiver (S/R) Interfaces**: Asynchronous data communication between components
- **Client-Server (C/S) Interfaces**: Synchronous service-oriented communication
- **Runnables with Trigger Types**: Periodic execution and data-reception-triggered execution
- **Port-Based Communication**: Required and provided ports with well-defined interfaces

### Application Components
- **WheelSpeedSensor SW-C**: Simulates wheel rotation sensor (periodic runnable)
- **VehicleModeManager SW-C**: Manages vehicle state - Park/Drive/Reverse (periodic runnable)
- **SpeedDisplay SW-C**: Displays current speed on dashboard (on-data-reception trigger)
- **SpeedLimiter SW-C**: Enforces speed limits based on vehicle mode (on-data-reception trigger)
- **WarningLight SW-C**: Activates overspeed warnings (on-data-reception trigger)
- **DiagManager SW-C**: Handles Diagnostic Trouble Codes (DTC) storage and retrieval (on-data-reception trigger)

### Basic Services (BSW Layer)
- **Task Scheduler**: Round-robin and priority-based scheduling simulating AUTOSAR OS
- **COM Layer**: Signal packing/unpacking simulating CAN frame handling
- **Configuration Management**: XML/JSON-based system description (inspired by ARXML)

### User Interface
- **Terminal Mode**: ncurses-based text dashboard with real-time updates
- **Qt GUI Mode**: Graphical speedometer with analog gauge, warning indicators, mode selector, and DTC viewer

### Build and Test
- **CMake Build System**: Cross-platform build configuration
- **Unit Testing**: Component-level tests with mocked RTE interfaces
- **Modular Architecture**: Clean separation between BSW, RTE, SW-Cs, and UI layers

## Architecture

### System Architecture Diagram

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ WheelSpeed   │    │ VehicleMode  │    │ DiagManager  │
│ Sensor       │    │ (Park/Drive) │    │ (DTC store)  │
│ [Periodic]   │    │ [Periodic]   │    │ [OnReceive]  │
└──────┬───────┘    └──────┬───────┘    └──────▲───────┘
       │S/R                │S/R                │S/R
═══════╪══════════════════╪════════════════════╪═══════
       │          Simulated RTE (rte.c)        │
       │       - Sender/Receiver routing       │
       │       - Client/Server calls           │
═══════╪══════════════════╪════════════════════╪═══════
       │S/R               │C/S                 │S/R
┌──────▼───────┐    ┌─────▼────────┐    ┌──────┴───────┐
│ SpeedDisplay │    │ SpeedLimiter │    │ WarningLight │
│ [OnReceive]  │    │ [OnReceive]  │    │ [OnReceive]  │
└──────────────┘    └──────────────┘    └──────────────┘
       │                   │                   │
       └───────────────────┴───────────────────┘
                           │
                    ┌──────▼──────┐
                    │  UI Layer   │
                    │ (Qt / Term) │
                    └─────────────┘

Legend:
  S/R = Sender-Receiver Interface (asynchronous)
  C/S = Client-Server Interface (synchronous)
```

### Layer Separation

1. **Application Layer (C++)**: SW-Cs implementing business logic
2. **RTE Layer (C)**: Communication middleware, follows MISRA-C guidelines
3. **BSW Layer (C)**: Basic software services (OS scheduler, COM), follows MISRA-C guidelines
4. **UI Layer (C++/Qt)**: Visualization and user interaction

## Getting Started

### Prerequisites

#### Required
- **C++ Compiler**: GCC 7+ or Clang 6+ with C++17 support
- **CMake**: Version 3.12 or higher
- **ncurses**: Terminal UI library
  ```bash
  # Ubuntu/Debian
  sudo apt-get install libncurses5-dev libncursesw5-dev

  # Fedora/RHEL
  sudo dnf install ncurses-devel

  # macOS
  brew install ncurses
  ```

#### Optional (for Qt GUI)
- **Qt Framework**: Qt 5.12 or higher
  ```bash
  # Ubuntu/Debian
  sudo apt-get install qt5-default qtbase5-dev

  # Fedora/RHEL
  sudo dnf install qt5-qtbase-devel

  # macOS
  brew install qt@5
  ```

#### Optional (for testing)
- **Google Test**: For unit testing (automatically downloaded by CMake if not found)

### Build Instructions

#### Terminal Mode (ncurses only)

```bash
# Clone the repository
git clone <repository-url>
cd Virtual_Speedometer

# Create build directory
mkdir build && cd build

# Configure and build
cmake ..
make

# Run the application
./VirtualSpeedometer
```

#### Qt GUI Mode

```bash
# Create build directory
mkdir build && cd build

# Configure with Qt support
cmake -DENABLE_QT_GUI=ON ..
make

# Run the application
./VirtualSpeedometer --gui
```

#### Build Options

```bash
# Enable Qt GUI support
cmake -DENABLE_QT_GUI=ON ..

# Enable unit tests
cmake -DENABLE_TESTS=ON ..

# Enable debug symbols
cmake -DCMAKE_BUILD_TYPE=Debug ..

# Release build with optimizations
cmake -DCMAKE_BUILD_TYPE=Release ..

# Combine multiple options
cmake -DENABLE_QT_GUI=ON -DENABLE_TESTS=ON -DCMAKE_BUILD_TYPE=Release ..
```

## Usage

### Terminal Mode

Launch the terminal-based dashboard:

```bash
./VirtualSpeedometer
```

**Controls:**
- Arrow keys: Increase/decrease simulated wheel speed
- `p`: Toggle Park mode
- `d`: Toggle Drive mode
- `r`: Toggle Reverse mode
- `i`: Inject diagnostic fault (trigger DTC)
- `q`: Quit application

**Display:**
```
╔════════════════════════════════════════╗
║      VIRTUAL SPEEDOMETER DASHBOARD     ║
╠════════════════════════════════════════╣
║ Speed:      45 km/h                    ║
║ Mode:       DRIVE                      ║
║ Limit:      120 km/h                   ║
║ Warning:    [ OFF ]                    ║
║ DTCs:       0 stored                   ║
╚════════════════════════════════════════╝
```

### Qt GUI Mode

Launch the graphical dashboard:

```bash
./VirtualSpeedometer --gui
```

**Features:**
- Analog speedometer gauge with real-time needle movement
- Digital speed readout
- Vehicle mode selector (Park/Drive/Reverse)
- Warning indicator lights
- DTC viewer panel showing stored diagnostic codes
- Speed limit display

## Project Structure

```
Virtual_Speedometer/
├── CMakeLists.txt              # Root build configuration
├── README.md                   # This file
├── LICENSE                     # Project license
│
├── config/                     # Configuration files
│   ├── system_config.xml       # System description (ARXML-like)
│   └── rte_config.json         # RTE routing configuration
│
├── src/
│   ├── bsw/                    # Basic Software (C, MISRA-C)
│   │   ├── rte/                # Runtime Environment
│   │   │   ├── rte.c           # RTE implementation
│   │   │   ├── rte.h           # RTE public API
│   │   │   └── rte_internal.h  # RTE private definitions
│   │   ├── os/                 # OS scheduler simulation
│   │   │   ├── scheduler.c     # Task scheduler
│   │   │   └── scheduler.h
│   │   └── com/                # COM layer (signal packing)
│   │       ├── com.c           # COM implementation
│   │       └── com.h
│   │
│   ├── swc/                    # Application Software Components (C++)
│   │   ├── WheelSpeedSensor.cpp
│   │   ├── WheelSpeedSensor.h
│   │   ├── VehicleModeManager.cpp
│   │   ├── VehicleModeManager.h
│   │   ├── SpeedDisplay.cpp
│   │   ├── SpeedDisplay.h
│   │   ├── SpeedLimiter.cpp
│   │   ├── SpeedLimiter.h
│   │   ├── WarningLight.cpp
│   │   ├── WarningLight.h
│   │   ├── DiagManager.cpp
│   │   └── DiagManager.h
│   │
│   ├── gui/                    # Qt GUI (C++/Qt)
│   │   ├── MainWindow.cpp
│   │   ├── MainWindow.h
│   │   ├── SpeedometerWidget.cpp
│   │   ├── SpeedometerWidget.h
│   │   ├── DTCViewer.cpp
│   │   └── DTCViewer.h
│   │
│   ├── terminal/               # Terminal UI (ncurses)
│   │   ├── terminal_ui.cpp
│   │   └── terminal_ui.h
│   │
│   └── main.cpp                # Application entry point
│
├── tests/                      # Unit tests
│   ├── mocks/                  # Mocked RTE for testing
│   │   └── rte_mock.h
│   ├── test_wheel_speed.cpp
│   ├── test_vehicle_mode.cpp
│   ├── test_speed_limiter.cpp
│   └── test_diag_manager.cpp
│
└── docs/                       # Additional documentation
    ├── ARCHITECTURE.md         # Detailed architecture
    ├── AUTOSAR_CONCEPTS.md     # AUTOSAR concepts explained
    └── API_REFERENCE.md        # Component API reference
```

## Testing

### Running Unit Tests

```bash
# Build with tests enabled
mkdir build && cd build
cmake -DENABLE_TESTS=ON ..
make

# Run all tests
ctest

# Or run test executable directly
./tests/VirtualSpeedometer_tests

# Run specific test suite
./tests/VirtualSpeedometer_tests --gtest_filter=WheelSpeedSensor.*
```

### Test Coverage

The project includes unit tests for:
- Individual SW-C runnables with mocked RTE interfaces
- RTE signal routing (Sender-Receiver)
- RTE service calls (Client-Server)
- Task scheduler behavior
- DTC storage and retrieval

## Configuration

### System Configuration (config/system_config.xml)

Defines the SW-C composition, port connections, and runnable mappings:

```xml
<SystemDescription>
  <Components>
    <SWC name="WheelSpeedSensor" type="SensorActuator">
      <Runnable name="WheelSpeed_Run" trigger="Periodic" period="10ms"/>
      <ProvidedPort name="Speed" interface="SR_Speed"/>
    </SWC>
    <!-- Additional components... -->
  </Components>
</SystemDescription>
```

### RTE Configuration (config/rte_config.json)

Defines signal routing and inter-component connections:

```json
{
  "connections": [
    {
      "sender": "WheelSpeedSensor.Speed",
      "receiver": "SpeedDisplay.SpeedInput",
      "type": "SenderReceiver"
    }
  ]
}
```

## Roadmap

### Completed
- Basic SW-C architecture with RTE
- Sender-Receiver interfaces
- Terminal UI with ncurses
- Configuration via XML/JSON

### Planned Enhancements
- [x] Client-Server interface implementation
- [x] Qt GUI dashboard
- [x] Diagnostic Manager with DTC support
- [ ] NvM (Non-volatile Memory) simulation for persistent DTC storage
- [ ] Mode Manager for system mode transitions
- [ ] CAN signal database integration
- [ ] Real-time performance metrics
- [ ] Additional SW-Cs (FuelGauge, TemperatureMonitor)
- [ ] SOME/IP communication simulation
- [ ] Static analysis with MISRA-C checker

### Future Ideas
- Integration with Vector CANoe/CANalyzer
- Support for AUTOSAR Adaptive Platform concepts
- Docker container for easy deployment
- Web-based dashboard using WebSocket communication

## Key AUTOSAR Concepts Demonstrated

### Software Components (SW-Cs)
Independent units of functionality that encapsulate application logic. Each component has clearly defined ports and interfaces, making it reusable and testable in isolation.

### Ports and Interfaces
Components communicate through **Required Ports** (data/services needed) and **Provided Ports** (data/services offered). Interfaces define the contract between components.

### Runtime Environment (RTE)
The middleware layer that routes signals and service calls between components. Components never call each other directly - all communication flows through the RTE, ensuring loose coupling.

### Runnables
Functions within SW-Cs that are executed by the AUTOSAR OS. This project demonstrates:
- **Periodic runnables**: Triggered at fixed time intervals
- **On-data-reception runnables**: Triggered when new data arrives

### Basic Software (BSW)
Low-level services like communication stacks and operating system functionality, implemented in C following MISRA-C guidelines for safety and reliability.

## Development Guidelines

### Code Style
- **BSW/RTE Layer (C)**: Follow MISRA-C:2012 guidelines where practical
- **Application Layer (C++)**: Modern C++17 style, Google C++ Style Guide
- **Naming Conventions**: Follow AUTOSAR naming conventions (SW-C names, port names)

### MISRA-C Compliance
The BSW and RTE layers aim to follow MISRA-C guidelines:
- No dynamic memory allocation in RTE layer
- Defensive programming with explicit error checking
- Avoid undefined behavior
- Use of static analysis tools recommended

### Version Control
- Feature branches for new components or features
- Descriptive commit messages
- Code review before merging to main

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- AUTOSAR specifications and documentation
- Automotive software engineering best practices
- Open-source embedded systems community

## Contact

For questions, suggestions, or feedback about this portfolio project, please open an issue in the repository.

---

**Disclaimer**: This is an educational project for demonstrating AUTOSAR concepts and C/C++ development skills. It is not certified for automotive use and should not be used in production vehicles.
