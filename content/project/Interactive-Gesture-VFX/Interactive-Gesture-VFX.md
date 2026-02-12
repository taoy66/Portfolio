---
layout: project
title: Computer Vision Hand Gesture Recognition System
date: 2026-02-11
description: A browser based computer vision system that tracks hands gesture in real time and control a 15,000 dot particle animation that forms visual effects like 3D heart, layered 3D text, and continuous fireworks.
summary: ---
layout: project
title: Computer Vision Hand Gesture Recognition System
date: 2026-02-11
description: An AI powered computer vision system that tracks hands gesture in real time and control a 15,000 dot particle animation that forms visual effects like 3D heart, layered 3D text, and continuous fireworks.
summary: An AI powered computer vision system in the browser for interactive visual art. Uses a pre trained AI hand tracking model (MediaPipe) and my gesture logic to control the animation. A webcam feed tracks your hands and drives a 15,000 dot scene in real time.
hidemeta: true

weight: 203

cover:
  image: "Coverimages/interactive-gesture-vfx-hero.png"

tags: [JavaScript, Three.js, MediaPipe, Computer Vision, Hand Tracking, Gesture Recognition, WebGL, Vite, GitHub Actions]
---

## Project Overview

**Interactive gesture vfx** is a browser based computer vision project. It uses a webcam feed and MediaPipe hand tracking to detect hands and output 21 landmarks per hand in real time. I convert those landmarks into gesture states that control a 15,000 particle Three.js scene, including a 3D heart, layered 3D text, and continuous fireworks.

**Tools:** JavaScript, Three.js, WebGL, MediaPipe HandLandmarker (@mediapipe/tasks vision), Vite, GitHub Actions

*Live Demo*
* Github Pages: https://taoy66.github.io/interactive-gesture-vfx/
* Github Repo: https://github.com/taoy66/interactive-gesture-vfx

## Computer Vision Highlights

* Real time hand detection and tracking from a live webcam stream
* 21 landmark keypoints per hand used as the core feature representation
* Scale normalized geometric rules for gesture recognition to reduce jitter
* Two hand interaction logic so left hand and right hand can drive different effects

---

## 1. The Challenge & Design Goals

I wanted an interactive system that runs fully in the browser and still looks smooth.

* Track up to two hands in real time using a webcam
* Turn stable gestures into clean visual states (heart, text, fireworks)
* Render a large particle count without lag
* Keep the setup simple so anyone can play with it in a minute

---

## 2. System Architecture

The app follows a simple pipeline:

* **Webcam input**
  * Uses `getUserMedia()` to capture the camera stream
* **Hand tracking**
  * MediaPipe HandLandmarker detects up to 2 hands
  * Returns 21 3D landmarks per hand per frame
  * Normalizes distances by hand size so gesture thresholds stay consistent across different hand sizes
* **Render loop**
  * A single animation loop updates gesture state and particle targets
  * Three.js renders particles using `BufferGeometry` for performance
---

## 3. Gesture Mapping

I use landmark geometry rules to classify poses. The goal is stable gesture recognition without training a custom model.

* **Left hand fist**
  * Morph particles into a 3D heart
* **Right hand fingers**
  * 1 finger: “Would you”
  * 2 fingers: “Would you” + “be my”
  * 3 fingers: “Would you” + “be my” + “Valentine?”
* **Right hand fist**
  * Starts fireworks
  * Keeps spawning until the right hand leaves the camera view

---

## 4. Particle Rendering (15,000 points)

I render particles as points and move them toward different target layouts each frame.

* **Swarm state**
  * Default free floating particle distribution
* **Morph states**
  * Each frame interpolates particle positions toward a target layout (swarm, heart, text)
* **Why this works**
  * `BufferGeometry` keeps updates fast
  * A single point cloud lets me swap shapes without rebuilding the scene

---

## 5. How I Generate the 3D Text and Heart

**Text as particles**
* Rasterize text on an offscreen `<canvas>`
* Sample pixels into points
* Add a small Z depth to make it feel 3D

**Heart as particles**
* Generate points from a mathematical heart field
* Add Z thickness for volume

*Demo Snapshot*
![](/projects/gesturevfx_images/gesturevfx_scene.png)

---

## 6. Build, Run, Deploy

**Run locally**
```bash
npm install
npm run dev
```

---
