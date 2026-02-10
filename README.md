# ADSR Echo (Beta)
**Senior Design Project Group L01 | Multi-Reverb Effect Plugin**

**ADSR Echo** is a cross-platform JUCE audio plugin (VST3/AU) made with **C++**, currently in **beta development**. 

This project aims to create a hub for reverb effects in music production/audio design, to solve the problem of inefficient workflow when managing multiple reverb effects on an audio workstation. Our plugin explores a **modular audio-processing architecture** that allows users to freely construct effect chains using both **series and parallel processing**, similar to modern DAWs and modular effect racks.

This repository represents an **in-progress group project**. The current UI and feature set are functional but temporary, and will continue to evolve as development progresses.

---

## Project Status

**Beta / In Development**

- Core DSP architecture is implemented and functional  
- Modular routing (series + parallel chains) is working  
- UI is usable but **temporary** and subject to redesign  
- Feature set is actively expanding  

---

## My Contributions

My primary responsibility on this project was the **core modular audio architecture**, including:

- Designing and implementing the **modular processing system**
- Laying out the **audio processor structure** to support:
  - Multiple effect slots per chain
  - Dynamic module creation and removal
  - Both **series** and **parallel** signal routing
- Implementing the **effect module framework**, allowing different DSP modules to be swapped into slots
- Crafting the plugin parameter tree (apvts), allowing the state of the plugin to be saved and reloaded between sessions
- Designing and implementing the **temporary UI** needed to interact with the modular system, including:
  - Module slot editors
  - Chain selection
  - Per-chain controls
 
Additionally, I ensured **thread safety and real-time audio integrity**:

- Carefully managed interactions between the **GUI/message thread** (asynchronous) and **audio thread** (real-time)
- Used **atomic flags** and **pending actions** to move modules or update UI without interfering with buffer processing  
- Avoided memory allocations on the audio thread wherever possible
  - Memory allocation can take an indeterminate amount of time
  - The audio thread must finish processing the buffer quickly, or glitches/pops can occur in the resulting audio
- Ensured that **processing attributes (modules, chain order, parameters)** remain consistent during buffer processing

This work forms the backbone of the plugin and enables future expansion of DSP modules and UI features.

---

## Modular Architecture Overview

The plugin is built around the idea of **chains** and **slots**:

- Multiple **chains** can run in parallel  
- Each chain contains a series of **module slots**  
- Each slot hosts a DSP module (Delay, Reverb, Convolution, etc.)  
- Chains are mixed together at the output, allowing true parallel processing  

This design allows users to:

- Experiment with different module orders  
- Blend parallel effect chains  
- Extend the system with new DSP modules easily

---

## Relevant Code

The most relevant parts of my work are located here:

### Core Audio Architecture

- **Audio Processor**
  - `PluginProcessor.h`
  - `PluginProcessor.cpp`

These files contain:

- Modular chain layout
- Parallel and series processing logic
- Slot management
- Signal routing and summing
- Parameter management and state saving/loading

---

### Effect Module System

- **Module Slot Framework**
  - `Modular Classes/ModuleSlot.h`

- **Effect Modules**
  - `Modular Classes/Effect Modules/`
    - `DelayModule.*`
    - `ReverbModule.*`
    - `ConvolutionModule.*`

These files define the DSP module interface and the concrete effect implementations used by the processor.

---

### Temporary UI (Designed & Implemented by Me)

- **Plugin Editor**
  - `PluginEditor.h`
  - `PluginEditor.cpp`

- **Module Slot UI**
  - `Modular Classes/ModuleSlotEditor.*`

The UI reflects the underlying modular architecture and was designed as a functional, interim solution during development. A more polished UI is planned for later iterations.

---

## Technologies Used

- **JUCE Framework**
- **Projucer IDE Tool** 
- **C++17**
- **VST3 / AU Plugin Formats**

---

## Future Work

- Expanded DSP module library  
- Preset management  
- UI redesign and visual polish  
- Advanced modulation and automation features  

