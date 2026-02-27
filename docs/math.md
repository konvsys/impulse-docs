# The Mathematical Framework

Impulse-X operates on the principle that market "alpha" exists in the transition between regimes. We utilize three primary mathematical pillars to identify these shifts.

### 1. Kinematic Derivatives

We apply a **Savitzky-Golay filter** to price data to derive smooth first and second derivatives without the phase lag of moving averages. 

* **Velocity ($v$):** The rate of change of price.
* **Acceleration ($a$):** The rate of change of velocity, identifying momentum ignition.

### 2. Shannon Entropy ($H$)

Entropy measures the "Chaos" or uncertainty in a price series. 
$$H(X) = -\sum_{i=1}^{n} P(x_i) \log_b P(x_i)$$

* **Low Entropy:** High-information, trending environments.
* **High Entropy:** Mean-reverting, "noisy" environments where signals decay rapidly.

### 3. Hidden Markov Models (HMM)

The engine assumes that market states are "Hidden." By observing the relationship between Kinematics and Entropy, the HMM decodes the most likely underlying state:

1. **Steady Drift:** High Velocity, Low Entropy.
2. **Volatile Compression:** Low Velocity, Rising Acceleration.
3. **Chaotic Distribution:** High Entropy, Divergent Kinematics.
