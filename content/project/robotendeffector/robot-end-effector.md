---
layout: project
title: Design and Rapid Prototyping of a Robotic End-Effector
date: 2025-12-31
description: A custom, lightweight robot End-Effector designed and optimized in SolidWorks, built using FDM 3D printing.
summary:  A custom, lightweight robot End-Effector designed and optimized in SolidWorks, built using FDM 3D printing.
hidemeta: true


weight: 201

cover:
  image: "Coverimages/end-effector-hero.JPG"



tags: [Robotics, SolidWorks, FEA, DfMA, 3D Printing]
---

## Project Overview

**Tools:** SolidWorks (CAD & Simulation), Bambu Studio, FDM (PLA)

I designed and fabricated a custom, lightweight robot End-Effector optimized for Fused Deposition Modeling (FDM). The project focused on **Design for Assembly (DfMA)** principles to minimize part count, eliminate post-processing, and maximize the payload capacity of a constrained robotic arm.

---

## 1. The Challenge & Design Goals

The objective was to create a functional manipulator compatible with an **MG90S Micro Servo** while strictly adhering to low-weight and low-cost constraints.

* **Weight Limit:** Must remain under 25g to preserve robot arm torque.
* **Fabrication:** Must be printable without the need for post-process sanding or drilling.
* **Actuation:** Convert rotational servo input into linear jaw motion.

---

## 2. Mechanical Design (CAD)

I streamlined the assembly into four unique printable components: a central Servo Mount, Left/Right Jaws, and connecting Linkages.

* **Kinematics:** Engineered a direct linkage system to translate servo rotation into linear clamping force.
* **DfMA Features:** Integrated **2.1 mm radiused lead-ins** on mounting slots to self-align fasteners, reducing assembly difficulty.
* **Tolerancing:** Applied a uniform **0.2 mm clearance gap** on all moving parts to compensate for material shrinkage and prevent joint fusion.

*Four CAD Models*
![](/projects/roboEF_images/End-effector_Parts.png)

*Robot End-effector Kinematics Animation*
![](/projects/roboEF_images/Kinematics.gif)

---

## 3. Engineering Validation (FEA)

Before fabrication, I validated the geometry using SolidWorks Simulation.

* **Scenario:** Simulated a "stalled grip" condition with a **500 N load** applied to the jaw tips.
* **Boundary Conditions:** Fixed geometry at the mount base with pin connectors at linkage joints.
* **Analysis:** Results showed peak Von Mises stress of **~50 MPa** at linkage pivots.
* **Design Iteration:** Based on these results, I modified the print parameters to increase **Wall Loops** in high-stress areas rather than using 100% infill, optimizing the strength-to-weight ratio.



![](/projects/roboEF_images/FEA_Stress.JPG)

*Finite Element Analysis highlighting stress concentrations at the pivots.*

---

## 4. Manufacturing & Accuracy Analysis

Parts were fabricated on a **Bambu Lab A1 Mini** using PLA with a **15% Grid Infill** to reduce mass.

*Printed Parts*
![Printed Parts](/projects/roboEF_images/printedParts.JPG)

I performed metrology on critical features to verify the printer's dimensional accuracy. The parts achieved a maximum deviation of **0.03 mm** from the CAD model. Vertical holes (designed at 3.00 mm) printed at 2.97 mm, validating the necessity of the pre-engineered clearance gaps.

*Comparison of CAD dimensions vs. physical printed parts.*
![Dimensional Accuracy Table](/projects/roboEF_images/Dimensional_Accuracy_Table.png)


---

## 5. Servo Motor Control (Arduino)

To validate the gripper motion, I used an Arduino sketch to drive the **MG90S micro servo** through a simple open-loop sweep.

```cpp
#include <Servo.h>

Servo myServo;  // Create servo object

void setup() {
  myServo.attach(9);  // Attach servo signal to pin 9
}

void loop() {
  myServo.write(0);    // Move servo to 0 degrees
  delay(3000);         // Wait 3 seconds
  myServo.write(90);   // Move servo to 90 degrees
  delay(3000);
  myServo.write(0);    // Return to 0 degrees
  delay(3000);
}
```

**Notes:**
- **Pin 9** is the PWM output signal.
- Power the servo from a stable **5 V** supply (don’t rely on the Arduino 5 V pin if the servo stalls).

---

## 6. Final Results

The final prototype met all initial performance metrics:

* **Mass:** **20.11 g** (Success: <25g goal met).
* **Cost:** **<$0.50 USD** in materials.
* **Efficiency:** The entire mechanism was assembled in **under 5 minutes** with **zero post-processing** (no sanding/filing) required.


![](/projects/roboEF_images/End-effector-Animation.gif)

---