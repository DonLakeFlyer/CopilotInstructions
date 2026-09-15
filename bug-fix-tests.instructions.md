---
name: 'Bug Fix Tests'
description: 'Regression-test policy when fixing bugs: reproduce at the lowest level, extend existing tests before adding new ones, name tests after the rule, and report coverage.'
applyTo: '**'
---

# Regression tests when fixing a bug

Individual per-bug tests pile up and rot. Follow this procedure for every bug fix.

## 1. Reproduce at the lowest level that fails

Prefer, in order: a pure function → a single class → an integration round trip
(e.g. MockLink/Vehicle). Even if the bug was reported through the UI, push the
reproduction down until it no longer fails, then test one level above that.

## 2. Search before adding a test function

Before writing a new test slot, search the repository's test tree for an existing
test of the same invariant. If one exists, add a row to its data-driven table
(`_data()` / `QTest::addRow` in Qt) instead of a new function. Only add a new
function when no existing test covers the rule.

## 3. Name the test after the rule, not the bug

Test function names describe the invariant (`_panAnchorInvariant`,
`_lossyComponentNotDeclaredUnresponsive`). Issue or PR numbers go in the data row
tag or a one-line comment — never in the function name.

## 4. Not every fix earns a permanent test

Skip the test, and say so explicitly in the final reply, for: typos, QML property
or layout tweaks, dead code paths, and code slated for rewrite. Do not write a
test just to satisfy the process.

## 5. Report coverage after the fix is green

End the final reply with a short **Test coverage** block:

- Reproduction level used (pure function / class / integration) and why not lower.
- Existing test(s) checked for the same invariant, and whether a row was added.
- If a new function was added: does it duplicate existing coverage? If so, propose
  the merge.
- If no test was added: the explicit reason from rule 4.
