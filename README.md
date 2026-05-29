# PSK and QPSK Modulation & Demodulation

# Aim
To write a simple Python program for the modulation and demodulation of:
- Phase Shift Keying (PSK)
- Quadrature Phase Shift Keying (QPSK)

---

# Tools Required
- Python 3.x
- NumPy Library
- Matplotlib Library
- Jupyter Notebook / VS Code / Google Colab

Install required libraries:

```bash
pip install numpy matplotlib
```

---

# Theory

## Phase Shift Keying (PSK)
PSK is a digital modulation technique in which the phase of the carrier signal is changed according to the binary input data.

- Binary 1 → Normal Phase
- Binary 0 → Phase Shift of 180°

---

## Quadrature Phase Shift Keying (QPSK)
QPSK is an advanced form of PSK in which two bits are transmitted simultaneously using four different phase angles.

| Bit Pair | Phase |
|----------|-------|
| 00 | 45° |
| 01 | 135° |
| 10 | 225° |
| 11 | 315° |

---

# Program

```python
import numpy as np
import matplotlib.pyplot as plt

# Binary Data
data = [1, 0, 1, 1, 0, 0, 1, 0]

# Parameters
fc = 5          # Carrier frequency
samples = 100   # Samples per bit

# Time Axis
t = np.linspace(0, len(data), len(data) * samples)

# Digital Signal
digital_signal = np.repeat(data, samples)

# ----------------------------------------
# BPSK Modulation
# ----------------------------------------

psk_signal = []

for i, bit in enumerate(data):

    t_bit = np.linspace(i, i + 1, samples)

    if bit == 1:
        carrier = np.sin(2 * np.pi * fc * t_bit)
    else:
        carrier = -np.sin(2 * np.pi * fc * t_bit)

    psk_signal.extend(carrier)

psk_signal = np.array(psk_signal)

# ----------------------------------------
# BPSK Demodulation
# ----------------------------------------

psk_demod = []

for i in range(0, len(psk_signal), samples):

    segment = psk_signal[i:i+samples]

    if np.mean(segment) > 0:
        psk_demod.extend([1] * samples)
    else:
        psk_demod.extend([0] * samples)

# ----------------------------------------
# QPSK Modulation
# ----------------------------------------

# Group bits into pairs
bit_pairs = []

for i in range(0, len(data), 2):
    bit_pairs.append((data[i], data[i+1]))

qpsk_signal = []

phase_map = {
    (0,0): np.pi/4,
    (0,1): 3*np.pi/4,
    (1,0): 5*np.pi/4,
    (1,1): 7*np.pi/4
}

for i, pair in enumerate(bit_pairs):

    t_bit = np.linspace(i, i + 1, samples)

    phase = phase_map[pair]

    carrier = np.sin(2 * np.pi * fc * t_bit + phase)

    qpsk_signal.extend(carrier)

qpsk_signal = np.array(qpsk_signal)

# ----------------------------------------
# QPSK Demodulation
# ----------------------------------------

qpsk_demod = []

for i in range(0, len(qpsk_signal), samples):

    segment = qpsk_signal[i:i+samples]

    correlation = []

    for pair, phase in phase_map.items():

        ref = np.sin(2 * np.pi * fc *
                     np.linspace(0,1,samples) + phase)

        corr = np.sum(segment * ref)

        correlation.append((corr, pair))

    detected = max(correlation)[1]

    qpsk_demod.extend([detected[0]] * (samples//2))
    qpsk_demod.extend([detected[1]] * (samples//2))

# ----------------------------------------
# Plotting
# ----------------------------------------

plt.figure(figsize=(12,10))

# Digital Signal
plt.subplot(5,1,1)
plt.step(t, digital_signal)
plt.title("Digital Input Signal")

# BPSK Signal
plt.subplot(5,1,2)
plt.plot(t, psk_signal)
plt.title("PSK Modulated Signal")

# BPSK Demodulated
plt.subplot(5,1,3)
plt.step(t, psk_demod)
plt.title("PSK Demodulated Signal")

# QPSK Signal
t_qpsk = np.linspace(0, len(bit_pairs),
                     len(bit_pairs) * samples)

plt.subplot(5,1,4)
plt.plot(t_qpsk, qpsk_signal)
plt.title("QPSK Modulated Signal")

# QPSK Demodulated
plt.subplot(5,1,5)
plt.step(t, qpsk_demod)
plt.title("QPSK Demodulated Signal")

plt.tight_layout()
plt.show()
```

---

# Output Waveform

<img width="1001" height="836" alt="image" src="https://github.com/user-attachments/assets/9e665031-7b15-41d1-b7f0-14f194f07a57" />


# Results

Thus, the modulation and demodulation of:
- Phase Shift Keying (PSK)
- Quadrature Phase Shift Keying (QPSK)

were successfully implemented using Python. The corresponding modulated and demodulated waveforms were obtained and analyzed successfully.

---

# Hardware Experiment Output Waveform

Attach the hardware experiment waveform images here.

Example:

```md

```

---

# Applications
- Digital Communication Systems
- Satellite Communication
- Wireless Communication
- Mobile Communication
- Broadband Networks

---

# Author
YADHAV J
B.E. Electronics and Communication Engineering (ECE)  
Saveetha Engineering College
