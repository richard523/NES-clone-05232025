## Inputs

The `HandlerData` class is initialized with:
*   `_JSONRequest` (const std::string&): The raw JSON request string from the client.
*   `_Logger` (BG::Common::Logger::LoggingSystem\*): Pointer to the logging system.
*   `_RoutePath` (std::string): The specific RPC route path being handled.
*   `_Simulations` (Simulations type, which is `std::vector<std::unique_ptr<Simulator::Simulation>>*`): Pointer to the vector of all active simulation instances.
*   `PermitBusy` (bool, default false): Flag to allow processing even if the target simulation is busy.
*   `NoSimulation` (bool, default false): Flag indicating if a simulation instance is not required for this handler.

The class then internally parses `_JSONRequest` to extract parameters for various `GetPar...` methods.

## Outputs

The methods of `HandlerData` primarily produce:
*   Formatted JSON response strings via:
    *   `ResponseAndStoreRequest()`
    *   `ErrResponse()`
    *   `ResponseWithID()`
    *   `StringResponse()`
*   The `Status` member (BGStatusCode) is updated internally based on the success or failure of parsing and parameter extraction, and this status is included in the JSON responses.
*   Extracted parameter values are returned via out-parameters in the `GetPar...` methods (e.g., `GetParInt(name, value)`).
*   If a valid simulation is associated, the request details are stored in the simulation's history via `ResponseAndStoreRequest()`.

## Description

The `RPCHandlerHelper.cpp` file, along with its header, defines the `HandlerData` class, a crucial utility for simplifying and standardizing the implementation of RPC request handlers in the BrainGenix-NES system. It encapsulates common tasks such as parsing incoming JSON requests, validating and extracting parameters of various types, retrieving the target simulation instance, and formatting JSON responses (both success and error). By providing these common functionalities, `HandlerData` helps reduce boilerplate code and enforce consistent error handling and logging across different API endpoints.

## ASCII Diagram

```
+-------------------------------------------------+
|  HandlerData                                    |
+-------------------------------------------------+
| # SimVec: Simulations                           |
| # Logger_: LoggingSystem*                       |
| # JSONRequestStr: string                        |
| # RoutePath_: string                            |
| # RequestJSON: nlohmann::json                   |
| # Status: BGStatusCode                          |
| # SimulationID: int                             |
| # ThisSimulation: Simulator::Simulation*        |
+-------------------------------------------------+
| + HandlerData(_JSONRequest, _Logger, ...)       |
| + HasError(): bool                              |
| + GetStatus(): BGStatusCode                     |
| + ResponseAndStoreRequest(json&, bool): string  |
| + ErrResponse(...): string                      |
| + ResponseWithID(...): string                   |
| + StringResponse(key, value): string            |
| + SimID(): int                                  |
| + Sim(): Simulation*                            |
| + ReqJSON(): const json&                        |
| + FindPar(name, &iterator, json&, opt): bool    |
| + GetParBool(name, &value, json&): bool         |
| + GetParInt(name, &value, json&): bool          |
| + GetParFloat(name, &value, json&): bool        |
| + GetParString(name, &value, json&): bool       |
| + GetParVec3(name, &value, json&, units): bool  |
| + GetParVecInt(name, &value, json&, opt): bool  |
| + GetParVecFloat(name, &value, json&): bool     |
+-------------------------------------------------+
```
