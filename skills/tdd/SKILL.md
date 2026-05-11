---
name: tdd
description: >-
  Test-driven development with red-green-refactor loop based on local Obsidian task files. Use when user wants to build features or fix bugs using TDD based on a specific [[task-note]] from the .docs/tasks/ folder.
---
# Test-Driven Development

## Philosophy

**Core principle**: Tests should verify behavior through public interfaces, not implementation details. Code can change entirely; tests shouldn't.

**Good tests** are integration-style: they exercise real code paths through public APIs. They describe _what_ the system does, not _how_ it does it. A good test reads like a specification - "user can checkout with valid cart" tells you exactly what capability exists. These tests survive refactors because they don't care about internal structure.

**Bad tests** are coupled to implementation. They mock internal collaborators, test private methods, or verify through external means (like querying a database directly instead of using the interface). The warning sign: your test breaks when you refactor, but behavior hasn't changed. If you rename an internal function and tests fail, those tests were testing implementation, not behavior.

Mock only external layers outside the subject's relevant context. Every test treats the component, hook, business logic, or other code under test as a black box and verifies only observable behavior.

```typescript
jest.mock("@/api/orders", () => ({ createOrder: jest.fn() }));
jest.mock("@/hooks/useCurrentUser", () => ({ useCurrentUser: () => ({ id: "user-1" }) }));
jest.mock("@stripe/stripe-js", () => ({ loadStripe: jest.fn() }));
jest.mock("@/utils/toast", () => ({ toast: { error: jest.fn() } }));

test("submits a plan and shows confirmation", async () => {
  createOrder.mockResolvedValue({ id: "order-1" });

  render(<CheckoutPlan/>);
  await user.click(screen.getByRole("radio", { name: /pro/i }));
  await user.click(screen.getByRole("button", { name: /checkout/i }));

  expect(await screen.findByText(/order confirmed/i)).toBeVisible();
  expect(createOrder).toHaveBeenCalledWith({ plan: "pro", userId: "user-1" });
});

test("shows a recoverable error when checkout fails", async () => {
  createOrder.mockRejectedValue(new Error("network"));

  render(<CheckoutPlan/>);
  await user.click(screen.getByRole("button", { name: /checkout/i }));

  expect(await screen.findByRole("alert")).toHaveTextContent(/try again/i);
});
```

## Anti-Pattern: Horizontal Slices

**DO NOT write all tests first, then all implementation.** This is "horizontal slicing" - treating RED as "write all tests" and GREEN as "write all code."

This produces **crap tests**:

- Tests written in bulk test _imagined_ behavior, not _actual_ behavior
- You end up testing the _shape_ of things (data structures, function signatures) rather than user-facing behavior
- Tests become insensitive to real changes - they pass when behavior breaks, fail when behavior is fine
- You outrun your headlights, committing to test structure before understanding the implementation

**Correct approach**: Vertical slices via tracer bullets. One test → one implementation → repeat. Each test responds to what you learned from the previous cycle. Because you just wrote the code, you know exactly what behavior matters and how to verify it.

```
WRONG (horizontal):
  RED:   test1, test2, test3, test4, test5
  GREEN: impl1, impl2, impl3, impl4, impl5

RIGHT (vertical):
  RED→GREEN: test1→impl1
  RED→GREEN: test2→impl2
  RED→GREEN: test3→impl3
  ...
```

## Workflow

### 1. Planning & Context

**Context Check:**
- If the user invoked this skill with a specific target (e.g., `/tdd build the login button`), focus ONLY on that explicit request and skip reading the vault tasks.
- If the user invoked this skill alone (e.g., just `/tdd`), default to the local Obsidian vault: read the relevant `[[task-note]]` from the `.docs/tasks/` folder and any linked inspirations.
- If `.docs/tasks/` does not exist, ask the user where task notes are stored before proceeding.

Before writing any code:
- [ ] Confirm with the user which behaviors to test first (based on their explicit prompt OR the vault task).
- [ ] List the behaviors to test (not implementation steps).
- [ ] Get user approval on the plan.

Ask: "Which behaviors are most important to test?"

**You can't test everything.** Confirm with the user exactly which behaviors matter most. Focus testing effort on critical paths and complex logic, not every possible edge case.

### 2. Scan Existing Tests First

**Before writing any new test**, scan the co-located test file (and related test files) for tests that already cover or partially cover the target behavior:

- Read the existing test file in full.
- Identify tests that are GREEN but would need to change to reflect new or modified behavior — these are your adapt candidates.
- Identify tests that are already RED for the right reason — leave them; they are your tracer bullet.
- **Only create a new test if no existing test can be adapted.**

Prefer adapting over adding: a test suite that grows unbounded with every feature becomes noise. Adjust an existing test's setup, assertions, or description to match the new behavior instead of duplicating coverage.

### 3. Tracer Bullet

Write (or adapt) ONE test that confirms ONE thing about the system:

```
RED:   Write/adapt test for first behavior → test fails
GREEN: Write minimal code to pass → test passes
```

This is your tracer bullet - proves the path works end-to-end.

### 4. Incremental Loop

For each remaining behavior:

```
SCAN:  Check existing tests for adapt candidates
RED:   Write/adapt next test → fails
GREEN: Minimal code to pass → passes
UPDATE: Mark the relevant `[x]` Acceptance criteria in the local `.docs/tasks/` markdown file.
```

Rules:

- One test at a time
- Scan before adding — adapt an existing test when the behavior it covers has changed
- Only enough code to pass current test
- Don't anticipate future tests
- Keep tests focused on observable behavior
- Update the local `.docs/tasks/` markdown file as criteria are met.

### 5. Refactor

After all tests pass, look for:

- [ ] Extract duplication
- [ ] Deepen modules (move complexity behind simple interfaces)
- [ ] Apply SOLID principles where natural
- [ ] Consider what new code reveals about existing code
- [ ] Run tests after each refactor step

**Never refactor while RED.** Get to GREEN first.

## Checklist Per Cycle

```
[ ] Confirmed vault task path exists or asked the user for location
[ ] Read the task and linked inspirations from the local Obsidian Vault
[ ] Existing tests scanned before writing a new one
[ ] Adapted existing test when behavior has changed, not added a duplicate
[ ] Test describes behavior, not implementation
[ ] Test uses public interface only
[ ] Test would survive internal refactor
[ ] Code is minimal for this test
[ ] Updated the Acceptance Criteria checklist in the local Obsidian task file
[ ] No speculative features added
```
