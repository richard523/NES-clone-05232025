## Inputs

The primary function `VisualizeSimulation` takes the following inputs:
*   `_Logger` (BG::Common::Logger::LoggingSystem\*): Pointer to the logging system instance.
*   `_Renderer` (Renderer::Interface\*): Pointer to an object implementing the renderer interface, used for scene management and rendering.
*   `_Simulation` (Simulation\*): Pointer to the `Simulation` object that contains the data to be visualized. This includes:
    *   Simulation components (e.g., BS compartments) used by `BuildMeshFromSimulation`.
    *   `_Simulation->VisualizerParams`: Parameters controlling visualization aspects (e.g., `VisualizeElectrodes`).
    *   `_Simulation->ID`: Used for creating unique output directory paths.
*   `_ImageProcessorPool` (Visualizer::ImageProcessorPool\*): Pointer to a pool of image processors, likely for post-processing rendered images.

The helper function `VSCreateDirectoryRecursive` takes:
*   `dirName` (std::string const &): The path of the directory to create.
*   `err` (std::error_code &): A reference to an error code object to report filesystem errors.

## Outputs

*   **`VisualizeSimulation` function:**
    *   Returns `bool`: `true` if the function completes (error handling for underlying mesh building or rendering calls is not explicit in this function's return value, but assertions check for null pointers).
    *   **Side Effects:**
        *   Creates image files in a subdirectory within `Visualizations/` (e.g., `Visualizations/[SimulationID]/`). The actual file creation is handled by `RenderVisualization`.
        *   Modifies the state of the `_Renderer` object (e.g., `_Renderer->ResetScene()`).
*   **`VSCreateDirectoryRecursive` function:**
    *   Returns `bool`: `true` if the directory was created successfully or already existed. `false` if an error occurred.
    *   Sets the `err` (std::error_code) parameter if a filesystem error occurs during directory creation.

## Description

The `Visualizer.cpp` file, along with its header `Visualizer.h`, defines the core logic for generating visual representations of simulations within the BrainGenix-NES system. The main function, `VisualizeSimulation`, orchestrates this process. It first directs the construction of a 3D mesh based on the data in a given `Simulation` object (handled by `BuildMeshFromSimulation` and optionally `BuildMeshFromElectrodes`). Then, it instructs the rendering of this mesh into image files (handled by `RenderVisualization`), which are saved to a simulation-specific directory. The module also includes a utility function, `VSCreateDirectoryRecursive`, for ensuring the output directory structure exists.

## ASCII Diagram

```
+---------------------------------------------------------------------------------+
| VisualizeSimulation(_Logger*, _Renderer*, _Simulation*, _ImageProcessorPool*)   |
+---------------------------------------------------------------------------------+
| 1. _Logger->Log("Starting Visualization...")                                    |
| 2. _Renderer->ResetScene()                                                      |
|                                                                                 |
| 3. BuildMeshFromSimulation(_Logger, _Renderer, _Simulation)                     |
|    (Calls functions from Visualizer/MeshBuilder.h)                              |
|                                                                                 |
| 4. If _Simulation->VisualizerParams.VisualizeElectrodes:                        |
|    BuildMeshFromElectrodes(_Logger, _Renderer, _Simulation)                     |
|    (Calls functions from Visualizer/MeshBuilder.h)                              |
|                                                                                 |
| 5. TargetDirectory = "Visualizations/" + _Simulation->ID + "/"                  |
| 6. VSCreateDirectoryRecursive(TargetDirectory, &errorCode)                      |
|                                                                                 |
| 7. RenderVisualization(_Logger, _Renderer, &_Simulation->VisualizerParams,      |
|                        TargetDirectory, _ImageProcessorPool)                    |
|    (Calls functions from Visualizer/MeshRenderer.h)                             |
|                                                                                 |
| 8. Return true                                                                  |
+---------------------------------------------------------------------------------+
       |                                       |                                      |
       | Uses                                  | Uses                                 | Uses
       v                                       v                                      v
+--------------------------+  +--------------------------------+  +--------------------------------------+
| BuildMeshFromSimulation  |  | RenderVisualization            |  | VSCreateDirectoryRecursive           |
| (MeshBuilder.h)          |  | (MeshRenderer.h)               |  | (filesystem utility)                 |
+--------------------------+  +--------------------------------+  +--------------------------------------+

```
