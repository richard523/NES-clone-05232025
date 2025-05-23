## Inputs

The `Manager` class receives inputs primarily through its constructor:
*   `_NumArgs` (int): The count of command-line arguments.
*   `_Args` (char\*\*): An array of C-style strings representing the command-line arguments.
    *   If `_NumArgs > 1`, `_Args[1]` is interpreted as an integer to set the `ProfilingStatus` in the internal `Config_` object.

The constructor also instantiates `ConfigFileParser`, which reads from a YAML configuration file (e.g., `NES.yaml`) to populate the internal `Config_` object.

## Outputs

The primary output of the `Manager` class is the provision of configuration data to other parts of the system:
*   `GetConfig()` method: Returns a reference to the internal `Config_` struct, which contains all loaded and processed configuration settings.
*   The constructor indirectly influences program behavior by setting the `ProfilingStatus_` based on command-line arguments.

## Description

The `ConfigurationManager.cpp` file, along with its header `ConfigurationManager.h`, defines the `Manager` class. This class is central to handling application configuration in BrainGenix-NES. It orchestrates the loading of settings from a YAML configuration file (via `ConfigFileParser`) and can also process command-line arguments to override certain parameters like the profiling mode. The `Manager` class then provides access to these consolidated configuration settings for other modules within the application through its `GetConfig()` method.

## ASCII Diagram

```
+--------------------------+
|  Manager (Config)        |
+--------------------------+
| - Config_: Config        |  // Internal Config struct instance
+--------------------------+
| + Manager(               |
|     _NumArgs: int,       |
|     _Args: char**        |
|   )                      |  // Initializes Config_ using ConfigFileParser
|                          |  // and command-line args.
| + ~Manager()             |
| + GetConfig(): &Config   |  // Returns reference to Config_
+--------------------------+
        |
        | Uses
        v
+--------------------------+
|  ConfigFileParser        |
+--------------------------+
| + ConfigFileParser(      |
|     _Config: &Config     |
|   )                      |
| ...                      |
+--------------------------+
```
