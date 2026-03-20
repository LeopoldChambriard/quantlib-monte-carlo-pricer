cat << 'EOF' > README.md
# QuantLib Monte Carlo Derivative Pricing Engine

High-performance C++ pricing library extending **QuantLib** to price vanilla and path-dependent exotic options using Monte Carlo simulations. The project features custom pricing engines, path generators, and numerical validation against analytical benchmarks under geometric Brownian motion.

## Implemented Pricing Engines

### 1. Constant Black-Scholes Process (`ConstantBlackScholesProcess`)
Extends QuantLib's `GeneralizedBlackScholesProcess` with constant market parameters (risk-free rate $r$, dividend yield $q$, volatility $\sigma$), optimizing path generation efficiency.

### 2. Discrete Arithmetic Average Strike Asian Engine (`MCDiscreteArithmeticAverageStrikeEngine`)
* **Payoff**:
  $$\text{Call} = \max\left(S_T - \frac{1}{N}\sum_{i=1}^N S_{t_i}, 0\right), \quad \text{Put} = \max\left(\frac{1}{N}\sum_{i=1}^N S_{t_i} - S_T, 0\right)$$
* Handles discrete observation schedules over the lifespan of the option.

### 3. Lookback Option Engine (`MCLookbackEngine`)
* Evaluates path-dependent options whose payoff relies on the realized maximum or minimum price over the trajectory:
  $$\text{Floating Strike Call} = \max(S_T - \min_{0 \le t \le T} S_t, 0)$$
  $$\text{Fixed Strike Call} = \max(\max_{0 \le t \le T} S_t - K, 0)$$

### 4. European Option Monte Carlo Engine (`MCEuropeanEngine`)
* Benchmarking engine used to validate Monte Carlo path diffusion, sample variance, and confidence intervals against analytical closed-form Black-Scholes formulas.

## Project Structure

```text
├── constantblackscholesprocess.hpp / .cpp  # Custom constant parameters 1D diffusion process
├── mceuropeanengine.hpp                   # Monte Carlo engine for standard European options
├── mclookbackengine.hpp                   # Path-dependent Lookback option Monte Carlo engine
├── mc_discr_arith_av_strike.hpp           # Discrete arithmetic average strike Asian engine
├── main.cpp                               # Pricing benchmarks, test cases & convergence checks
├── Makefile                               # Build configuration linking QuantLib
├── _clang-format                          # Code formatting configuration
└── build.yml                              # Continuous integration workflow