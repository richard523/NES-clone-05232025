## Inputs

The functions in `JSONHelpers.h` primarily take the following types of inputs:
*   `_JSON` (const nlohmann::json\* or nlohmann::json&): A pointer or reference to a `nlohmann::json` object from which data is to be extracted or to which data is to be written.
*   `_ParamName` or `_JSONKey` (std::string): The key/name of the parameter to access within the JSON object.
*   `_Input` (std::string): For some `SetVec3` functions, this is a string representation of a JSON array (e.g., `"[1.0, 2.0, 3.0]"`).
*   `_Vector` (various types like `Simulator::Geometries::Vec3D&` or `std::vector<int>*`): References or pointers to data structures to be populated from JSON or written into JSON.
*   `_Prefix` (std::string), `_Units` (std::string): String components used to construct JSON keys, typically for `Vec3D` components (e.g., "PositionX_um").

## Outputs

The functions in this module produce outputs in several ways:
*   **Return values:**
    *   `GetInt`, `GetFloat`, `GetString` return the extracted primitive value. `GetFloat` returns -1 on error.
*   **Modification of input parameters (by reference or pointer):**
    *   `SetVec3` functions modify the target `nlohmann::json` object or `Simulator::Geometries::Vec3D` object.
    *   `GetVec3`, `GetArrVec3` modify the input `Simulator::Geometries::Vec3D` object.
    *   `GetIntVector`, `GetFloatVector`, `GetStringVector` modify the input `std::vector` object.
*   **Console output (error reporting):**
    *   `GetFloat` prints error messages to `std::cerr` if a type or parse error occurs.
    *   `GetArrVec3` prints an error message to `std::cout` if the retrieved JSON array does not contain exactly 3 elements.

## Description

The `JSONHelpers.cpp` file, along with its header `JSONHelpers.h`, provides a suite of utility functions under the `BG::NES::Util` namespace. These functions are designed to simplify common operations involving the `nlohmann::json` library. They facilitate the extraction of basic data types (integers, floats, strings), `Simulator::Geometries::Vec3D` vectors (from various JSON representations), and vectors of primitive types from JSON objects. Additionally, helper functions are provided to serialize `Vec3D` objects into JSON. Some basic error reporting to `std::cerr` or `std::cout` is included for specific error conditions.

## ASCII Diagram

```
+------------------------------------------------------+
| Namespace: BG::NES::Util                             |
+------------------------------------------------------+
|                                                      |
|  Functions:                                          |
|                                                      |
|  + GetInt(_JSON*, _ParamName): int                   |
|  + GetFloat(_JSON*, _ParamName): float               |
|  + GetString(_JSON*, _ParamName): string             |
|                                                      |
|  + SetVec3(_TargetJSON*, _InputStr, _Prefix, _Units) |
|  + SetVec3(_VectorVec3D&, _InputStr)                 |
|  + SetVec3(_TargetJSON&, _InputVec3D, _Prefix, _Units)|
|                                                      |
|  + GetVec3(_VectorVec3D&, _InputJSON*, _Prefix, _Units)|
|  + GetArrVec3(_VectorVec3D&, _InputJSON*, _Name)     |
|                                                      |
|  + GetIntVector(_VectorInt*, _InputJSON*, _JSONKey)  |
|  + GetFloatVector(_VectorFloat*, _InputJSON*, _JSONKey)|
|  + GetStringVector(_VectorString*, _InputJSON*, _JSONKey)|
|                                                      |
+------------------------------------------------------+
```
