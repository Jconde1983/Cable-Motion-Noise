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

// Main vertical oscillation (core movement)
@P.y += sin(t * (freq * 2.0) + phase - pointDelay) * amp;  // Direct control over vertical motion

// Lateral sway (adds weight and realism)
@P.x += sin(t * (freq * 0.6) + phase - pointDelay) * side_amp;
@P.z += cos(t * (freq * 0.4) + phase - pointDelay) * side_amp;

// Apply noise - separate from main movement, affecting all axes
vector noiseOffset = snoise(@P * 1.5 + t * 0.8); // Organic noise
@P += noiseOffset * noise_strength * 0.3; // Light noise for natural look

// Optional: Apply stronger noise on specific axes
@P.x += noiseOffset.x * noise_strength * 1.0;  // Some horizontal noise
@P.y += noiseOffset.y * noise_strength * 1.5;  // Larger noise effect on Y-axis
@P.z += noiseOffset.z * noise_strength * 0.8;  // Slight noise on Z-axis
