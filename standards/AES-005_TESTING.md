# AES-005 — Testing Standard

Version: 1.0

Status: Active

---

# Purpose

Testing ensures that engineering changes remain reliable, reproducible, and safe.

Testing is mandatory.

---

# Testing Pyramid

1. Unit Tests
2. Integration Tests
3. System Tests
4. Simulation Tests
5. Paper Trading Validation

---

# Minimum Requirements

Every production feature should include:

- Functional validation
- Error handling
- Edge case testing
- Regression protection

---

# Trading Validation

Trading components require:

- Historical Backtesting
- Walk-Forward Validation
- Out-of-Sample Testing
- Monte Carlo Analysis (where applicable)
- Paper Trading

---

# Test Quality

Tests should be:

- Independent
- Deterministic
- Repeatable
- Fast
- Readable

---

# Continuous Integration

Every Pull Request should:

- Execute automated tests
- Validate documentation
- Check formatting
- Verify configuration

---

# Failure Handling

Failed tests block merging until resolved.

Temporary bypasses require documented justification.

---

# Quality Principle

Untested software is considered incomplete.