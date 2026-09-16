# Audio Fingerprinting with STFT

A signal-processing project exploring audio fingerprinting using short-time Fourier transforms (STFT), spectrogram peak features, and relative time-frequency relationships.

## Overview

This project investigates how an audio signal can be represented by a compact fingerprint derived from its time-frequency structure.

The current implementation uses Python to:

1. Compute an STFT of an audio signal
2. Visualize its time-frequency representation
3. Identify prominent spectrogram peaks
4. Construct a fingerprint from relative time-frequency spacing between peaks
5. Compare fingerprints from clean and noisy audio

The long-term goal is to develop a hardware implementation of the fingerprinting pipeline on an FPGA for real-time audio analysis and matching.

## Current Implementation: Phase 1

### STFT Analysis

Audio signals are transformed into the frequency domain using a Short-Time Fourier Transform (STFT). This produces a spectrogram showing how the frequency content of the signal changes over time.

STFT basically takes \(m\) blocks of data with \(n\) amount of data between them. There is overlap between the blocks, and the amount of overlap depends on the step size. The STFT performs a Fourier transform on the data for each block.

STFT is useful for analyzing audio signals because it takes into account both the time and frequency at which a peak occurred. Therefore, STFT can be used to create a spectrogram of the data.


### Peak-Based Fingerprinting

Prominent peaks in the spectrogram are represented by their time and frequency coordinates:

\[
(m, k)
\]

The fingerprint is constructed from the relative differences between consecutive peaks:

\[
(\Delta m, \Delta k)
=
(m_{i+1}-m_i,\; k_{i+1}-k_i)
\]

This representation focuses on the relative structure of the peaks rather than their absolute locations.

### Noise Evaluation

Clean and noisy audio fingerprints are compared to investigate how consistently the relative peak structure is preserved when background noise is introduced.

## Phase 1 Findings

The first phase investigated how background noise affects spectrogram peak locations and the resulting audio fingerprint.

Background noise made the spectrogram peaks less distinct. In the noisy spectrogram, prominent regions appeared smeared across neighboring frequencies, making the exact signal frequency more difficult to identify. In some cases, the original peak was obscured or replaced by a noise-induced peak.

Despite changes in the absolute peak locations, the relative differences between consecutive peaks remained mostly consistent. The noisy fingerprint points generally clustered close to the corresponding clean fingerprint points, with two notable differences where the signal was more strongly affected by noise.

This suggests that the temporal and frequency structure of the audio was partially preserved even when individual peak locations changed.

Absolute spectrogram coordinates depend on the precise time and frequency location of each peak. Relative differences instead describe the relationship between neighboring peaks.

Therefore, a shift in the absolute position of the peaks can occur without necessarily changing their relative structure. This makes relative peak spacing a potentially more robust representation for audio fingerprinting, particularly when recordings are affected by noise or begin at different points in time.

> **Phase 1 conclusion:** For the data tested, relative time-frequency differences were more stable under background noise than the absolute locations of individual spectrogram peaks.

## Project Status

### Completed

- [x] Implemented STFT-based audio analysis in Python
- [x] Generated and visualized spectrograms
- [x] Identified spectrogram peaks
- [x] Constructed relative time-frequency fingerprints
- [x] Compared fingerprints between clean and noisy audio

### In Progress

- [ ] Automate spectrogram peak detection
- [ ] Develop an automated fingerprint matching method
- [ ] Quantitatively evaluate matching performance under different noise conditions
- [ ] Develop a hardware architecture for the fingerprinting pipeline
- [ ] Implement spectrum analysis in Verilog
- [ ] Deploy the fingerprinting pipeline on an FPGA
- [ ] Investigate real-time audio matching

## Technologies

- Python
- NumPy
- SciPy
- Matplotlib
- Digital Signal Processing (DSP)
- Short-Time Fourier Transform (STFT)
- Verilog / FPGA *(planned)*

## Planned Hardware Architecture

The eventual hardware implementation is planned to follow this pipeline:

Audio Input   
Windowing  
FFT / Spectrum Analysis   
Magnitude Spectrum   
Peak Detection   
Fingerprint Generation   
Fingerprint Matching

The Python implementation will serve as a reference model for validating the hardware implementation.