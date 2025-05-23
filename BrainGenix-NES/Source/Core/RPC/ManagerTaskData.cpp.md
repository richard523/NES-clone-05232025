## Inputs

The `ManagerTaskData.cpp` file itself does not define executable code that processes inputs. Its corresponding header, `ManagerTaskData.h`, defines the `ManagerTaskData` struct.

The primary inputs for a `ManagerTaskData` instance are set externally before or during task initialization:
*   `InputData` (std::string): String data required for the task to execute.
*   `Task` (std::unique_ptr<std::thread>): A unique pointer to the `std::thread` object that will execute the task. This is typically set when the task thread is launched.
*   The `ID` is usually assigned by a task management system.

## Outputs

The `ManagerTaskData` struct is used to convey the results and status of an asynchronous task:
*   `Status` (ManagerTaskStatus): Indicates the current state or outcome of the task (e.g., `Success`, `Active`, `TimeOut`).
*   `OutputData` (nlohmann::json): A JSON object where the task can store its results. The status can also be included here using `IncludeStatusInOutputData()`.
*   `ReplaceSimulationID` (int): If the task results in needing to replace a simulation, this ID is set.

## Description

The `ManagerTaskData.h` file defines the `ManagerTaskData` struct and the `ManagerTaskStatus` enum, which are fundamental for managing asynchronous background tasks within the BrainGenix-NES API. The `ManagerTaskData` struct encapsulates all information related to a specific task, including its input, the thread executing it, its current status, and any output data generated. This structure facilitates tracking and retrieving results from operations that run independently of the main RPC handling flow.

## ASCII Diagram

```
+-----------------------------+
| struct ManagerTaskData      |
+-----------------------------+
| + InputData: string         | // Input for the task
| + Task: unique_ptr<thread>  | // Thread executing the task
| + ID: int                   | // Task identifier
| + Status: ManagerTaskStatus | // Current status of the task
| + OutputData: json          | // Output from the task
| + ReplaceSimulationID: int  | // Optional ID for sim replacement
+-----------------------------+
| + ManagerTaskData()         |
| + SetStatus(_status)        |
| + IncludeStatusInOutputData()|
| + HasReplacementSimID(): bool|
+-----------------------------+

Enum: ManagerTaskStatus
 - Success
 - Active
 - TimeOut
 - GeneralFailure
 - NUMManagerTaskStatus
```
