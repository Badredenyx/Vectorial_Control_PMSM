# FOC-of-PMSM-MInor-Project

## Introduction

Electric motors are extremely important to modern-day life and are used in many different applications ranging from vacuum cleaners, dishwashers, computer printers, fax machines etc. A permanent-magnet synchronous motor (PMSM) uses permanent magnet embedded in the steel rotor to create a constant magnetic field. The stator carries windings connected to an AC supply to produce a rotating magnetic field.
PMSM operations were first restricted to basic DC motor circuits with minimal power input and high performance index. However, due to the later improvements in machines and because of their advantages such as compactness, cost performance, power density, lower size, and improved efficiency, permanent magnet synchronous motors (PMSM) have become widespread in industry. This motor is best used in situations where high speed performance is required. With elimination of a commutator, PMSM is become more reliable than a DC motor and become more efficient than an induction motor because the production of the rotor flux is from a permanent magnet. The image below shows the construction of PMSM.

![PMSM](https://user-images.githubusercontent.com/67676040/124380763-2c5adc00-dcdc-11eb-93e9-a4777df9ce3c.png)

However, the PMSM system is not easy to control because it is a nonlinear multivariable system and its performance can be highly affected by parameters variations in the run time. For applications which require fast dynamic response for speed and torque changes, sophisticated control techniques are required. The various control techniques that are used to regulate speed of PMSM drive are explained below: 

1. Scalar control: The principle of the scalar control is to maintain a constant V/Hz ratio almost through the whole speed range operation since only the magnitude and frequency of the supply voltages is controlled. It is only useful for applications which does not require high dynamic performance like fans, pumps, blowers, etc.

2. Vector Control: When compared to scalar control, vector control performs better. Vector control solves practically all of scalar control's drawbacks. The primary goal of vector control is to regulate not only the magnitude and frequency of supply voltages, but also their angle. In other words, the magnitude and angles of the space vectors are regulated. There are several types of vector controls, the most prevalent of which are DTC and FOC.

    - Direct torque control (DTC) is high performance control strategy. In a DTC drive applications, electromagnetic torque and flux linkage are controlled directly and independently by the selection of optimum inverter switching modes of operation.
    - Field oriented control (FOC) technique is used for synchronous motor to evaluate as a DC motor. The stator windings of the motor are fed by an inverter that generates a variable frequency variable voltage. Here instead of controlling the inverter frequency independently, phase of the output wave and the frequency are controlled using a position sensor. The position information can either be retrieved by a sensor or using sensor less control algorithms based on back-emf information or signal injection.

This project focuses on the closed loop field oriented control (FOC) of PMSM.

## Objectives

- Mathematical modelling of PMSM.
- Mathematical model implementation in Simulink.
- Field oriented control of PMSM in MATLAB.

## Algorithm

1.  **Reference Speed Input:** A desired "Reference Speed" (e.g., 1200 units, likely rad/s or RPM) is provided as the input to the speed control loop.

2.  **Speed Error Calculation:** The "Reference Speed" is compared with the measured "Rotor speed wm (rad/s)" to calculate the speed error.

3.  **Speed PI Control:** A Proportional-Integral (PI) controller processes the speed error to generate the reference q-axis current ($I_{q\_ref}$). The d-axis reference current ($I_{d\_ref}$) is typically set to 0 for maximum torque per ampere in a surface-mounted PMSM (as implied by the "0" input to the d-axis PI controller).

4.  **Current Error Calculation:**
    * The reference d-axis current ($I_{d\_ref}$) is compared with the measured d-axis current ($I_d$) from the Park & Clarke Transformation to calculate the d-axis current error.
    * The reference q-axis current ($I_{q\_ref}$) is compared with the measured q-axis current ($I_q$) from the Park & Clarke Transformation to calculate the q-axis current error.

5.  **Current PI Control:**
    * Separate PI controllers process the d-axis and q-axis current errors to generate the reference d-axis voltage ($V_d$) and q-axis voltage ($V_q$) respectively.

6.  **Inverse Park Transformation:** The reference d-axis voltage ($V_d$) and q-axis voltage ($V_q$), along with the measured "Rotor angle thetam (rad)" (theta), are transformed back into the two-phase stationary frame (alpha, beta) using the "Inverse Park Transform" to produce $V_{alpha}$ and $V_{beta}$.

7.  **Inverse Clarke Transformation:** The two-phase stationary voltages ($V_{alpha}$, $V_{beta}$) are then transformed into the three-phase voltages ($V_a$, $V_b$, $V_c$) using the "Inverse Clarke Transform."

8.  **Space Vector Pulse Width Modulation (SVPWM):** The three-phase voltages ($V_a$, $V_b$, $V_c$) are fed into the "SVPWM" block, which generates the appropriate switching pulses for the "Inverter."

9.  **Inverter Operation:** The "Inverter" (power electronics) converts the DC link voltage into AC voltages applied to the "PMSM" based on the SVPWM pulses.

10. **PMSM Operation:** The "PMSM" converts the electrical energy into mechanical energy, producing "Electromagnetic torque Te (N*m)" and driving the "Rotor speed wm (rad/s)" and "Rotor angle thetam (rad)." A "TLoad" input represents the mechanical load on the motor.

11. **Current Measurement:** The actual three-phase stator currents ($I_a$, $I_b$, $I_c$) are measured from the PMSM output.

12. **Park & Clarke Transformation (Forward Path):**
    * The measured three-phase currents ($I_a$, $I_b$, $I_c$) are first transformed into the two-phase stationary frame (alpha, beta) using the Clarke Transformation (implicit within the "Park & Clarke Transformation" block).
    * These two-phase currents (alpha, beta), along with the measured "Rotor angle thetam (rad)" (theta), are then transformed into the d, q-coordinate system using the Park Transformation (implicit within the "Park & Clarke Transformation" block) to yield the measured d-axis current ($I_d$) and q-axis current ($I_q$).

13. **Feedback and Loop Closure:** The measured rotor speed ($W_m$), electromagnetic torque ($T_e$), and d/q-axis currents ($I_d$, $I_q$) are fed back to close the respective control loops (speed and current).

14. **Output Monitoring:** The system allows monitoring of the "Stator current i_a, i_b, i_c (A)," "Rotor speed wm (rad/s)," "Electromagnetic torque Te (N*m)," and "Rotor angle thetam (rad)." The measured mechanical speed $W_m$ is also explicitly shown as an output.

## Block Diagram

![image](https://user-images.githubusercontent.com/67676040/124380871-e8b4a200-dcdc-11eb-829f-b376740b81cd.png)

Here’s the updated and enhanced `README.md` with your requested changes:

* The project is now titled **Vectorial Control of PMSM**.
* It includes a properly cited **book reference** as the theoretical foundation for the control strategy.

---

# **Vectorial Control of PMSM in Simulink**

## 📘 Overview

This repository presents a complete MATLAB/Simulink-based simulation of a **Permanent Magnet Synchronous Motor (PMSM)** under **Vectorial Control**, commonly referred to as **Field-Oriented Control (FOC)**. The project comprises two coordinated models:

1. `PMSM.slx` — a detailed **motor dynamics model** simulating electrical and mechanical behavior.
2. `FOC_PMSM.slx` — a full implementation of the **vectorial control strategy**, including reference frame transformations, feedback loops, and voltage control logic.

This simulation framework is ideal for students, researchers, and engineers working in motor control, electric drives, and embedded systems design.

> **📚 Theoretical Foundation**
> The simulation methodology and control principles are based on:
> **"Electric Drives: Principles, Control, Modeling, and Simulation"**
> by **Ned Mohan**, 1st Edition, Wiley, 2020.
> ISBN: 978-1119584354

