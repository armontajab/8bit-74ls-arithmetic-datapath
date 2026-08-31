# 8-Bit Arithmetic Datapath in 74-Series Logic

A complete 8-bit datapath built entirely from discrete 74LS TTL devices and
simulated in Proteus. No microcontroller, no HDL - arithmetic, logic, data
selection, decoding and storage wired together from individual chips.

---

## Bill of materials

| Qty | Part | Function |
|---|---|---|
| 2 | 74LS283 | 4-bit binary full adder with fast carry look-ahead |
| 2 | 74LS86 | Quad 2-input XOR |
| 2 | 74LS157 | Quad 2:1 multiplexer |
| 1 | 74LS139 | Dual 2-to-4 decoder / demultiplexer |
| 2 | 74LS194 | 4-bit bidirectional universal shift register |
| 3 | - | Toggle switches for control lines |

---

## Signal path

```
A, B operands
     |
     v
 74LS283 x2  --->  8-bit sum (carry look-ahead, cascaded)
     |
 74LS86 x2   --->  bitwise comparison / parity
     |
     v
 74LS157 x2  --->  select: raw operand or sum
     |
     v
 74LS139     --->  route to one of four output channels
     |
     v
 74LS194 x2  --->  store, shift left, shift right, or hold
```

### Arithmetic - 74LS283

Two 4-bit adders cascaded into an 8-bit adder, the carry-out of the low stage
feeding the carry-in of the high stage. Carry look-ahead computes each carry
from generate and propagate terms instead of waiting for the previous stage:

```
Sum:       Si   = Ai XOR Bi XOR Ci
Generate:  Gi   = Ai . Bi
Propagate: Pi   = Ai XOR Bi
Carry:     Ci+1 = Gi + (Pi . Ci)
```

### Logic - 74LS86

XOR outputs high wherever two input bits differ, giving both bitwise comparison
and parity generation.

### Selection - 74LS157

A single select line flips the whole word between two sources:

| Select | Output |
|---|---|
| 0 | Operand A |
| 1 | Sum A + B |

### Routing - 74LS139

A two-bit address pulls exactly one of four outputs **low** (outputs are
active-low), enabling the channel that receives the result.

### Storage - 74LS194

| S1 | S0 | Mode |
|---|---|---|
| 1 | 1 | Parallel load |
| 0 | 1 | Shift left |
| 1 | 0 | Shift right |
| 0 | 0 | Hold |

A value is loaded in one clock and then walked left or right on subsequent
pulses. Mode changes are made while the clock is high, as the datasheet
recommends.

---

## Running the simulation

Open `proteus/digital project.pdsprj` in Proteus 8. Set the operands with the
logic toggles, choose a mode with the control switches, and clock the shift
register with the pulse generator. Logic probes on the outputs show the result.

Suggested first test: load a known value in parallel, then switch to shift mode
and clock it, to confirm bit order and direction before trying anything else.

---

## Repository contents

```
proteus/      Proteus 8 simulation project
docs/         Schematic capture screenshots
```

---

## Context

Final project for **Digital Systems I**, Shahid Beheshti University,
Spring 2025. Supervisor: Dr. Farhad Pouladi. Individual project.
