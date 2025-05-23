## Inputs

The `VSDARPCInterface` class is initialized with:
*   `_Logger` (BG::Common::Logger::LoggingSystem\*): Pointer to the logging system.
*   `_RPCManager` (API::RPCManager\*): Pointer to the main RPC manager, used for registering VSDA-specific routes.
*   `_SimulationsVectorPointer` (std::vector<std::unique_ptr<Simulation>>\*): Pointer to the vector of active simulation instances.

The various RPC handler methods exposed by this class take a single `std::string _JSONRequest` as input. This JSON string is expected to contain:
*   `SimulationID` (integer): To identify the target simulation instance.
*   Command-specific parameters, for example:
    *   For `VSDAEMSetupMicroscope`: `PixelResolution_nm`, `ImageWidth_px`, `ImageHeight_px`, various noise and rendering parameters.
    *   For `VSDAEMDefineScanRegion`: `Point1_um` (array), `Point2_um` (array), `SampleRotation_rad` (array).
    *   For `VSDAEMQueueRenderOperation` and `VSDAEMGetImageStack`: `ScanRegionID`.
    *   For `VSDAGetImage`: `ImageHandle` (string).
    *   Similar parameters for Calcium (`Ca`) routes.

## Outputs

Each RPC handler method returns a `std::string` which is a JSON formatted response. Common fields in the response include:
*   `StatusCode` (integer): Indicates the success or failure of the operation (typically 0 for success).
Specific methods may include additional output fields:
*   `VSDAEMDefineScanRegion`: `ScanRegionID`.
*   `VSDAEMGetRenderStatus`: `RenderStatus`, `CurrentSlice`, `TotalSlices`, etc.
*   `VSDAEMGetImageStack`: `RenderedImages` (an array of image paths/handles).
*   `VSDAEMGetIndexData`: Detailed index information for a scan region and its images.
*   `VSDAEMGetDatasetHandle`: `DatasetHandle` for Neuroglancer.
*   `VSDAEMGetNeuroglancerDatasetURL`: `NeuroglancerURL`.
*   `VSDAGetImage`: `ImageData` (Base64 encoded image data).
*   Similar specific outputs for Calcium (`Ca`) routes.

The methods also interact with `Simulation` objects, modifying their state (e.g., setting up VSDA parameters, queueing render operations, storing results).

## Description

The `VSDARPCInterface.cpp` file, along with its header `VSDARPCInterface.h`, implements the RPC interface for the VSDA (Voltage Sensitive Dye Array) subsystem of the BrainGenix-NES simulator. This class defines and registers a comprehensive set of RPC routes that allow clients to control and interact with both Electron Microscopy (EM) and Calcium Imaging (Ca) simulation and rendering functionalities.

Key responsibilities include:
*   Initializing VSDA simulations.
*   Configuring microscope and scan parameters.
*   Defining scan regions.
*   Queueing and managing rendering operations.
*   Retrieving render status and results (image stacks, index data).
*   Preparing and providing access to Neuroglancer datasets.
*   Fetching individual image files (Base64 encoded).

It uses the `API::HandlerData` helper for parsing requests and interacts with underlying VSDA logic functions (found in `VSDA/RPCRoutes/EM.h` and `VSDA/RPCRoutes/Ca.h`) to perform the core operations.

## ASCII Diagram

```
+-----------------------------------------------------------------+
| VSDARPCInterface                                                |
+-----------------------------------------------------------------+
| - SimulationsPtr_*: std::vector<unique_ptr<Simulation>>*        |
| - Logger_*: LoggingSystem*                                      |
| - RPCManager_*: API::RPCManager*                                |
+-----------------------------------------------------------------+
| + VSDARPCInterface(_Logger*, _RPCManager*, _SimulationsPtr_*)   | // Constructor, registers routes
| + ~VSDARPCInterface()                                           |
|                                                                 |
|   // --- EM Routes (Input: JSON string, Output: JSON string) --- |
| + VSDAEMInitialize(_JSONRequest): string                        |
| + VSDAEMSetupMicroscope(_JSONRequest): string                   |
| + VSDAEMDefineScanRegion(_JSONRequest): string                  |
| + VSDAEMQueueRenderOperation(_JSONRequest): string              |
| + VSDAEMGetRenderStatus(_JSONRequest): string                   |
| + VSDAEMGetImageStack(_JSONRequest): string                     |
| + VSDAEMGetIndexData(_JSONRequest): string                      |
| + VSDAEMPrepareNeuroglancerDataset(_JSONRequest): string        |
| + VSDAEMGetDatasetHandle(_JSONRequest): string                  |
| + VSDAEMGetNeuroglancerDatasetURL(_JSONRequest): string         |
|                                                                 |
|   // --- Ca Routes (Input: JSON string, Output: JSON string) --- |
| + VSDACAInitialize(_JSONRequest): string                        |
| + VSDACASetupMicroscope(_JSONRequest): string                   |
| + VSDACADefineScanRegion(_JSONRequest): string                  |
| + VSDACAQueueRenderOperation(_JSONRequest): string              |
| + VSDACAGetRenderStatus(_JSONRequest): string                   |
| + VSDACAGetImageStack(_JSONRequest): string                     |
|                                                                 |
|   // --- Common Route ---                                        |
| + VSDAGetImage(_JSONRequest): string                            |
+-----------------------------------------------------------------+
        | Uses                                  | Calls
        v                                       v
+-----------------------+     +---------------------------------------+
| API::HandlerData      |     | VSDA EM/Ca Logic Functions            |
| (for request parsing) |     | (e.g., VSDA_EM_Initialize,            |
+-----------------------+     |  VSDA_CA_SetupMicroscope from         |
                              |  VSDA/RPCRoutes/EM.h, Ca.h)           |
                              +---------------------------------------+
```
