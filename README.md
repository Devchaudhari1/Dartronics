<div align="center">

# 🎯 Digital Dartboard Game

### A Hardware-Driven Digital Dart Game with PRBS-Based Targeting, Speed Control & Scoreboard

![Verilog](https://img.shields.io/badge/Verilog-HDL-8A2BE2?style=for-the-badge)
![Logisim](https://img.shields.io/badge/Logisim-Digital%20Logic-FF6B35?style=for-the-badge)
![Hardware](https://img.shields.io/badge/Hardware-Digital%20Circuits-0F9D58?style=for-the-badge)
![NITK](https://img.shields.io/badge/NITK-CSE-1F6FEB?style=for-the-badge)

**A digital implementation of a dart game combining sequential logic, pseudo-random pattern generation, LED-based target regions, player turn management, and score tracking.**

</div>

---

## 📌 Table of Contents

- [🎮 Overview](#-overview)
- [👥 Team](#-team)
- [💡 Motivation](#-motivation)
- [🎯 Problem Statement](#-problem-statement)
- [✨ Key Features](#-key-features)
- [🧠 System Architecture](#-system-architecture)
- [⚙️ How It Works](#-how-it-works)
- [🖥️ Logisim Circuit Design](#️-logisim-circuit-design)
- [💻 Verilog Implementation](#-verilog-implementation)
- [🔧 Hardware Implementation](#-hardware-implementation)
- [📂 Project Structure](#-project-structure)
- [📚 References](#-references)

---

## 🎮 Overview

The **Digital Dartboard Game** is a digital-circuit implementation of a classic dart game designed around **precision, timing, and dynamic target selection**.

The system uses a **time-varying pointer** that moves across four concentric target regions represented using LEDs. A throw captures the current target region and awards points according to the corresponding scoring logic.

### The project combines

- ⚡ Sequential digital logic
- 🔁 Finite State Machine concepts
- 🎲 Pseudo-Random Bit Sequence (PRBS) generation
- 💡 LED-based dartboard patterns
- 🎚️ Variable speed control
- 🧮 Multi-player score tracking
- 🏆 Winner / final-score determination
- 🖥️ Verilog simulation and hardware implementation

---

## 👥 Team

| Member | Roll Number | Email |
|:---|:---:|:---|
| **Dev Chaudhari** | 231CS221 | devchaudhari.231cs221@nitk.edu.in |
| **Himanshu Bande** | 231CS225 | himanshubande.231cs225@nitk.edu.in |
| **Aryan** | 231CS213 | aryan.231cs213@nitk.edu.in |

**Course:** B.Tech. CSE — 3rd Semester  
**Section:** S2  
**Team:** S2 T17

---

## 💡 Motivation

A dartboard game is not only an entertaining activity but also provides an engaging way to work with concepts such as **precision, timing, sequential control, and state management**.

The project aims to recreate the experience digitally using logic circuits. By combining a dynamically changing target, speed control, and a scoreboard, the game provides an adjustable level of challenge while demonstrating practical applications of digital systems concepts.

---

## 🎯 Problem Statement

The system is designed to:

- Accept input signals representing dart throws on a virtual dartboard.
- Provide multiple distinct target regions, with the bullseye representing the most challenging region.
- Introduce variations in target movement to increase difficulty.
- Maintain player scores without premature overflow during gameplay.
- Support gameplay for multiple players.
- Provide an intuitive and responsive digital gaming experience.

---

## ✨ Key Features

| Feature | Description |
|:---|:---|
| 🎯 **Dynamic Dartboard** | A time-varying pointer navigates across four concentric target regions. |
| 💡 **LED Indication** | LEDs indicate the current target position. |
| 🎲 **PRBS Generation** | A pseudo-random sequence generates dynamic dartboard patterns. |
| 🎚️ **Variable Speed** | Players can adjust the speed at which the pointer changes position. |
| 👥 **3-Player Support** | The game supports up to three players. |
| 🧮 **Scoreboard** | Player scores are tracked throughout the game. |
| ⏱️ **Time Penalty** | A penalty is imposed when the throw time limit is exceeded. |
| 🏆 **Winner Detection** | The final logic determines the winning player and winning score. |

---

## 🧠 System Architecture

```text
                         ┌──────────────────────┐
                         │     Clock / Reset    │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │   PRBS / LFSR Logic  │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Dartboard Target     │
                         │ / LED Pattern Logic  │
                         └──────────┬───────────┘
                                    │
                         ┌──────────▼───────────┐
                         │    Throw Button      │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │  Scoring & Turn      │
                         │     Management       │
                         └──────────┬───────────┘
                                    │
                       ┌────────────┴────────────┐
                       ▼                         ▼
              ┌─────────────────┐       ┌─────────────────┐
              │ Score Display   │       │ Winner / Final  │
              │                 │       │ Score Logic     │
              └─────────────────┘       └─────────────────┘
```

### Core Logic

1. The PRBS/LFSR logic generates a changing pseudo-random sequence.
2. The generated pattern determines the active dartboard region.
3. LEDs indicate the current region.
4. When the player presses the throw button, the current region is captured for scoring.
5. The corresponding points are added to the current player's score.
6. After the configured number of throws, control moves to the next player.
7. The final logic compares player scores and identifies the winner.

---

## ⚙️ How It Works

### 1. 🎯 Target Generation

The dartboard contains **four concentric target regions**. A moving pointer periodically changes its position, creating the timing challenge for the player.

### 2. 🎲 PRBS / LFSR

A Linear Feedback Shift Register is used to generate a pseudo-random sequence.

The Verilog implementation uses a 5-bit PRBS register:

```verilog
prbs <= {prbs[3:0], prbs[4] ^ prbs[2]};
```

The initial seed is:

```verilog
5'b10101
```

The lower PRBS bits are mapped to scoring values:

| PRBS Pattern | Points |
|:---:|:---:|
| `000` | **5** |
| `001` | **4** |
| `010` | **3** |
| `011` | **2** |
| `100` | **1** |
| Others | **0** |

### 3. 👥 Player Turns

The game maintains:

- Current player
- Player score
- Throw count
- Current PRBS state

The implementation supports **three players**, with the turn changing after five throws.

### 4. 🧮 Score Calculation

The current player's score is updated whenever the throw button is activated:

```verilog
player_score[player_turn] <=
    player_score[player_turn] + circle_points;
```

The current player's ID and score are then exposed through the output signals.

---

## 🖥️ Logisim Circuit Design

The project was developed and represented using modular digital logic circuits.

### 🔷 Overall Working / Modular Design

![Digital Dartboard Architecture](https://github.com/Devchaudhari1/S2-T17/blob/main/Digital%20dartboard%20game%20modularized.drawio.png)

### 🔷 Main Module

![Main Digital Dart Game](https://github.com/user-attachments/assets/16a7bc57-4218-4f0d-aa8a-b614f975afd8)

### 🔷 PRBS Flux Module

![PRBS Flux Module](https://github.com/user-attachments/assets/575946f7-9059-4f13-b150-0e8fa9f82b0a)

### 🔷 Final Score Comparator

![Final Score Comparator](https://github.com/user-attachments/assets/7a6e533e-e9ea-42f4-aa6e-1d4baf31d736)

### 🔷 Truth Table — Points Awarded Per Throw

![Truth Table](https://github.com/user-attachments/assets/e097b109-b863-4d5a-9b9c-c8e492875117)

### 🔷 State Equations — LED Concentric Circles

![State Equations](https://github.com/user-attachments/assets/e9f7804b-ed91-4b0f-a9e4-05b82f8c3b84)

![State Equation Footnote](https://github.com/user-attachments/assets/9e5105a2-dda6-4ca2-baa4-bbf8127eefd0)

---

## 💻 Verilog Implementation

### Main Module

The Verilog module defines the core game interface:

```verilog
module digital_dart_game (
    input clk,
    input reset,
    input throw_button,
    output [2:0] player_id,
    output [4:0] score_display,
    output [4:0] final_score,
    output [4:0] winner
);
```

### 🎲 PRBS Generation

```verilog
always @(posedge clk or posedge reset) begin
    if (reset)
        prbs <= 5'b10101;
    else
        prbs <= {prbs[3:0], prbs[4] ^ prbs[2]};
end
```

### 🎯 Point Assignment

```verilog
assign circle_points =
    (prbs[2:0] == 3'b000) ? 5 :
    (prbs[2:0] == 3'b001) ? 4 :
    (prbs[2:0] == 3'b010) ? 3 :
    (prbs[2:0] == 3'b011) ? 2 :
    (prbs[2:0] == 3'b100) ? 1 : 0;
```

### 👥 Player & Score Management

The design stores individual scores for three players:

```verilog
reg [4:0] player_score[0:2];
reg [2:0] player_turn;
reg [2:0] throw_count;
```

The game changes the active player after five throws.

### 🧪 Testbench

The repository also contains a Verilog testbench that:

- Generates the clock.
- Applies reset.
- Simulates throws for all three players.
- Monitors player ID.
- Monitors player score.
- Monitors winning score.
- Monitors winner output.

Example monitoring logic:

```verilog
$monitor(
    "Time: %0t | Player ID: %0d | Player Score: %0d | Winning Score: %0d | Winner: %0d",
    $time,
    player_id,
    score_display,
    winning_score,
    winner
);
```

---

## 🔧 Hardware Implementation

The project extends the digital design into physical hardware using basic digital logic ICs.

### 🎲 PRBS Generator for Dartboard Patterns

The hardware PRBS generator uses:

- **7474 D-type flip-flops**
- **7486 XOR gates**

The implementation follows an **LFSR-based configuration**, where the flip-flops store and shift the binary state while XOR gates provide feedback.

The resulting pseudo-random sequence controls the LED-based dartboard patterns.

The hardware implementation uses a **15-bit sequence** for the dartboard pattern generation and initializes the PRBS with a seed value of **1**, using the 3rd and 4th bits for initialization.

### ➕ 5-Bit BCD Address Logic

The project uses **7483 4-bit binary full adder ICs** to implement the address logic.

The design uses three 7483 ICs for the 5-bit address logic, with carry propagation between stages.

This enables address generation across the **0–31 decimal range** for controlling the dartboard patterns.

### 📸 Hardware Snapshots

#### PRBS Module

![PRBS Hardware](https://github.com/Devchaudhari1/S2-T17/blob/main/Snapshots/PRBS%20Module(hardware).png)

#### 5-Bit BCD Adder

![5 Bit BCD Adder](https://github.com/Devchaudhari1/S2-T17/blob/main/Snapshots/5%20bit%20bcd%20adder(hardware).png)

#### Achievement Unlocked Module

![Achievement Unlocked](https://github.com/Devchaudhari1/S2-T17/blob/main/Snapshots/Achievement%20Unlocked%20Module(hardware).png)

---

## 📂 Project Structure

```text
S2-T17/
│
├── Logisim/
│   └── Digital circuit designs
│
├── Snapshots/
│   ├── PRBS Module (hardware)
│   ├── 5 bit BCD adder (hardware)
│   └── Achievement Unlocked Module (hardware)
│
├── Verilog/
│   ├── Main module
│   └── Testbench
│
├── Digital dartboard game modularized.drawio.png
│
└── README.md
```

---

## 📚 References

1. **Digital anti-windup PI controllers for variable-speed motor drives using FPGA and stochastic theory**  
   Zhang, Dai; Li, Hui; Collins, Emmanuel G.  
   *IEEE Transactions on Power Electronics*, Volume 21, Issue 5, Pages 1496–1501, 2006.  
   [IEEE Xplore](https://ieeexplore.ieee.org/document/1640711)

2. **Real-time digital hardware simulation of power electronics and drives**  
   Parma, Gustavo G.; Dinavahi, Venkata.  
   *IEEE Transactions on Power Delivery*, Volume 22, Issue 2, Pages 1235–1246, 2007.  
   [IEEE Xplore](https://ieeexplore.ieee.org/document/4130508)

---

<div align="center">

### 🎯 Built with Digital Logic • Verilog • Logisim • Hardware

**S2 T17 · B.Tech. CSE · NITK**

</div>
