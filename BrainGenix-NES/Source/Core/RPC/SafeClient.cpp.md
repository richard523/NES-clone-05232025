## Inputs

The `SafeClient` class is initialized with:
*   `_Logger` (BG::Common::Logger::LoggingSystem\*): Pointer to the logging system.

Runtime configuration is provided via:
*   `SetHostPort(std::string _Host, int _Port)`: Sets the hostname and port for the RPC server to connect to.
*   `SetTimeout(int _Timeout_ms)`: Sets the timeout for RPC calls.

Methods for making RPC calls take:
*   `_Route` (std::string): The RPC route/endpoint to call.
*   `_Query` (std::string, optional): The JSON payload for the RPC request.
*   `_ForceQuery` (bool, optional): If true, attempts the call even if the client currently believes the connection is unhealthy.

## Outputs

The `SafeClient` primarily produces:
*   Log messages regarding connection status, errors, and version checks.
*   Boolean return values from `MakeJSONQuery` methods indicating success or failure of the RPC call.
*   The result of successful RPC calls is returned via a string pointer (`std::string* _Result`) passed to `MakeJSONQuery`.
*   The `ClientManager_` thread continuously monitors and attempts to maintain a healthy connection to the server.

## Description

The `SafeClient.cpp` file, along with its header `SafeClient.h`, defines a resilient RPC client for the BrainGenix-NES system. It wraps an `rpclib` client and adds a management layer, running in a separate thread, that periodically checks the connection health (by calling a version endpoint on the server) and attempts to reconnect if issues are detected. This class provides a more robust way to make RPC calls to an external service, handling transient errors and ensuring version compatibility.

## ASCII Diagram

```
+-------------------------------------------+
|  SafeClient                               |
+-------------------------------------------+
| - Client_: unique_ptr<rpc::client>        |
| - IsHealthy_: atomic_bool                 |
| - RequestExit_: atomic_bool               |
| - Logger_: LoggingSystem*                 |
| - ClientManager_: thread                  | // Manages connection health
| - RPCHost_: string                        |
| - RPCPort_: int                           |
| - RPCTimeout_ms: int                      |
+-------------------------------------------+
| + SafeClient(_Logger*)                    | // Starts ClientManager_ thread
| + ~SafeClient()                           | // Stops ClientManager_ thread
| + SetTimeout(_Timeout_ms): bool           |
| + SetHostPort(_Host, _Port): bool         |
| + Reconnect()                             | // Triggers reconnection attempt
| + MakeJSONQuery(_Route, *_Result, _Force): bool |
| + MakeJSONQuery(_Route, _Query, *_Result, _Force): bool |
|                                           |
| # RPCManagerThread()                      | // Loop for health check & reconnect
| # RunVersionCheck(): bool                 | // Pings server, checks version
| # Connect(): bool                         | // Establishes RPC connection
+-------------------------------------------+
```
