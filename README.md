// User-controlled parameters
float freq = ch("frequency"); // Speed of movement
float amp = ch("amplitude"); // Strength of movement
float side_amp = ch("side_amplitude"); // Strength of sideways sway
float noise_strength = ch("noise_strength"); // Random noise intensity
float delay_max = ch("delay_max"); // Maximum delay between points

// Time and delay
float t = @Time;
float delay = fit01(rand(@ptnum), 0, delay_max); 

// Up & down motion
@P.y += sin(t * freq + @ptnum * 0.3 - delay) * amp;

// Side-to-side sway
@P.x += sin(t * (freq * 0.8) + @ptnum * 0.5 - delay) * side_amp;
@P.z += cos(t * (freq * 0.6) + @ptnum * 0.4 - delay) * side_amp;

// Add procedural noise for randomness
vector noiseOffset = snoise(@P * 1.2 + t * 0.5);
@P += noiseOffset * noise_strength;


