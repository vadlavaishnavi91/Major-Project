% Efficiency Analysis of Massive MIMO under Perfect CSI
clc; clear; close all;

% System Parameters
K = 10;                 % Number of users
M = 100:100:1000;       % Number of base station antennas
SNR_dB = 0:5:30;        % Signal-to-Noise Ratio range (in dB)
SNR = 10.^(SNR_dB/10);  % Linear SNR

% Preallocate Results
spectral_eff = zeros(length(M), length(SNR_dB));

for m = 1:length(M)
    for s = 1:length(SNR)
        % Perfect CSI - Zero Forcing (ZF) Precoding
        % Effective SINR for each user (under perfect CSI)
        SINR = SNR(s) * (M(m) - K) / K;

        % Spectral Efficiency (per user)
        R_user = log2(1 + SINR);

        % Total Spectral Efficiency (sum over all users)
        spectral_eff(m, s) = K * R_user;
    end
end

% Energy Efficiency Calculation (assuming fixed power consumption model)
P_tx = 1;                 % Transmit power (normalized)
P_circuit = 0.1 * M;      % Circuit power proportional to antennas
energy_eff = spectral_eff ./ (P_tx + P_circuit');

% Plot Spectral Efficiency
figure;
plot(M, spectral_eff(:, end), 'o-', 'LineWidth', 1.5);
xlabel('Number of Antennas (M)');
ylabel('Spectral Efficiency (bps/Hz)');
title('Spectral Efficiency vs Number of Antennas (Perfect CSI)');
grid on;

% Plot Energy Efficiency
figure;
plot(M, energy_eff(:, end), 's-', 'LineWidth', 1.5);
xlabel('Number of Antennas (M)');
ylabel('Energy Efficiency (bits/Joule)');
title('Energy Efficiency vs Number of Antennas (Perfect CSI)');
grid on;
