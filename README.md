# Calibrator

Custom strategy project for Baldur Control Systems Calibrator, focused on steering torque/angle control and CAN signal mapping for BMW E46 integration.

## Overview

This repository contains:

- Custom strategy source code (`program_code.custom`)
- Custom real-time variable definitions
- Custom configuration and flash variable definitions
- CAN bus slot mapping/configuration files
- DBC files used for CAN signal documentation/decoding
- Reference programming manual for the custom strategy language

The strategy appears to implement steering control modes, PID terms, torque limiting by target angle, and CAN TX/RX integration for steering status and control-related signals.

## Repository Contents

- `program_code.custom`: Main custom strategy source.
- `user program real time variables.custom`: Real-time variables exported to Calibrator/runtime telemetry.
- `user definied configuration variables.custom`: UI-visible configuration items and tables.
- `user definied controller flash variables.custom`: Backing flash variable/type definitions.
- `canbus_config.cfg`: Main CAN bus mapping/configuration (including additional RX slots such as OpenPilot-style signals).
- `canbus1_steering_status.cfg`: Focused CAN1 RX mapping for steering status message layout.
- `bmw.dbc`: DBC database for BMW CAN signals.
- `ocelot_controls.dbc`: DBC database related to control/system signals.
- `customstrategy_help.md`: Custom strategy programmer manual/reference.

## Prerequisites

- Baldur Control Systems Calibrator installed.
- ECU/firmware that supports custom strategies and required variables.
- Correct CAN wiring, bus speed, and message definitions for your vehicle/network.

## Typical Workflow

1. Open Calibrator and load your ECU configuration.
2. Import or paste `program_code.custom` into the custom strategy code section.
3. Import the variable definition files:
   - `user program real time variables.custom`
   - `user definied configuration variables.custom`
   - `user definied controller flash variables.custom`
4. Apply CAN mappings from:
   - `canbus_config.cfg`
   - `canbus1_steering_status.cfg` (if used separately in your setup)
5. Verify IDs, byte offsets, scaling, and endianness against your target vehicle and DBC.
6. Compile and flash/apply configuration.
7. Validate in logs/telemetry before road testing.

## Safety Notes

- Treat all steering-related outputs as safety-critical.
- Validate behavior off-road first (bench/sim or controlled environment).
- Keep conservative torque/limit settings during initial tuning.
- Ensure immediate manual override/disengage paths are functional.

## Tuning Notes

Important tunables are defined under the steering control configuration group, including:

- Duty clamp range
- Integral clamp range
- `steeringkp`, `steeringki`, `steeringkd`
- `targetdelayticks`
- Target-angle-to-max-torque table (`targettorqueaxis` and `targettorquelimit`)

Start with low gains and conservative torque limits, then increase gradually while monitoring stability and actuator response.

## Language Reference

See `customstrategy_help.md` for syntax, built-ins, callbacks, variable types, and examples of the custom strategy language.

## Disclaimer

This repository is for research/development use. You are responsible for safe integration, testing, and legal compliance in your environment.
