# Black-Box API — Usage Guide

## Overview
The PI provides access to a query API for proof validation.
You do NOT need to know how the system works internally.
You only need the mathematical specification (specs/SYSTEM_SPECIFICATION.tex)
and the API responses.

## Endpoints

### 1. query(text)
**Input:** Raw text string
**Output:**
```json
{
  "positions": [
    {
      "t": 0,
      "selected_modules": [3, 7, 22, 35],
      "weights": [0.45, 0.28, 0.18, 0.09],
      "loss": 2.34
    },
    ...
  ],
  "mean_loss": 3.12,
  "perplexity": 48.7
}
```

### 2. ablate(text, remove_ids)
**Input:** Text + list of module IDs to exclude
**Output:**
```json
{
  "mean_loss": 3.45,
  "perplexity": 52.1,
  "modules_used": 33
}
```

### 3. attribute(text)
**Input:** Text string
**Output:** Per-position contribution vectors c_i(t) for all active modules.
```json
{
  "positions": [
    {
      "t": 0,
      "contributions": {
        "3": [0.12, -0.05, ...],
        "7": [0.08, 0.11, ...],
        "22": [0.03, -0.01, ...],
        "35": [0.01, 0.02, ...]
      }
    },
    ...
  ]
}
```

## Access
API access is provided by the PI on a per-experiment basis.
Submit your query scripts for review before running large experiments.
