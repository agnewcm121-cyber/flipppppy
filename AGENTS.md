# Goal

Investigate whether direct SPL token transfers into reserve vaults can desynchronize pool state and cause:
1. global sell failure,
2. zero-payment buys,
3. a net-profitable drain or unrecoverable two-sided stall.

# Repo workflow

- Use the existing Rust integration harness in `program/tests/integration.rs`.
- Prefer minimal new helpers in `program/tests/utils/`.
- Do not refactor unrelated code.
- First add a test for direct BBB transfer into `vault_a` causing sell failure.
- Second add a test for direct USDF transfer into `vault_b` causing zero-payment or undercharged buy behavior.
- Third, only if both primitives are reproduced, add a balance-ledger test for:
  direct USDF topup -> buy -> sell-back -> optional burn_fees
- Print before/after balances for attacker, vault_a, vault_b, and fees_accumulated.
- Keep changes small and compile/test after each step.

# Acceptance criteria

A finding is only "critical" if the test proves one of:
- net attacker profit after counting all external topups, or
- permanent two-sided stall with no public recovery path.

Otherwise, label it as a real state-desync bug but not a confirmed critical.
