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

---

## 2. Shannon Entropy & Signal Noise

While Kinematics tell us the direction and speed of the market, **Shannon Entropy** ($H$) tells us if we should actually trust that movement. Borrowed from Information Theory, Entropy measures the level of "Chaos," uncertainty, or predictability within the price series.

$$H(X) = -\sum_{i=1}^{n} P(x_i) \log_b P(x_i)$$

In this framework, $P(x_i)$ represents the probability of the market's velocity falling into a specific state over our lookback window. 

### The "Maximum Chaos" Case (High Entropy)

Imagine a completely random, choppy market. The probability of the price accelerating upward is exactly the same as it accelerating downward or simply drifting. Because all probabilities ($P(x_i)$) are equally distributed, the mathematical log sum of the equation is maximized (proof via Jensen's inequality). 

When Entropy maximizes, the market is entirely dominated by noise. Any standard breakout or trend-following signal generated in this environment is a statistical trap. Impulse-X reads this high entropy and warns you that signals will decay rapidly, suggesting a shift to mean-reversion tactics or staying flat.

### The "High Information" Trend (Low Entropy)

Conversely, when institutional capital steps in and initiates a true trend, the market's behavior becomes highly organized. The probability ($P(x_i)$) heavily concentrates into a single, predictable state (e.g., sustained positive velocity). Because the outcome is more certain, the Entropy drops significantly.

**The Practical Logic:** Think of Entropy like tuning an analog radio. 

* **High Entropy = Static:** The market is mean-reverting and choppy. You cannot trust the "broadcast" of price action.
* **Low Entropy = Clear Signal:** The noise has dropped out. The market is in a "High-Information" environment, meaning structural trends and momentum breakouts have a high mathematical probability of following through.

---

## 3. Hidden Markov Models (The Regime Decoder)

Standard indicators assume the market operates by a single set of continuous rules. In reality, financial markets are dynamic systems that abruptly shift between distinct structural "states" (e.g., low-volatility trending vs. high-volatility distribution). 

The problem? These underlying states are **hidden**—driven by latent variables like institutional liquidity and macroeconomic positioning. We cannot observe them directly; we can only observe the data they "emit" (price, velocity, and entropy).

To decode the market's true state, Impulse-X utilizes a **Hidden Markov Model (HMM)**. The HMM operates on three foundational probabilities to constantly update its "belief" about the market.



### 1. The Initial State Guess ($\pi$)

When you first boot Impulse-X, it cannot instantly know what the market is doing without historical context. It begins with an initial assumption vector. We hardcode the engine to start with a 100% belief that the market is in a **Neutral** state:
$$\pi = [1.0, 0.0, 0.0, 0.0, 0.0]$$
From this baseline, the engine begins absorbing real-time data to update its belief.

### 2. The Probability Transition Matrix ($A$)

Markets have *inertia*. A market in a strong bullish trend is mathematically highly likely to stay in that trend over the next bar, and highly unlikely to instantly snap into complete chaos in a single tick.

The HMM uses a $5 \times 5$ Transition Matrix ($A$) to define the "rules of movement" between regimes. Our proprietary matrix heavily weights self-transitions. For example, the matrix dictates that a "Bull Drift" state has a **90% probability** of remaining a Bull Drift, a 5% chance of fading to Neutral, and only a 1% chance of immediately jumping to Chaos.

$$
A = \begin{bmatrix}
P(N \to N) & P(N \to Bull) & \dots \\
P(Bull \to N) & P(Bull \to Bull) & \dots \\
\vdots & \vdots & \ddots
\end{bmatrix}
$$

### 3. The Emission Likelihood ($B$)

This is where our Kinematics and Entropy data plug in. The HMM asks: *"Given the Hidden State I think we are in, how likely is it that I would observe the current Velocity ($v$), Acceleration ($a$), Phase ($\phi$), and Entropy ($H$)? "*

The engine calculates an **Emission Likelihood** for each of the five states in real-time. 

* If the engine detects high positive Velocity ($v > 0$) and low Acceleration ($a \approx 0$), the likelihood of the "Bull Drift" state spikes.
* If Shannon Entropy breaches our dynamic threshold ($H > H_t$), the likelihood of the "High Chaos" state overrides all directional kinematics.
* If the Hilbert Transform dictates a negative phase angle ($\phi < 0$) alongside dropping velocity, the "Kinematic Dip" likelihood triggers.
  
  

### The Synthesis: Decoding the Market

On every new data point, Impulse-X updates its internal belief using the classic HMM Forward algorithm:

1. **Predict:** Multiply the current belief by the Transition Matrix ($A$) to predict where the market *should* go next based on inertia.
2. **Update:** Multiply that prediction by the new Emission Likelihood data to confirm or deny the prediction.
3. **Normalize:** Convert the result into a clean probability vector.

The engine then outputs the highest probability state directly to your terminal. 

### The Five Observable Regimes

Through this mathematical synthesis, the engine categorizes every market movement into one of five distinct regimes:

1. **Neutral:** Low momentum, average entropy. Awaiting institutional participation.
2. **Constant Velocity (Bull Drift):** Sustained momentum, low entropy. Ideal for trend-following.
3. **Kinematic Dip (Mean-Reverting):** Negative causal phase, dropping velocity. The math suggests a pullback within a broader structure.
4. **Kinetic Rip (Mean-Reverting):** Positive causal phase, spike in acceleration. An oversold bounce or local short-squeeze.
5. **High Chaos:** Shannon Entropy has maximized. Structural signals are breaking down. Cash is the safest position.
