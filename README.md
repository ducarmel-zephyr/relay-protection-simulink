# relay-protection-simulink

## Recommended repository name

`relay-protection-simulink`

Alternative names:

* `ansi-50-51-relay-model`
* `overcurrent-relay-simulink`
* `power-system-relay-protection`

---

## Folder structure

```text
relay-protection-simulink/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── model/
│   └── ANSI_50_51_Relay_Model_Cleanup_Presentation_Polish.slx
│
├── images/
│   ├── inverse-time-characteristic-curve.png
│   ├── timer-accumulation-plot.png
│   ├── trip-logic-plot.png
│   └── system-block-diagram.png
│
├── results/
│   ├── sample-fault-case-summary.pdf
│   ├── trip-time-comparison.csv
│   └── notes.md
│
├── docs/
│   ├── project-summary.md
│   ├── technical-method.md
│   └── poster-or-presentation.pdf
│
└── references/
    └── ieee-c37-112-notes.md
```

---

## README.md

```markdown
# Control & Protection Systems – ANSI 50/51 Relay Modeling Using MATLAB/Simulink

## Overview
This project presents a MATLAB/Simulink model of a power system protection scheme using ANSI 50/51 overcurrent relay logic. The objective is to study how a relay detects abnormal current conditions and issues a trip signal to protect the system under fault conditions.

The model includes a three-phase source, transmission line impedance, load, circuit breaker, measurement blocks, current scaling, and relay decision logic. It demonstrates both instantaneous overcurrent protection (ANSI 50) and inverse-time overcurrent protection (ANSI 51).

This work was developed as part of an undergraduate research project focused on control and protection systems.

## Engineering Problem
In real power systems, faults can produce dangerous levels of current that may damage equipment if they are not cleared quickly. Protection relays must distinguish between normal and abnormal operating conditions and respond with the correct timing.

This project models that protection behavior in simulation and compares inverse-time relay response to expected IEEE-style operating trends.

## Objectives
- Build a simplified power system model in MATLAB/Simulink
- Measure system current under normal and fault conditions
- Implement ANSI 50 instantaneous overcurrent logic
- Implement ANSI 51 inverse-time overcurrent logic
- Observe how relay operating time changes with fault severity
- Study the relationship between multiple of pickup and trip time

## System Description
The simulated system contains the following major parts:
- **Three-phase source** to represent the power supply
- **Transmission line impedance** to represent the feeder or protected line section
- **Load** to represent downstream demand
- **Circuit breaker** to isolate the faulted section after relay trip
- **Three-phase fault block** to apply abnormal operating conditions
- **Three-phase V-I measurement** to monitor current and voltage
- **RMS measurement** to obtain a stable current magnitude for relay logic
- **Current transformer scaling** to convert measured current to the relay operating basis
- **Relay subsystem** implementing ANSI 50/51 decision logic

## Why RMS Current Was Used
Because the source and fault current waveforms are sinusoidal and can contain transients, RMS current was used to provide a stable value for relay comparison. This makes the protection logic easier to analyze and aligns the measured quantity with relay pickup settings.

## Relay Logic
### ANSI 50 – Instantaneous Overcurrent
The ANSI 50 element trips immediately when the measured current exceeds a high pickup threshold. This is used for severe faults that require very fast isolation.

### ANSI 51 – Inverse-Time Overcurrent
The ANSI 51 element operates when current exceeds a lower pickup level and remains above that threshold long enough for the operating timer to reach the required value. The higher the fault current, the shorter the operating time.

## Key Parameters
- **I50_pickup**: pickup threshold for instantaneous element
- **I51_pickup**: pickup threshold for inverse-time element
- **M**: multiple of pickup, defined as measured current divided by pickup current
- **t_operate**: calculated relay operating time
- **TimeReached**: logic signal indicating that the inverse-time operating condition has been satisfied
- **curve_selector**: setting used to select the inverse-time curve behavior

## Method Summary
1. Build the electrical system model.
2. Apply a fault at a selected location and time.
3. Measure system current using the three-phase measurement block.
4. Convert the measured signal to RMS current.
5. Scale the current to the relay operating basis using CT logic.
6. Compare the current against ANSI 50 and ANSI 51 pickup values.
7. For ANSI 50, send an immediate trip if the current exceeds the high threshold.
8. For ANSI 51, compute the operating time based on fault severity and allow the timer to accumulate.
9. Issue a trip command to the breaker when relay conditions are satisfied.
10. Observe current behavior, timer accumulation, and trip response.

## Results
This project highlights three key engineering results:

### 1. Inverse-Time Characteristic
The inverse-time characteristic curve shows that as the multiple of pickup (**M**) increases, relay operating time decreases. This reflects the expected protection behavior: more severe faults should be cleared faster.

### 2. Timer Accumulation Behavior
The timer accumulation plot shows how the inverse-time element integrates operating time only when current remains above pickup. This helps explain why moderate overcurrent does not trip instantly.

### 3. Relay Trip Logic Response
The trip logic plot shows the final protection action. Once the relay operating condition is satisfied, the breaker receives a trip signal and the fault current is cleared.

## Representative Figures
Add your best visuals here:
- Inverse-time characteristic curve
- Timer accumulation plot
- Relay trip logic plot
- Optional overall system block diagram

## Technical Significance
This project demonstrates the core principles of protective relaying in a simple but meaningful way. It shows how relay settings, current magnitude, timing logic, and breaker action work together to protect electrical infrastructure.

It also provides a foundation for future work in:
- relay coordination
- distance protection
- differential protection
- real-time simulation
- hardware-in-the-loop testing

## Tools Used
- MATLAB
- Simulink
- Simscape Electrical Specialized Power Systems

## Skills Demonstrated
- Power system modeling
- Fault analysis
- Protective relaying fundamentals
- ANSI 50/51 overcurrent logic
- RMS signal processing
- Simulation-based engineering analysis
- Technical documentation

## Future Improvements
- Add multiple relay curves and compare results
- Include coordination between upstream and downstream relays
- Extend the model to directional or distance protection
- Port the project toward real-time simulation or HIL testing

## About Me
I am an Electrical Engineering student focused on power systems, protection and control, and intelligent infrastructure. This repository is part of my engineering portfolio and reflects my interest in building resilient and smart power system solutions.

## Contact
- LinkedIn: [Add your LinkedIn URL here]
- Resume / portfolio: [Add link if available]
```

---

## project-summary.md

```markdown
# Project Summary

This project models the operation of an ANSI 50/51 overcurrent relay in MATLAB/Simulink. The system includes a three-phase source, line impedance, load, breaker, and configurable fault block. The relay monitors the measured RMS current, compares it against pickup thresholds, and issues a trip command based on either instantaneous or inverse-time operating logic.

The purpose of the work is to demonstrate how protection systems respond to abnormal current conditions and how relay operating time changes with fault severity. The model also supports discussion of current transformer scaling, pickup setting selection, and inverse-time characteristics.
```

---

## technical-method.md

```markdown
# Technical Method

## Model Workflow
1. Generate the three-phase power signal.
2. Pass the current through the protected network.
3. Apply a fault to create an abnormal condition.
4. Measure the resulting three-phase current.
5. Convert the measured current to RMS form.
6. Scale the RMS value to the relay operating basis.
7. Compare the current with the ANSI 50 and ANSI 51 pickup thresholds.
8. Compute relay action based on fault magnitude and timing.
9. Send a trip signal to the breaker.
10. Observe fault clearing and resulting system response.

## Important Modeling Choices
- RMS current was used to provide a stable decision signal.
- Current scaling was used to match relay thresholds to the selected operating basis.
- Separate logic paths were used for instantaneous and inverse-time operation.
- The inverse-time function was used to capture realistic relay timing behavior.
```

---

## .gitignore

```gitignore
# MATLAB autosave and temp files
*.asv
*.autosave
*.slxc
*.mdl.r*
*.mexa64
*.mexw64
*.mexmaci64

# OS files
.DS_Store
Thumbs.db

# Logs
*.log
```

---

