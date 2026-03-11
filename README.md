# PS3100 Resonator - VST3 Plugin
<br>
<p align="center">
  <img src="logo.png">
</p>
<br>
<hr>

**A three-band parallel resonator VST3 plugin modeled after the Korg PS3100 resonator circuit, with design influences from the Polymoog resonator, Tiptop Audio Resonator, and Nonlinear Circuits Resonate. Features a unique per-band Dust Exciter for granular, physically-modeled resonant textures.**

<hr>

<br>

[![ENSEMBLE Demo](https://img.youtube.com/vi/6IAOZ5ATTes/maxresdefault.jpg)](https://youtu.be/6IAOZ5ATTes)
<br>
↑ Click for Youtube Video Demo ↑


<br>
<hr>

**Install (Windows VST3)**

Download the latest release .zip from the Releases page
Extract the .zip file
Copy the PS3100 Resonator.vst3 folder to:

   C:\Program Files\Common Files\VST3\

Open your DAW (Ableton, FL Studio, Bitwig, Reaper, etc.)
Rescan your VST3 plugin folder if needed
PS3100 Resonator will appear under Depattern in your plugin list

<hr>

**Circuit Heritage**
<br>
The Korg PS3100 (1977) featured a resonator section with three parallel bandpass filter stages controlled via vactrols (VTL5C3/2 optocouplers). The OTA-based filter topology, driven by BC550c/BC560c transistor pairs, produced warm, vowel-like formant peaks that gave the PS3100 its distinctive vocal quality.
This plugin faithfully models that architecture in software:

Chamberlin state-variable filters for accurate OTA bandpass behavior
Vactrol CV smoothing with asymmetric attack/release (5ms / 50ms)
Transistor-pair soft clipping at input and summing stages (BC550c/BC560c transfer characteristic)
Triangle LFO for simultaneous frequency modulation of all three bands
Per-band Dust Exciter for transient-reactive granular resonance (see below)

<hr>

**Dust Exciter**
<br>
The Dust Exciter is a unique feature not found on the original PS3100 hardware. It was discovered by accident during development and deliberately engineered into a musical tool.
How It Was Discovered
During prototyping, we noticed that rapidly adjusting the input volume knob produced a striking hollow, spectral, physically-modeled resonator sound — completely different from the normal resonator behavior. The effect only occurred while the knob was being turned, creating an ethereal ringing at the tuned band frequencies that sounded like striking a resonant metal surface.
What's Actually Happening
The artifact occurs because abrupt gain changes create step discontinuities in the audio signal. When the volume knob jumps from one value to another, the entire audio signal is instantaneously multiplied by a different number. That sudden step is mathematically equivalent to a wideband impulse — it contains energy across the entire frequency spectrum simultaneously. When this impulse hits the three parallel bandpass filters, each filter rings at its tuned resonant frequency, producing the characteristic hollow, pitched decay.
This is the exact same principle behind physical modeling synthesis: a resonant body (the bandpass filters) is excited by an impulse (the gain discontinuity). The resonator's emphasis (Q) controls determine how long the ringing sustains, and the frequency knobs determine the pitches. It's acoustically identical to striking a tuning fork, a drum head, or a metallic surface.
How It's Implemented
The Dust Exciter is a sample-and-hold gain modulator that deliberately creates these gain step discontinuities at a controllable rate:

A random gain value is sampled and held constant
The incoming audio is multiplied by this held gain before entering the bandpass filter
At irregular intervals, a new random gain value is sampled — the transition between the old and new value is the excitation impulse
The bandpass filter rings at its resonant frequency in response to each impulse

The DUST knob controls the grain rate:

At 0%: completely bypassed, no effect
At low values (~5 Hz): slow, sparse pings — subtle hollow resonance on transients
At medium values (~100–500 Hz): scattered, dusty texture — like sand on a vibrating plate
At high values (~2000 Hz): dense granular cloud — the resonator becomes a shimmering, textured instrument

Velocity sensitivity: The gain variation depth tracks the input signal's envelope, so louder audio creates bigger gain jumps and stronger excitation. Quiet passages barely excite the resonator. Percussive transients create the strongest ringing. This makes the Dust Exciter inherently musical and dynamic.
Per-band control: Each of the three bands has its own independent DUST knob, allowing you to create complex textures — for example, heavy dust excitation in the high frequencies for shimmer while keeping the low band clean for weight.
<hr>

**Parameters**
<br>
ParameterRangeDefaultDescriptionVOL IN-24 to +12 dB0 dBInput levelLFO RATE0.01–12 Hz0.3 HzFrequency modulation speed (all bands)LFO DEPTH0–100%0%Frequency modulation amountMIX0–100%70%Wet/dry blendVOL OUT-24 to +12 dB0 dBOutput levelBand I FREQ60–600 Hz180 HzLow band center frequencyBand I EMPH0.5–404.0Low band resonance (Q)Band I GAIN-60 to +12 dB0 dBLow band levelBand I DUST0–100%0%Low band dust exciter amountBand II FREQ200–3000 Hz800 HzMid band center frequencyBand II EMPH0.5–404.0Mid band resonance (Q)Band II GAIN-60 to +12 dB0 dBMid band levelBand II DUST0–100%0%Mid band dust exciter amountBand III FREQ1k–12k Hz4000 HzHigh band center frequencyBand III EMPH0.5–404.0High band resonance (Q)Band III GAIN-60 to +12 dB0 dBHigh band levelBand III DUST0–100%0%High band dust exciter amount
<hr>

**Signal Flow**
<br>
                            ┌─────────────────────────────────────────┐
                            │          PER BAND (×3 parallel)         │
                            │                                         │
Input ──► [VOL IN] ──► [Soft Clip] ──►│  [Dust Exciter] ──► [SVF Bandpass] ──► [Band Gain]  │──► [Sum]
                            │   (S&H gain mod)    (Vactrol CV)               │
                            └─────────────────────────────────────────┘
                                                                              │
                                                                              ▼
                                                              [Soft Clip] ──► [DC Block]
                                                                              │
                                                              [Wet/Dry Mix] ◄─┘
                                                                  │
                                                            [VOL OUT] ──► Output

             [Triangle LFO] ──────────► modulates all band frequencies
<hr>

**Building From Source**
<br>
See BUILD.md for complete build instructions.
Requirements:

Visual Studio 2026 (or 2022+) with "Desktop development with C++" workload
CMake 3.22+
JUCE framework (cloned automatically)

Quick build:
cmdgit clone https://github.com/juce-framework/JUCE.git
mkdir build && cd build
cmake .. -G "Visual Studio 18 2026" -A x64
cmake --build . --config Release
The built .vst3 will be in build\PS3100Resonator_artefacts\Release\VST3\ and is also auto-copied to your system VST3 folder.
<hr>

**License**
<br>
DSP code is provided as-is for personal/educational use.
JUCE has its own licensing terms — see https://juce.com/legal/
<hr>
Inspired by the Korg PS3100, Polymoog, Tiptop Audio Resonator, Nonlinear Circuits Resonate, and DSPTone Polyresonator. All product names and trademarks are the property of their respective owners.
