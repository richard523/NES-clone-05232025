## Inputs

The `main` function, the entry point of the application, receives inputs from the command line:
*   `NumArguments` (int): The number of command-line arguments provided when the application is launched.
*   `ArgumentValues` (char\*\*): An array of C-style strings representing the command-line arguments. These are primarily used by the `BG::NES::Config::Manager` to load configuration and potentially set the profiling status.

The application also reads from configuration files (e.g., `NES.yaml`) during the initialization of `BG::NES::Config::Manager`.

## Outputs

The `main` function itself returns an integer exit code to the operating system (typically 0 for successful completion, though the main loop is infinite unless profiling). Other significant outputs include:
*   Running network services (API server, simulation RPC interfaces) that allow external interaction with the system.
*   Log messages written to the console or log files via the `BG::Common::Logger::LoggingSystem`.
*   Console output, including the BrainGenix ASCII logo and any profiling information.
*   If profiling is enabled, specific profiling data might be generated (managed by `BG::NES::Profiling::Manager`).

## Description

The `Main.cpp` file contains the `main` function, which serves as the primary entry point and orchestrator for the BrainGenix-NES Neuron Emulation System. It initializes core components in a specific order: configuration management, logging, the RPC API server, rendering and visualizer pools, and various simulator RPC interfaces. After setup, it prints a logo and then either executes a pre-defined profiling task or enters an infinite loop to keep the system's services running.

## ASCII Diagram

```
+--------------------------------------+
| main(NumArguments, ArgumentValues)   |
+--------------------------------------+
              |
              v
+--------------------------------------+
| BG::NES::Config::Manager             | // Initialize Configuration
+--------------------------------------+
              |
              v
+--------------------------------------+
| BG::Common::Logger::LoggingSystem    | // Setup Logging
+--------------------------------------+
              |
              v
+--------------------------------------+
| BG::NES::API::RPCManager             | // Setup API Server
+--------------------------------------+
              |
              v
+--------------------------------------+
| BG::NES::Simulator::VSDA::RenderPool | // Setup Render Pool
| BG::NES::Simulator::VisualizerPool   | // Setup Visualizer Pool
+--------------------------------------+
              |
              v
+--------------------------------------+
| Simulation & Service RPC Interfaces  | // Setup various RPC interfaces
| (SimulationRPCInterface, etc.)       |
+--------------------------------------+
              |
              v
+--------------------------------------+
| BG::NES::Util::LogLogo               | // Print Logo
+--------------------------------------+
              |
              v
+--------------------------------------+
| Check Profiling Status               |
| (SystemConfiguration.ProfilingStatus_)|
+--------------------------------------+
      |        |
 (Profiling)   (Normal Run)
      |        |
      v        v
+-----------------+  +-----------------------------+
| BG::NES::       |  | while(true) {               |
| Profiling::     |  |  sleep_for(10ms);           |
| Manager         |  | }                           |
+-----------------+  +-----------------------------+
      |
      v
+--------------------------------------+
| return 0;                            |
+--------------------------------------+
```
