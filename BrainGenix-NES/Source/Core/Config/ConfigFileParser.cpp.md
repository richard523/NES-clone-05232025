## Inputs

The primary input to the `ConfigFileParser` class is through its constructor:
*   `_Config` (reference to a `BG::NES::Config::Config` struct): This struct is passed by reference.
    *   Its `ConfigFilePaths` member (a `std::vector<std::string>`) provides a list of potential paths where the YAML configuration file might be located.
    *   The constructor uses these paths to find and load the `NES.yaml` (or equivalent) file.

The class also implicitly reads from the filesystem, specifically the YAML configuration file identified.

## Outputs

The main output is the modification of the `_Config` struct passed to the constructor. The constructor populates the following members of the `_Config` struct based on the content of the YAML file:
*   `PortNumber`
*   `Host`
*   `MaxVoxelArraySize_`
*   `VoxelArrayPercentOfSystemMemory_`

If the configuration file cannot be found or parsed, the program logs an error to `std::cerr` and calls `exit(1)`, effectively terminating the application.

## Description

The `ConfigFileParser.cpp` file, along with its header `ConfigFileParser.h`, defines a class responsible for loading and interpreting the application's configuration from a YAML file. It iterates through a list of predefined paths to locate the configuration file (e.g., "NES.yaml"). Upon successful parsing, it populates a `Config` struct with parameters such as network settings and memory limits, making these settings available to other parts of the BrainGenix-NES system. Error handling is included to manage file access and parsing issues, leading to program termination if critical configuration data cannot be loaded.

## ASCII Diagram

```
+--------------------------+
|  ConfigFileParser        |
+--------------------------+
|                          | // No public fields
+--------------------------+
| + ConfigFileParser(      |
|     _Config: &Config     | // Input: Config struct to populate
|   )                      | // Reads YAML file, populates _Config
|                          | // Exits on critical error
| + ~ConfigFileParser()    |
+--------------------------+
        |
        | Reads from
        |
+--------------------------+
|  NES.yaml (example)      |
|--------------------------|
| Network_NES_API_Port: ...|
| Network_NES_API_Host: ...|
| VSDA_EM_MaxVoxelArraySize: ...|
| VSDA_EM_PercentOfSysteMemoryLimit: ...|
+--------------------------+
        |
        | Populates
        v
+--------------------------+
|  Config (struct)         |
+--------------------------+
|   PortNumber             |
|   Host                   |
|   MaxVoxelArraySize_     |
|   VoxelArrayPercentOfSystemMemory_ |
|   ...                    |
+--------------------------+
```
