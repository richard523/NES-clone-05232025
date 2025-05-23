## Inputs

The `Engine` class methods primarily take a pointer to a `Simulation` object as input:
*   `Reset(Simulation* _Sim)`:
    *   `_Sim`: Pointer to the `Simulation` object to be reset.
*   `RunFor(Simulation* _Sim)`:
    *   `_Sim`: Pointer to the `Simulation` object to be run. The duration of the run is expected to be defined within the `_Sim` object itself (specifically `_Sim->RunTimes_ms`).

## Outputs

The methods of the `Engine` class have side effects on the `Simulation` object passed to them:
*   `Reset(Simulation* _Sim)`: Modifies the state of the `_Sim` object by resetting its internal components (PatchClampDACs, PatchClampADCs, Receptors, Staples) to their default or initial states.
*   `RunFor(Simulation* _Sim)`: Advances the state of the `_Sim` object by executing the simulation logic for the duration specified in `_Sim->RunTimes_ms`. This typically involves updating neuron states, handling interactions, etc.

The methods themselves do not return any values.

## Description

The `Engine.cpp` file, along with its header `Engine.h`, defines the `Engine` class within the `BG::NES::Simulator` namespace. This class provides the primary control interface for managing and executing simulations. It offers functionalities to reset a given `Simulation` object to its initial conditions and to run the simulation for a period defined within the `Simulation` object. The `Engine` delegates the detailed simulation mechanics to the `Simulation` object itself and various `Updater` modules.

## ASCII Diagram

```
+--------------------------------------+
|  Engine                              |
+--------------------------------------+
|                                      |
+--------------------------------------+
| + Engine()                           |
| + ~Engine()                          |
| + Reset(_Sim: Simulation*)           |
|   - Calls Updater::PatchClampDACReset|
|   - Calls Updater::PatchClampADCReset|
|   - Calls Updater::ReceptorReset     |
|   - Calls Updater::StapleReset       |
| + RunFor(_Sim: Simulation*)          |
|   - Calls _Sim->RunFor(...)          |
+--------------------------------------+
        |
        | (Operates on)
        v
+--------------------------------------+
|  Simulation                          |
+--------------------------------------+
| + PatchClampDACs[]                   |
| + PatchClampADCs[]                   |
| + Receptors[]                        |
| + Staples[]                          |
| + RunTimes_ms                        |
| + RunFor(duration)                   |
| ... (other simulation data/logic)    |
+--------------------------------------+
```
