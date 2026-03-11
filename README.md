# PS3100 Resonator — VST3 Plugin
<br>
<p align="center">
  <img src="logo.png">
</p>
<br>

**A three-band parallel resonator VST3 plugin modeled after the Korg PS3100
resonator circuit, with design influences from the Polymoog resonator,
Tiptop Audio Resonator, and Nonlinear Circuits Resonate.**
<hr>

## Circuit Heritage

The Korg PS3100 (1977) featured a resonator section with three parallel
bandpass filter stages controlled via vactrols (VTL5C3/2 optocouplers).
The OTA-based filter topology, driven by BC550c/BC560c transistor pairs,
produced warm, vowel-like formant peaks that gave the PS3100 its
distinctive vocal quality.

This plugin faithfully models that architecture in software:
- **Chamberlin state-variable filters** for accurate OTA bandpass behavior
- **Vactrol CV smoothing** with asymmetric attack/release
- **Transistor-pair soft clipping** at input and summing stages
- **Feedback path** from output back to input
- **Triangle LFO** for frequency modulation of all bands

## Building

See [BUILD.md](BUILD.md) for complete Windows build instructions.

Quick version:
```cmd
git clone https://github.com/juce-framework/JUCE.git
mkdir build && cd build
cmake .. -G "Visual Studio 17 2022" -A x64
cmake --build . --config Release
```

## License

DSP code is provided as-is for personal/educational use.
JUCE has its own licensing terms — see https://juce.com/legal/
