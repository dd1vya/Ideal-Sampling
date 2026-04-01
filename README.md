# Ideal, Natural, & Flat-top -Sampling
# Aim
Write a simple Python program for the construction and reconstruction of ideal, natural, and flattop sampling.
# Tools required
Google colab
# Theory
### Ideal sampling
Ideal sampling is a theoretical concept in which the continuous-time signal is sampled using a train of impulses (delta functions).

Characteristics

Each sample has zero width and infinite height

Sampling is done at exact instants

No distortion is introduced during sampling

Practically not realizable
### Natural sampling
In natural sampling, the signal is sampled by multiplying it with a train of rectangular pulses having finite width.

Characteristics

Pulse width is small but non-zero

The top of the pulse follows the shape of the input signal

More practical than ideal sampling
### Flat-top sampling
Flat-top sampling is a practical sampling technique where the signal is sampled and held constant for a fixed duration.

Characteristics

Each sample has a constant amplitude during the pulse width

Implemented using a Sample-and-Hold (S/H) circuit

Most commonly used in practical systems
# Program
### Ideal sampling
```
import numpy as np
from scipy.signal import resample

f, fs = 5, 100

t = np.linspace(0, 1, 1000)
ts = np.arange(0, 1, 1/fs)

x = np.sin(2*np.pi*f*t)
xs = np.sin(2*np.pi*f*ts)
xrec = resample(xs, len(t))

plt.figure(figsize=(10,6))

plt.subplot(3,1,1)
plt.plot(t, x)
plt.title("Continuous Signal")

plt.subplot(3,1,2)
plt.stem(ts, xs, basefmt="r-")
plt.title("Sampled Signal")

plt.subplot(3,1,3)
plt.plot(t, xrec, 'r--')
plt.title("Reconstructed Signal")

plt.tight_layout()
plt.show()
```
### Natural samplying
```
import numpy as np
from scipy.signal import butter, lfilter

fs, fm, fp = 1000, 5, 50
t = np.arange(0, 1, 1/fs)
m = np.sin(2*np.pi*fm*t)                
p = (np.sin(2*np.pi*fp*t) > 0).astype(int) 

s = m * p                              
b, a = butter(5, 10/(fs/2))
r = lfilter(b, a, s)

plt.figure(figsize=(10, 8))

plt.subplot(4,1,1)
plt.plot(t, m)
plt.title("Message Signal")

plt.subplot(4,1,2)
plt.plot(t, p)
plt.title("Pulse Train")

plt.subplot(4,1,3)
plt.plot(t, s)
plt.title("Natural Sampling")

plt.subplot(4,1,4)
plt.plot(t, r, 'g')
plt.title("Reconstructed Signal")

plt.tight_layout()
plt.show()
```
### Flat-top samplying
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs = 1000
t = np.arange(0, 1, 1/fs)
fm = 5
fp = 50
message = np.sin(2*np.pi*fm*t)

step = int(fs / fp)
indices = np.arange(0, len(t), step)

flat_top = np.zeros_like(t)
width = step // 2
for i in indices:
    flat_top[i:min(i+width, len(t))] = message[i]

pulse = np.zeros_like(t)
pulse[indices] = 1

b, a = butter(5, (2*fm)/(0.5*fs), 'low')
reconstructed = lfilter(b, a, flat_top)

plt.figure(figsize=(14, 10))

plt.subplot(4, 1, 1)
plt.plot(t, message, label='Original Message Signal')
plt.title('Original Message Signal')
plt.xlabel('Time (s)')
plt.ylabel('Amplitude')
plt.legend()
plt.grid(True)

plt.subplot(4, 1, 2)
plt.stem(t[indices], pulse[indices], basefmt=" ", label='Ideal Sampling Instances')
plt.title('Ideal Sampling Instances')
plt.xlabel('Time (s)')
plt.ylabel('Amplitude')
plt.legend()
plt.grid(True)

plt.subplot(4, 1, 3)
plt.plot(t, flat_top, label='Flat-Top Sampled Signal')
plt.title('Flat-Top Sampled Signal')
plt.xlabel('Time (s)')
plt.ylabel('Amplitude')
plt.grid(True)
plt.legend()

plt.subplot(4, 1, 4)
plt.plot(t, reconstructed, label=f'Reconstructed Signal (Low-pass Filter, Cutoff={2*fm} Hz)', color='green')
plt.title('Reconstructed Signal')
plt.xlabel('Time (s)')
plt.ylabel('Amplitude')
plt.legend()
plt.grid(True)

plt.tight_layout()
plt.show()
```
# Output Waveform
### Ideal samplying
<img width="990" height="590" alt="image" src="https://github.com/user-attachments/assets/9e10c20f-62e4-4c39-9f8b-cd25c209d126" />



### Natural samplying
<img width="989" height="790" alt="image" src="https://github.com/user-attachments/assets/964ba284-54ca-4da0-9dcf-b3665916c088" />



### Flat-top samplying
<img width="1398" height="990" alt="image" src="https://github.com/user-attachments/assets/64d0928f-b046-404e-ac81-297d09d0cb06" />



# Results
Thus, the python programs for ideal sampling, natural sampling and flat-top sampling has been executed and verified successfully.
