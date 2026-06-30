# Pulse Counter Verification Plan

## Overview

This document describes the verification strategy for the Pulse Counter design.

The objective is to verify the functional correctness of the register interface, pulse counting logic, overflow behavior, and register protection mechanisms using a directed, self-checking SystemVerilog testbench.

---

# Verification Objectives

The verification environment shall verify:

- Register reset behavior
- Register read/write functionality
- Reserved-bit protection
- Reserved address handling
- Pulse counting functionality
- Counter overflow detection
- Overflow flag persistence
- Write-One-to-Clear (W1C) functionality

---

# Test Matrix

| Category | Test | Description | Pass Criteria |
|----------|------|-------------|---------------|
| **Control Register** | Reset Value | Verify CR resets to default value | CR = `32'h0000_0000` after reset |
| | Read/Write | Verify writable control bits | Readback matches expected value |
| | Reserved Bits | Attempt to write reserved bits | Reserved bits remain unchanged |
| **Status Register** | Reset Value | Verify SR resets correctly | SR = `32'h0000_0000` after reset |
| | Read Only | Attempt to write SR | Register contents remain unchanged |
| | Reserved Bits | Write reserved bits | Reserved bits remain zero |
| **Address Decoder** | Reserved Address | Read/write invalid addresses | Return default value / ignore writes |
| **Pulse Counter** | Single Pulse | Generate one pulse | Counter increments by one |
| | Multiple Pulses | Generate multiple pulses | Counter value matches expected count |
| | Overflow Detection | Count beyond maximum value | Overflow flag asserted |
| | Overflow Hold | Continue simulation after overflow | Overflow flag remains asserted |
| | Overflow Clear | Clear overflow flag using W1C | Overflow flag clears correctly |

---

# Directed Test Sequences

## 1. Register Reset Test

Purpose

- Verify that all registers initialize correctly after reset.

Checks

- Control Register reset value
- Status Register reset value

Expected Result

- All registers return to their default values.

---

## 2. Control Register Verification

Purpose

Verify control register functionality.

Checks

- Read operation
- Write operation
- Reserved-bit protection

Expected Result

- Writable fields update correctly.
- Reserved bits remain unchanged.

---

## 3. Status Register Verification

Purpose

Verify status register behavior.

Checks

- Read-only protection
- Reset value
- Reserved-bit protection

Expected Result

- Writes are ignored.
- Register contents remain valid.

---

## 4. Reserved Address Verification

Purpose

Verify invalid address accesses.

Checks

- Read invalid addresses
- Write invalid addresses

Expected Result

- Reads return default value.
- Writes do not modify the DUT.

---

## 5. Pulse Counter Verification

Purpose

Verify pulse counting functionality.

Checks

- Single pulse
- Multiple pulses
- Counter increment

Expected Result

- Counter accurately tracks the number of input pulses.

---

## 6. Overflow Verification

Purpose

Verify overflow detection.

Checks

- Generate pulses until overflow occurs.
- Verify overflow flag assertion.

Expected Result

- Overflow flag asserted after counter wraps.

---

## 7. Overflow Hold Verification

Purpose

Verify overflow flag persistence.

Checks

- Continue simulation after overflow.

Expected Result

- Overflow flag remains asserted until cleared.

---

## 8. Write-One-to-Clear Verification

Purpose

Verify W1C behavior.

Checks

- Write a '1' to the overflow flag.

Expected Result

- Overflow flag clears.
- Counter operation resumes normally.

---

# Pass Criteria

The design is considered functionally verified when:

- All directed test cases pass.
- Register behavior matches the specification.
- Reserved bits remain protected.
- Invalid addresses are handled correctly.
- Pulse counting is accurate.
- Overflow detection behaves correctly.
- Overflow flag clears using Write-One-to-Clear.
- The self-checking testbench reports zero functional errors.

---

# Future Verification Improvements

Potential enhancements include:

- Migration to a UVM-based verification environment
- Constrained-random stimulus generation
- Functional coverage collection
- Assertion-Based Verification (SystemVerilog Assertions)
- Scoreboard-based reference model
- Automated regression testing using Makefiles and Python scripts