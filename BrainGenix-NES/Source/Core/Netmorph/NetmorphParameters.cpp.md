## Inputs

The `NetmorphParameters.cpp` file itself does not define executable code that processes inputs. Its corresponding header, `NetmorphParameters.h`, defines the `NetmorphParameters` struct and `NetmorphState` enum.

The `NetmorphParameters` struct is primarily populated externally before initiating a Netmorph task. Key input members include:
*   `Sim` (Simulation\*): Pointer to the main NES simulation object.
*   `ModelContent` (std::string): The string content of the Netmorph model definition.
*   `OutputDirectory` (std::string): Path to the directory where Netmorph should store its output files.
*   Initially, `State` would be set (e.g., to `Netmorph_REQUESTED`) to trigger processing by a worker thread.

## Outputs

This module primarily provides data structures. The "outputs" are the members of an instantiated `NetmorphParameters` struct after a Netmorph task has been processed:
*   `Progress_percent` (int): Updated to reflect the completion percentage of the Netmorph simulation.
*   `Result` (Netmorph): Contains the results from the Netmorph simulation run.
*   `State` (NetmorphState): Updated to reflect the current status of the Netmorph task (e.g., `Netmorph_DONE` or an error state if applicable, though error states are not explicitly part of the enum).

## Description

The `NetmorphParameters.cpp` file and its header `NetmorphParameters.h` define the data structures used to manage Netmorph simulation tasks within the BrainGenix-NES system. The core `NetmorphParameters` struct encapsulates all necessary inputs (like the model content and output directory), tracks progress, stores the results from Netmorph, and maintains the overall state of the task using the `NetmorphState` enum. This allows for asynchronous execution and monitoring of Netmorph simulations.

## ASCII Diagram

```
+--------------------------+
| struct NetmorphParameters|
+--------------------------+
| + Sim*: Simulation       | // Pointer to NES Simulation object
| + ModelContent: string   | // Netmorph model definition
| + OutputDirectory: string| // Path for Netmorph outputs
| + Progress_percent: int  | // Simulation progress
| + Result: Netmorph       | // Stores Netmorph output
| + State: NetmorphState   | // Current task state
+--------------------------+

Enum: NetmorphState
 - Netmorph_NONE
 - Netmorph_REQUESTED
 - Netmorph_WORKING
 - Netmorph_DONE
```
