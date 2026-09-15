---
name: 'Test Policy'
description: 'Test policy for all code changes: test at the lowest level, extend existing tests before adding new ones, name tests after the invariant, test-first for new code, reproduce-first for bug fixes, and report coverage.'
applyTo: '**'
---

# Test policy

Tests pile up and rot when each change adds its own. Follow this for every change
that touches behavior — new code, bug fixes, refactors.

## Shared rules (all changes)

1. **Test at the lowest level that exercises the behavior.** Prefer, in order: a
   pure function → a single class → an integration round trip (e.g. MockLink/Vehicle).
   Push the test down until it can go no lower, then test one level above that.
2. **Search before adding a test function.** Before writing a new test slot, search
   the repository's test tree for an existing test of the same invariant. If one
   exists, add a row to its data-driven table (`_data()` / `QTest::addRow` in Qt)
   instead of a new function. Only add a new function when no existing test covers
   the rule.
3. **Name the test after the invariant, not the change.** Function names describe the
   rule (`_panAnchorInvariant`, `_lossyComponentNotDeclaredUnresponsive`). Issue or
   PR numbers go in the data row tag or a one-line comment — never in the function name.
4. **Not every change earns a permanent test.** Skip the test, and say so explicitly
   in the final reply, for: typos, QML property or layout tweaks, dead code paths,
   and code slated for rewrite. Do not write a test just to satisfy the process.
5. **One test per contract, one reason to fail.** Cover boundaries and failure paths
   over repeated happy-path variations. Assert observable behavior, not private state
   or call order.

## Bug fixes: reproduce first

Write a failing test that reproduces the bug at the lowest level (rule 1) *before*
changing production code. Even if the bug was reported through the UI, push the
reproduction down until it no longer fails. The fix is done when that test is green
and no other test went red.

## New code: test-first

Write the test for the intended behavior before or alongside the implementation, not
after. Shape the API so the lowest-level test is possible — pure functions and
injectable dependencies over designs that can only be exercised through an
integration round trip. If a contract cannot be tested below the integration level,
say so and explain why.

## Report coverage when the change is green

End the final reply with a short **Test coverage** block:

- Test level used (pure function / class / integration) and why not lower.
- Existing test(s) checked for the same invariant, and whether a row was added.
- If a new function was added: does it duplicate existing coverage? If so, propose
  the merge.
- For new code: which contracts are covered and which were deliberately left untested.
- If no test was added: the explicit reason from rule 4.
