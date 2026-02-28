# The Mathematical Framework

Impulse-X operates on the principle that market alpha exists in the transitions between hidden regimes. To decode these shifts in real-time without the fatal flaw of lagging indicators, we rely on three mathematical pillars.

---

## 1. Kinematic Derivatives & Causal Phase

Standard technical analysis relies on Moving Averages (MAs) or Relative Strength (RSI). These are inherently flawed because they average past data, creating a phase lag. By the time a moving average crosses, the optimal entry has already passed.

Impulse-X discards time-averaging. Instead, we treat price as a physical object in motion and measure its physics. 

### The Savitzky-Golay Filter

To calculate true momentum without lag, we apply a **Savitzky-Golay filter** to the logarithmic price series. Instead of averaging data, this filter fits a low-degree polynomial to a rolling window of price action. This allows us to extract smooth, instantaneous derivatives:

* **Velocity ($v$):** The first derivative of price. It measures the true speed and direction of capital flow.
  $$v = \frac{dp}{dt}$$
* **Acceleration ($a$):** The second derivative. It measures momentum ignition or exhaustion.
  $$a = \frac{d^2p}{dt^2}$$

**The Practical Logic:** Imagine tracking a car. Velocity tells you it is moving forward at 60 mph (a bullish trend). However, if the car is approaching a red light, the driver hits the brakes. The car is still moving forward (velocity is positive), but it is slowing down (**acceleration becomes negative**). 

Impulse-X detects this negative acceleration in price, signaling trend exhaustion long before the price actually reverses and moving averages catch up.

### Causal Hilbert Transform

Markets are cyclical, moving between expansion and contraction. To understand where the market is within a cycle *right now*, Impulse-X uses a **Hilbert Transform** to calculate the instantaneous phase angle.

The Transform creates an analytic signal from the price data:
$$X_a(t) = X(t) + j \cdot \mathcal{H}\{X(t)\}$$

**The Practical Logic:**
Think of a swinging pendulum. The Hilbert Transform tells us exactly where the pendulum is in its arc (e.g., passing the center at maximum speed, or hanging suspended at the apex right before reversing). 

*Crucially, Impulse-X applies reflective padding to the boundaries of the data array before applying the transform.* This eliminates the "end-point distortion" common in standard digital signal processing, ensuring our phase reading is strictly causal and free from look-ahead bias.

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
