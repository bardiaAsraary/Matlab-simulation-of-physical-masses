# 📘 Soft Sensing & Robotics — Two-Mass Spring-Damper System

## 🧩 Overview

This project models, simulates, and identifies a coupled two-mass spring–damper system.
A complete workflow is implemented:

Physical modeling from differential equations

Simulink simulation

System identification via TF, ARX, and FIR/OE models

Validation using step, sine, and chirp inputs

Frequency-domain analysis (Bode, PZ maps)

Residual analysis

MATLAB code for reproducibility

The system is strongly overdamped, primarily due to the high damping coefficient on Mass 1, which makes the dynamics nearly decoupled.

## ⚙️ System Description

A force f(t) is applied to Mass 2, while Mass 1 is attached to ground with heavy damping.
The physical parameters are:

| Parameter | Symbol | Value | Unit | Description |
|----------|--------|--------|------|-------------|
| Mass 1 | M₁ | **1** | kg | Left mass connected to ground |
| Mass 2 | M₂ | **5** | kg | Right mass driven by external force |
| Damping (ground) | B₁ | **50** | Ns/m | Strong damping on Mass 1 |
| Damping (coupling) | B₂ | **1** | Ns/m | Viscous coupling between the masses |
| Spring constant | K | **100** | N/m | Spring connecting the two masses |
```
Governing Equations
M1​x¨+(B1​+B2​)x˙+Kx=B2​y˙​+Ky
M2​y¨​+B2​y˙​+Ky=B2​x˙+Kx+f(t)
```
These equations are implemented in Simulink.

## 🛠️ Task 1 — Simulink Simulation
✔ Objective

Simulate the mechanical system under different inputs (step, sinusoidal, chirp) to analyze displacement responses.

### ✔ Architecture

The Simulink model includes:

Force balance blocks

Parameter gain blocks

Integrators for velocity & position

Two subsystems representing Mass 1 and Mass 2

### ✔ Inputs Simulated

Step: 10 N

Sine: 1 Hz and 5 Hz

Chirp: 0.1 → 10 Hz over 50 s

### ✔ Key Observation

Due to heavy damping (B₁ = 50), the system behaves almost like a single-mass system (M₂). Responses show no oscillation, confirming overdamped behavior.

## 📉 Task 2 — Model Approximation & Step Response
### ✔ Transfer Function (TF) Estimation

A 2nd-order TF was identified:

𝐺
(
𝑠
)
=
0.02
𝑠
2
+
0.52
𝑠
+
100
𝑠
2
+
0.2
𝑠
+
20
G(s)=
s
2
+0.2s+20
0.02s
2
+0.52s+100
	​


However, it incorrectly predicts the DC gain, which should be:

1
𝐾
=
0.01
K
1
	​

=0.01

A corrected version scales the TF accordingly.

### ✔ Step Response Metrics (True System)
Metric	Value	Notes
Rise Time	3.2 s	10–90%
Overshoot	0%	Overdamped
Settling Time	25 s	±2%
Steady-State Gain	0.01	Matches physical expectation
### ✔ Validation

TF model: 92% fit, but wrong DC gain

ARX/FIR: High accuracy across transient & steady-state

ARX model performs best overall

## 🔍 Task 3 — Data-Driven Identification (ARX & FIR)
### ✔ Models

ARX: arx(data, [2 2 1])

FIR/OE: oe(data, [0 3 1])

### ✔ Performance Comparison
Model	Fit (%)	Notes
ARX	94.3%	Best match
FIR/OE	89.6%	Needs more taps
TF	92%	Gain error
### ✔ Frequency-Domain Validation

ARX best matches true dynamics up to 1–2 Hz

TF and FIR diverge at higher frequencies

Bode and PZ plots confirm overdamping and low-pass behavior

### ✔ Residual Analysis

ARX residuals are white → excellent model

TF & FIR residuals show correlation (missing dynamics)

## 🧪 MATLAB Code

Full MATLAB scripts for:

Data loading

Model identification

Step/sine/chirp simulation

Zoomed comparisons

Bode plots

Pole–zero maps

Residual analysis

DC-gain-corrected TF

All code is included in the /code section or in the report files.

## 🧾 Conclusion

This project successfully demonstrates:

Accurate modeling of an overdamped two-mass system

Reliable system identification using ARX

Strong validation across time & frequency domains

Importance of DC gain correction for TF models

ARX is the preferred model for this system due to its high fidelity and low model order.

Future improvements may include:

Grey-box modeling enforcing physical parameters

Identification under noisy measurements

Controller design based on the reduced models
