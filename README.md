# Nim Game Strategies (Rule-Based + Evolutionary) — Python

This project implements the classic **Nim** game and compares multiple strategies, including:
- **Random play**
- A simple **greedy baseline**
- An **optimal nim-sum–based strategy**
- A custom **rule-based nim-sum agent**
- An **evolved strategy** whose behavior is tuned via a lightweight evolutionary loop


---

## What is Nim?

Nim is a two-player impartial game with multiple rows (heaps) of objects. On each turn, a player chooses one row and removes one or more objects from it (optionally up to a maximum `k`). The player who takes the last object wins (normal play).

---

## Main components

### 1) Core game representation
- **`Nim` class**
  - Initializes a Nim state with `num_rows`, where rows are `[1, 3, 5, ...]`
  - Optional constraint `k` limits the maximum number of objects removable per move
  - `nimming(ply)` applies a move after validating it

- **`Nimply`**
  - A namedtuple move representation: `(row, num_objects)` 

---

## Strategies included

### Baselines
- **`pure_random(state)`**
  - Selects a random non-empty row and removes a random valid number of objects.

- **`gabriele(state)`**
  - Chooses a move that removes as many as possible from the lowest available row (a deterministic greedy heuristic). :contentReference[oaicite:3]{index=3}

### Theory-based (nim-sum)
- **`nim_sum(state)`**
  - Computes the XOR-based nim-sum of the heap sizes (implemented via binary representation + parity).

- **`optimal(state)`**
  - Analyses all possible moves and selects a “spicy” move (in this code: prefers resulting states with nim-sum ≠ 0; otherwise picks any move). :contentReference[oaicite:4]{index=4}

> Note: In standard Nim theory, the winning objective is usually to move to a state with **nim-sum = 0** when possible. This project also includes a direct rule-based implementation of that idea (below). :contentReference[oaicite:5]{index=5}

### Custom tasks (your implementations)

#### Task 2.1 — Rule-based nim-sum strategy
- **`rule_based_nim_sum(state)`**
  - If current nim-sum is **0**, it falls back to random (no forced win).
  - Otherwise, it searches for a legal move that makes the resulting nim-sum **0**.
  - Respects the optional `k` constraint. :contentReference[oaicite:6]{index=6}

#### Task 2.2 — Evolve a strategy
This section builds an “evolved” agent by tuning a single parameter:
- **`fitness(...)`**
  - Plays repeated games vs an opponent (default: `rule_based_nim_sum`) and counts wins.
- **`gaussian_mutation(...)`**
  - Adds Gaussian noise to strategy parameters.
- **`select_top_strategies(...)`**
  - Selects the best strategies by fitness.
- **`evolve_strategy(...)`**
  - Runs a simple evolutionary loop across generations to find a good parameter setting.
- **`evolved_strategy(state, param)`**
  - Uses the parameter as a bias knob (mapped to `[0, 1]`) and chooses moves using nim-sum logic. :contentReference[oaicite:7]{index=7}

---

## How to run

### Requirements
- Python 3.8+
- `numpy`

Install:
```bash
pip install numpy
