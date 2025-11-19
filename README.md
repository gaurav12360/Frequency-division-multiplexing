# Frequency-division-multiplexing
# Aim

To study and implement Frequency Division Multiplexing (FDM) using multiple baseband signals modulated on different carrier frequencies and to observe the multiplexed signal, its transmission, and perfect demodulation of individual channels.

Theory

Frequency Division Multiplexing (FDM) is a technique where multiple signals are transmitted simultaneously over a single communication channel, with each signal assigned a unique carrier frequency. These carrier frequencies are spaced apart so that their spectra do not overlap.

Each baseband message signal is modulated using a different carrier, typically using Amplitude Modulation (AM – DSB-SC) or SSB.
The modulated signals are added together to form one composite FDM signal.

At the receiver end:

The multiplexed signal is mixed with the corresponding carrier.

An ideal low-pass filter (LPF) extracts the original baseband message.

Since carrier frequencies are well-spaced, perfect reconstruction is possible.

Conditions for FDM:

Carrier frequencies must be sufficiently spaced (guard bands)

Message bandwidths must not overlap

Filters must ideally suppress unwanted frequencies

FDM is used in:

Radio broadcasting (FM/AM radio channels)

Telephone systems (old analog trunks)

Cable TV transmission

Algorithm
Transmitter Side

Start the program and define sampling frequency and time vector.

Generate multiple baseband message signals at different frequencies.

Assign a unique carrier frequency to each message signal.

Generate corresponding carrier signals using cosine waves.

# PROGRAM :
```
clear;

Fs = 10000;
T = 0.05;
t = 0:1/Fs:T-1/Fs;
N = length(t);

fbase = [50, 100, 150, 200, 250, 300];
A = [1, 0.9, 0.8, 1, 0.7, 0.6];

s = zeros(6, N);
for k = 1:6
    s(k,:) = A(k) * sin(2 * %pi * fbase(k) * t);
end

fc = [1000, 2000, 3000, 4000, 5000, 6000];
carriers = zeros(6, N);
for k = 1:6
    carriers(k,:) = cos(2 * %pi * fc(k) * t);
end

modulated = zeros(6, N);
for k = 1:6
    modulated(k,:) = s(k,:) .* carriers(k,:);
end

fdm = sum(modulated, "r");

function y = ideal_lpf(sig, Fs, cutoff)
    Nloc = length(sig);
    S = fft(sig);
    mask = zeros(1, Nloc);
    half = floor(Nloc/2);
    for i = 1:Nloc
        idx = i-1;
        if idx > half then idx = idx - Nloc; end
        freqHz = idx * Fs / Nloc;
        if abs(freqHz) <= cutoff then mask(i) = 1; end
    end
    Y = S .* mask;
    y = real(ifft(Y));
endfunction

recovered = zeros(6, N);
cutoff = 400;

for k = 1:6
    y = 2 * fdm .* carriers(k,:);
    recovered(k,:) = ideal_lpf(y, Fs, cutoff);
end

scf(0); clf();
for k = 1:6
    subplot(3,2,k);
    plot(t, s(k,:));
end

scf(1); clf();
plot(t, fdm);

scf(2); clf();
for k = 1:6
    subplot(3,2,k);
    plot(t, recovered(k,:));
end
```
# OUTPUT
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/22324700-3dd2-464e-8fd0-bfb2ef49fa17" />

# RESULT

All six baseband signals were successfully multiplexed and demultiplexed with minimal distortion. The recovered signals matched the original signals in amplitude and frequency, demonstrating the working of Frequency Division Multiplexing (FDM).
