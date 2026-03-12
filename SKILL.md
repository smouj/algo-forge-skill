title: "Algo Forge Skill"
description: "A specialized OpenClaw skill for crafting custom algorithms from natural language descriptions, automating code generation for complex computational tasks."
author: "OpenClaw Team"
version: "1.2.0"
tags: ["algorithms", "generation", "automation", "code", "nlp", "optimization"]
category: "Development Tools"
requires: ["python>=3.8", "openai-api", "numpy", "scipy"]
env_vars: ["OPENAI_API_KEY", "ALGO_FORGE_MODEL=gpt-4-turbo"]
---

# Algo Forge Skill

## Purpose

Algo Forge is designed to bridge the gap between human-described computational problems and executable algorithms. It excels at transforming natural language specifications into optimized, production-ready code across multiple programming languages (Python, JavaScript, C++, Rust). Key real-world use cases include:

- **Financial Modeling**: Generate Monte Carlo simulation algorithms for portfolio risk assessment from descriptions like "simulate 10,000 paths of stock prices using geometric Brownian motion with volatility 0.2 and drift 0.05".
- **Data Processing Pipelines**: Create ETL algorithms for processing large datasets, such as "extract JSON logs from S3, filter by timestamp and user ID, aggregate metrics by hour, and output to PostgreSQL".
- **Machine Learning Preprocessing**: Automate feature engineering scripts, e.g., "normalize numerical features using z-score, one-hot encode categorical variables with less than 10 unique values, and handle missing values with median imputation".
- **Optimization Problems**: Solve combinatorial challenges like "implement a genetic algorithm to optimize warehouse layout minimizing travel distance for 50 products and 10 aisles".
- **Real-time Analytics**: Build streaming algorithms for "compute sliding window averages of sensor data over 5-minute intervals with exponential decay weighting".

## Scope

Algo Forge operates via command-line interface integration with OpenClaw, focusing on algorithm generation and refinement. Supported commands include:

- `/algo-forge generate <description>`: Creates initial algorithm from natural language prompt
- `/algo-forge refine <file> --optimize=performance`: Improves existing algorithm with specific optimization flags (performance, memory, readability)
- `/algo-forge test <file> --input=data.json --output=results.csv`: Runs algorithm against test data and validates outputs
- `/algo-forge benchmark <file> --metrics=time,space --iterations=100`: Benchmarks algorithm performance across multiple runs
- `/algo-forge document <file> --format=markdown`: Generates comprehensive documentation with complexity analysis
- `/algo-forge migrate <file> --to=javascript`: Translates algorithm to different language while preserving logic

Dependencies: Requires OpenAI API access, NumPy for numerical computations, and SciPy for scientific algorithms. Environment must have ALGO_FORGE_MODEL set to preferred GPT model.

## Work Process

Algo Forge follows a structured 5-step workflow for reliable algorithm generation:

1. **Prompt Analysis**: Parse natural language description using NLP to identify problem type (search, sort, optimization, simulation), constraints (time/space complexity, data types), and requirements (inputs/outputs, edge cases).
2. **Skeleton Generation**: Create initial code structure with placeholder functions, imports, and basic logic flow using GPT-4-turbo.
3. **Optimization Phase**: Apply domain-specific optimizations (e.g., memoization for recursive algorithms, vectorization for array operations) based on identified patterns.
4. **Testing Integration**: Embed unit tests and validation checks automatically, using sample inputs to ensure correctness.
5. **Documentation and Export**: Generate README.md with usage examples, complexity analysis (Big O notation), and deployment instructions.

Each step includes verification: Prompt analysis validates input completeness, skeleton generation checks syntax, optimization measures improvements, testing ensures 95%+ pass rate, and documentation includes runnable examples.

## Golden Rules

- **Clarity First**: Always prioritize readable, well-commented code over premature optimization. Use descriptive variable names and modular functions.
- **Constraint Awareness**: Respect specified time/space limits; if O(n²) requested, do not generate O(n³) solutions unless explicitly allowed.
- **Error Handling**: Include try-catch blocks for all I/O operations and validate inputs against expected types/ranges.
- **No Magic Numbers**: Parameterize all constants (e.g., array sizes, thresholds) and provide configuration options.
- **Language Idioms**: Generate code that follows language-specific best practices (e.g., list comprehensions in Python, async/await in JS).
- **Ethical Generation**: Refuse prompts requesting algorithms for illegal activities, data manipulation, or harmful simulations.
- **Version Control**: All generated code includes gitignore for sensitive files and commit messages describing changes.

## Examples

### Example 1: Financial Simulation
**Input Prompt:**  
`/algo-forge generate "Create a Python function to simulate Black-Scholes option pricing for European call options with parameters: spot price 100, strike 105, volatility 0.2, risk-free rate 0.05, time to expiry 1 year. Include Monte Carlo estimation with 10,000 paths."`

**Generated Output (algo_black_scholes.py):**
```python
import numpy as np
from scipy.stats import norm

def black_scholes_call(S, K, T, r, sigma, paths=10000):
    """
    Monte Carlo simulation for European call option pricing using Black-Scholes model.
    
    Args:
        S (float): Spot price
        K (float): Strike price  
        T (float): Time to expiry (years)
        r (float): Risk-free rate
        sigma (float): Volatility
        paths (int): Number of simulation paths
    
    Returns:
        float: Estimated call option price
    """
    np.random.seed(42)  # For reproducibility
    dt = T / 252  # Daily time steps
    drift = (r - 0.5 * sigma**2) * dt
    diffusion = sigma * np.sqrt(dt)
    
    # Simulate price paths
    prices = np.zeros(paths)
    for i in range(paths):
        price_path = S
        for _ in range(252):
            price_path *= np.exp(drift + diffusion * np.random.normal())
        prices[i] = max(price_path - K, 0)
    
    return np.mean(prices) * np.exp(-r * T)

# Example usage
if __name__ == "__main__":
    price = black_scholes_call(100, 105, 1, 0.05, 0.2)
    print(f"Estimated call price: ${price:.2f}")
```

**Verification:** Run `python algo_black_scholes.py` - should output ~$8.02 (approximates analytical Black-Scholes value).

### Example 2: Data Pipeline Optimization
**Input Prompt:**  
`/algo-forge refine data_pipeline.py --optimize=performance`

**Before (Original):**
```python
def process_data(data):
    result = []
    for item in data:
        if item['status'] == 'active':
            result.append(item['value'] * 2)
    return result
```

**After (Optimized):**
```python
import numpy as np

def process_data(data):
    """
    Optimized data processing using vectorized operations.
    Filters active items and doubles values.
    """
    values = np.array([item['value'] for item in data if item['status'] == 'active'])
    return (values * 2).tolist()
```

**Performance Benchmark:** `time python -c "import data_pipeline; data_pipeline.process_data([{'value': i, 'status': 'active' if i%2 else 'inactive'} for i in range(100000)])"` - Vectorized version runs 50x faster on large datasets.

### Example 3: Algorithm Translation
**Input Prompt:**  
`/algo-forge migrate fibonacci.py --to=javascript`

**Python Input:**
```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

**JavaScript Output:**
```javascript
function fibonacci(n) {
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// Example usage with memoization optimization
const memo = new Map();
function optimizedFibonacci(n) {
    if (memo.has(n)) return memo.get(n);
    if (n <= 1) return n;
    const result = optimizedFibonacci(n - 1) + optimizedFibonacci(n - 2);
    memo.set(n, result);
    return result;
}
```

## Rollback Commands

- **Revert Generation:** `/algo-forge rollback generate <session_id>` - Removes all files created in the last generation session and restores workspace to pre-generation state.
- **Undo Refinement:** `/algo-forge rollback refine <file> --version=previous` - Reverts file to version before last optimization, preserving git history.
- **Reset Benchmark:** `/algo-forge rollback benchmark <file>` - Deletes benchmark logs and temporary test files generated during performance analysis.
- **Clear Cache:** `/algo-forge rollback cache` - Purges all cached algorithms and prompts, forcing fresh generation on next run.

## Dependencies and Requirements

- **Python 3.8+** with NumPy 1.21+, SciPy 1.7+
- **OpenAI API Key** with sufficient credits for GPT-4-turbo calls (estimated $0.02 per complex algorithm)
- **Environment Variables:** 
  - `OPENAI_API_KEY`: Your OpenAI API key
  - `ALGO_FORGE_MODEL`: Default "gpt-4-turbo" (can override to "gpt-3.5-turbo" for cost savings)
  - `ALGO_FORGE_CACHE_DIR`: Optional custom cache directory (defaults to ~/.openclaw/algo-forge-cache)
- **System Requirements:** 4GB RAM minimum, 2GB disk space for cache

## Verification Steps

1. **Syntax Check:** Run `python -m py_compile <generated_file>` or equivalent for target language.
2. **Unit Tests:** Execute embedded tests with `pytest <generated_file>` (Algo Forge includes 5-10 test cases automatically).
3. **Performance Validation:** Use `/algo-forge benchmark` to ensure meets specified complexity bounds.
4. **Output Consistency:** Compare generated outputs against expected results for sample inputs.
5. **Integration Test:** Run in target environment and verify no runtime errors.

## Troubleshooting

- **API Rate Limits:** Error "429 Too Many Requests" - Wait 60 seconds or switch to cheaper model.
- **Memory Issues:** Large simulations fail - Reduce paths/iterations or use streaming approach.
- **Incorrect Logic:** Algorithm doesn't match description - Refine prompt with more specific constraints.
- **Language Errors:** Migration fails - Ensure target language runtime is installed and compatible.
- **Performance Degradation:** Optimizations not effective - Check data types; consider parallel processing for CPU-bound tasks.

## Changelog

- v1.2.0: Added multi-language migration, improved error handling
- v1.1.0: Integrated benchmarking suite, enhanced NLP parsing
- v1.0.0: Initial release with core generation capabilities