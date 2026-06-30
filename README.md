# ⭐ Star-Delta Motor Starter

A PLC ladder logic simulation of an automatic Star-Delta motor starter built using **CodeSys**. The system starts the motor in Star configuration for reduced starting current, then automatically transitions to Delta configuration for full running torque after a timed delay.

---

## 📹 Demo Video

🎥 [Watch Simulation Video on YouTube](#) ← *Replace this with your YouTube link*

---

## 📌 Project Overview

| Field | Details |
|---|---|
| **Platform** | CodeSys (Simulation) |
| **Language** | Ladder Diagram (LD) |
| **Function** | Automatic Star-Delta motor starting sequence |
| **Star Run Time** | 5 seconds (TON_0, T#5S) |
| **Transition Delay** | 0.5 seconds (TON_1, T#0.5S) — ensures Star contactor fully opens before Delta closes |

---

## 🔌 I/O Table

### Inputs

| Variable | Type | Description |
|---|---|---|
| `START` | BOOL | Start button — energizes Main contactor |
| `STOP` | BOOL | Stop button — de-energizes the system |

### Outputs

| Variable | Type | Description |
|---|---|---|
| `MAIN_CONTACT` | BOOL | Main line contactor — connects motor to supply |
| `STAR_CONTACT` | BOOL | Star contactor — connects motor windings in Star |
| `DELTA_CONTACT` | BOOL | Delta contactor — connects motor windings in Delta |
| `RELEASE_STAR` | BOOL | Internal flag — signals Star time has elapsed |

### Timers

| Block | Type | Setting | Description |
|---|---|---|---|
| `TON_0` | TON | `T#5S` | Star run timer — motor runs in Star for 5 seconds |
| `TON_1` | TON | `T#0.5S` | Transition delay timer — 0.5 second gap before Delta engages |
| `elapsed_time` | TIME | — | Tracks elapsed time for each timer |

---

## 🔄 System Sequence

1. START pressed → MAIN_CONTACT energizes and seals itself in
2. With MAIN_CONTACT energized, STAR_CONTACT immediately engages — motor starts in Star configuration (reduced starting current)
3. TON_0 begins timing — runs for 5 seconds while motor is in Star
4. After 5 seconds → RELEASE_STAR activates → STAR_CONTACT de-energizes
5. TON_1 begins timing a short 0.5 second transition delay — this prevents Star and Delta contactors from ever being closed at the same time
6. After 0.5 seconds → DELTA_CONTACT engages — motor now runs in Delta configuration (full running torque)
7. STOP pressed at any time → MAIN_CONTACT de-energizes → entire sequence resets

---

## 🧩 Ladder Logic — Rung Breakdown

| Rung | Description |
|---|---|
| 1 | START with seal-in via MAIN_CONTACT energizes MAIN_CONTACT. STOP breaks the circuit |
| 2 | MAIN_CONTACT (with DELTA_CONTACT interlock) energizes STAR_CONTACT and starts TON_0. After 5s, TON_0 sets RELEASE_STAR |
| 3 | MAIN_CONTACT and RELEASE_STAR start TON_1. After 0.5s delay, with STAR_CONTACT confirmed open, DELTA_CONTACT energizes |

### Why the 0.5 Second Delay Matters
This is a key safety feature in real Star-Delta starters — the brief delay between releasing the Star contactor and engaging the Delta contactor prevents a short circuit condition where both contactors could be closed simultaneously, which would short the motor windings.

---

## 🖥️ Visualization

A simple HMI was built in CodeSys with **start** and **stop** push buttons and three status indicator lamps to monitor the Main, Star, and Delta contactor states.

---

## 📁 Repository Structure

```
star-delta-starter-plc/
├── media/
│   ├── ladder_logic.pdf
│   ├── plc_program_screenshot.png
│   └── visualization_screenshot.png
├── README.md
```

---

## 🛠️ Tools Used

- **CodeSys 3.5** — PLC programming and simulation
- **Ladder Diagram (LD)** — Programming language
- **Soft PLC simulation** — No physical hardware required

---

## 👤 Author

**S Navaneeth Krishna**
B.Tech — Electrical and Electronics Engineering
Muthoot Institute of Science & Technology, Kerala

[![GitHub](https://img.shields.io/badge/GitHub-navaneeth--linh-181717?style=flat&logo=github)](https://github.com/navaneeth-linh)

---

## 📜 License

This project is open source and available for educational use.
