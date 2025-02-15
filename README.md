// User-defined parameters (exposed in the UI)
float freq = ch("frequency");         // Global speed of oscillation
float amp = ch("amplitude");          // Vertical movement amplitude
float side_amp = ch("side_amplitude");// Side-to-side swaying strength
float noise_strength = ch("noise_strength"); // Organic noise factor
float delay_max = ch("delay_max");    // Max time offset per point
float phase_offset = ch("phase_offset"); // Per-point variation

// Time and delay calculations
float t = @Time;
float delay = fit01(rand(@ptnum), 0, delay_max); 
float phase = @ptnum * phase_offset; // Adds unique movement per point

// Add per-point delay based on point number for a more cascading effect
float pointDelay = fit(@ptnum, 0, @numpt, 0, delay_max); // Higher points lag more

// Primary vertical oscillation (core movement)
@P.y += sin(t * freq + phase - pointDelay) * amp;

// Lateral sway (adds weight and realism)
@P.x += sin(t * (freq * 0.8) + phase - pointDelay) * side_amp;
@P.z += cos(t * (freq * 0.6) + phase - pointDelay) * side_amp;

// Organic noise for natural variation
vector noiseOffset = snoise(@P * 1.2 + t * 0.5);
@P += noiseOffset * noise_strength;



