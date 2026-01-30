
![ars_Stats](https://github.com/user-attachments/assets/4f7297a5-c5b8-44c6-be85-32ec53d766a6)

<img width="1783" height="914" alt="Screenshot from 2026-01-30 16-07-36" src="https://github.com/user-attachments/assets/82a215ca-39e7-43c6-ad0b-3f73f7e6ca7b" />


# Arnold Render Samples Stats (ARS)

## Overview

**Arnold Render Samples Stats (ARS)** is a real-time diagnostic and optimization tool designed to expose detailed Arnold rendering statistics directly from render metadata.
Its primary goal is to help lighting and look-development artists **monitor, debug, and optimize renders efficiently** by understanding the true cost of sampling before launching final renders.

ARS converts Arnold’s internal EXR metadata into **clear, readable, and production-useful metrics**, allowing artists to make **data-driven decisions** instead of relying on trial and error.

---

## Why ARS?

Arnold’s sampling system is multiplicative. Small changes in AA or GI samples can cause **exponential increases in render time**, memory usage, and rays per pixel.

ARS helps answer critical production questions:

* Are the current samples actually needed?
* Which component is driving noise or render cost?
* Is the scene oversampled?
* Can samples be redistributed instead of increased?

This tool is built to **optimize shots before final delivery**, saving render time and farm cost.

---

## Key Features

### Render Status & System Information

* Active **Render Layer**
* Render time formatted as **HH:MM:SS**
* Number of render **threads**
* **Peak memory usage** displayed in GB
* Final render **resolution**

### Detailed Sampling Breakdown

ARS displays and evaluates:

* Camera (AA) samples
* Diffuse samples + depth contribution
* Specular samples + depth contribution
* Transmission samples + depth contribution
* Subsurface Scattering (SSS) samples

Each component includes a **maximum sample calculation**, showing worst-case per-pixel cost.

### Sample Cost Analysis

* **Minimum total samples per pixel**
* **Maximum total samples per pixel** (depth-aware)
* **Rays per pixel**

This allows artists to immediately understand how expensive a pixel truly is.

### Artist-Friendly Utilities

* Frame number display
* Custom **Note** field for annotations (WIP, test, approved, etc.)

---

## Sampling Logic (How ARS Interprets Arnold)

Arnold sampling is calculated as:

```
Total Rays ≈ AA² × GI² × Depth
```

### Example 1

* Camera (AA) = 4
* Diffuse = 3

Calculation:

* AA Rays = 4² = 16
* Diffuse Rays per AA ray = 3² = 9
* **Total Diffuse Rays per pixel = 16 × 9 = 144**

### Example 2

* Camera (AA) = 3
* Diffuse = 2

```
3² × 2² = 9 × 4 = 36 diffuse rays per pixel
```

ARS performs these calculations automatically and displays the results in real time.

---

## Recommended Workflow (Best Practice)

### Test Before Final Render

Before launching a **final production render**, ARS is intended to be used during **test renders**.

#### Suggested Workflow:

1. Launch a **short test render** (e.g. **5 frames**)
2. Use ARS to capture:

   * Sampling data
   * Rays per pixel
   * Memory usage
   * Min / Max total samples
3. **Calculate the average values** across the test frames

Averaging multiple frames helps account for:

* Lighting variation
* Motion
* Reflections and refractions
* Depth-heavy or complex frames

### Decision Making

Based on ARS data, artists can confidently decide to:

* **Increase samples** (only where needed)
* **Decrease samples** if the shot is oversampled
* Redistribute samples (AA vs Diffuse vs Specular vs Transmission)

This avoids blindly increasing samples and ensures **final renders are launched with validated, optimized settings**.

---

## Primary Purpose

The core purpose of **Arnold Render Samples Stats (ARS)** is to:

* Read Arnold render metadata directly from EXR files
* Visualize true sampling cost per pixel
* Help artists decide whether current samples are:

  * Good to go
  * Overkill
  * Or insufficient
* Optimize shots **before** committing to final renders

ARS promotes **efficient, consistent, and predictable rendering** in production environments.

---

## Ideal Use Cases

* Look development and lighting
* Shot-based optimization
* Debugging noisy or slow renders
* Heavy GI / reflection / transmission scenes
* Production pipelines where render cost matters

<img width="598" height="292" alt="Screenshot from 2026-01-30 16-08-32" src="https://github.com/user-attachments/assets/a8924075-d9dc-42b6-bc3d-0c5e9b3eac2b" />



