# Line Balancing Optimization Tool (Operator Allocation)

This Gurobi model solves a classic IE problem:  
**How do you assign a fixed number of operators across sequential workstations with different task times to maximize throughput?**

I developed this tool during my internship to help evaluate staffing plans for a labor-intensive production line with 25 stations.
This project was built using my **academic Gurobi license**, as MSA did not have one available.  
By leveraging optimization modeling instead of manual spreadsheets, it replaced hours of Excel guesswork with a mathematically optimal allocation — making staffing recommendations both faster to generate and easier to justify.

---

## Problem Setup

Each station has a known average **cycle time** (in minutes). You have a fixed number of operators (ranging from 28 to 34), and need to decide how many to assign to each station.

The model tests three allocation strategies:
1. **Integer Allocation** – realistic, only whole operators per station.
2. **Half-Operator Allocation** – simulates floaters who split time between two stations.
3. **Fractional Allocation** – idealized, lets you assign any fraction of an operator (not physically possible, but useful as a benchmark).

Each scenario is solved as a Gurobi optimization model with the goal of **maximizing hourly throughput**.

---

## Core Constraint

For every station `i`: 60 * ops[i] ≥ mu * cycle_times[i]

Where:
- `ops[i]` = number of operators at station i (integer, half, or fractional depending on the scenario),
- `mu` = units per hour (throughput),
- `cycle_times[i]` = task time (min/unit) for station i.

Total operator count is constrained to the value being tested (e.g., 28, 29, ..., 34). The model optimizes `mu`.

---

## Output

For each operator count and strategy, the model prints:
- 📈 **Max throughput** (units/hour)
- 📦 **Expected daily output** (based on a 9-hour shift)
- ⚙️ **Adjusted daily output** (assumes 28% PF&D losses)
- 🔍 **Operators per station**
- ❗ **Top 3 bottleneck stations**

---

## Sample Use Case

Helps answer questions like:
- “What’s the theoretical max output we could hit?”
- “How much benefit do floaters give us?”
- “Which stations are limiting our line speed?”
- “How does daily output change as we remove operators?”

---

## Assumptions

- Tasks are sequential (no parallelism or precedence logic).
- Each station must complete every unit (no skipped stations).
- No individual operator preferences or skills are enforced.
- Cycle times are fixed averages.
- PF&D factor is applied as a flat 28% adjustment.

---

# Disclaimer

This repository and all content within are intended solely for educational and demonstration purposes. All files and mockups utilize dummy data and do not reflect MSA's confidential data or station assignments. 

