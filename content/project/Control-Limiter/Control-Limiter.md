---
layout: project
title: Control Limiter for Cast Saw Safety
date: 2026-02-12
description: An Arduino based control limiter is a blade distance limiting apparatus for the Stryker 940 cast saw to prevent injuries during the cast removal procedure.
summary: A safety add on for a cast saw. I wrote an Arduino C++ program that reads a voltage change when the blade approaches a conductive layer inside the cast, then triggers LED and buzzer alerts and logs contact events.
hidemeta: true

weight: 202

cover:
  image: "Coverimages/control-limiter-hero.png"
  alt: "Control Limiter "
  relative: true

tags: [Arduino, C++, Embedded Systems, Sensors, Safety, Rapid Prototyping, Product Design]
---

## Project Overview

This **Control Limiter** project reduces cast saw related injuries during cast removal. Overcutting can harm patients and create risk for clinicians and hospitals.

I built an Arduino based device that detects when the cast saw blade approaches a conductive layer inside the cast. When the risk rises, it alerts the user using an LED and a buzzer.

## Problem

Cast removal can go wrong when the blade cuts too deep.

* Overtcutting can cause patient harm
* The clinician often lacks a clear, real time signal that the blade is near the inner layer

## Concept

I added a conductive silver fabric layer inside the cast. When the blade gets close enough, the electrical path changes. The control limiter reads that change and triggers an alert.

Cast stack used in the concept:

* Stockinette
* Silver fabric (conductive layer)
* Fiberglass
![](/projects/ControlLimiter_images/Control-Limiter-Mechanism.png)
## How It Works

### What the device senses

* The system monitors the voltage difference between the cast saw and the conductive fabric layer.
* When the blade cuts through fiberglass and approaches the silver fabric, the measured signal changes because the fabric conducts.

### What the device outputs

* Visual alert via LED
* Audio alert via buzzer
* Event logging, counts how many times the blade meets the conductive layer



## System Breakdown

### Hardware

* Cast saw (Stryker style set up)
* Arduino microcontroller
* Voltage sensing circuit (digital voltmeter style input)
* LED indicator
* Buzzer
![](/projects/ControlLimiter_images/CLHardware.png)

![](/projects/ControlLimiter_images/CLSchematics.png)



### Software
![](/projects/ControlLimiter_images/BlockDiagramCL.png)
* Arduino C++ loop that samples the analog input at a fixed rate
* Threshold logic with basic filtering to reduce false triggers
* C++ event counter for contact occurrences

Arduino sketch (C++) used to read the analog voltage divider and display the input voltage on an LCD.

```cpp
#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
int analogInput = 0;

float vout = 0.0;
float vin = 0.0;
float R1 = 100000.0; // resistance of R1 (100K) - see text!
float R2 = 10000.0;  // resistance of R2 (10K) - see text!
int value = 0;

void setup(){
  pinMode(analogInput, INPUT);
  lcd.begin(16, 2);
  lcd.print("DC VOLTMETER");
}

void loop(){
  // read the value at analog input
  value = analogRead(analogInput);

  vout = (value * 5.0) / 1024.0; // see text
  vin = vout / (R2 / (R1 + R2));

  if (vin < 0.09) {
    vin = 0.0; // statement to quash undesired reading!
  }

  lcd.setCursor(0, 1);
  lcd.print("INPUT V=");
  lcd.print(vin);
  delay(500);
}
```


## Why This Matters

The project adds a simple feedback signal during a procedure where the user usually relies on feel and sound.

* Makes blade proximity more visible
* Supports safer user decisions in the moment
* Shows end to end thinking from problem to prototype

