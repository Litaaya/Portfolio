---
title: "[Software][Unit][Integration] Testing"
date: 2026-08-02
draft: true
tags: ["software-testing", "unit-testing", "integration-testing"]
description: "An overview of software testing, unit testing, and integration testing"
---

> A big thank you to ChatGPT for generating the diagrams and examples, since I was too lazy to draw them myself in draw.io or Excalidraw =))

---

Software Testing, Unit Testing, and Integration Testing are three directly related concepts:

1. **Software Testing** is the general concept of testing software.
2. **Unit Testing** tests individual units of source code.
3. **Integration Testing** tests the interactions that occur when multiple units or modules are connected.

The testing process is commonly described in the following order:

```text
Unit Testing
    ↓
Integration Testing
    ↓
System Testing
    ↓
Acceptance Testing
```

---

# What is Software Testing?

**Software Testing** is the process of examining a software application or system to determine:

- Whether the actual results match the expected results.
- Whether the software satisfies the defined requirements.
- Whether the software contains defects, omissions, or unexpected behavior.
- Whether the product has sufficient quality, reliability, and safety for use.

Testing may involve executing a single component or the entire system to evaluate one or more properties of interest.

In simple terms, testing helps verify that software works as designed and reduces the risk of defects appearing in production.

However, testing cannot prove with absolute certainty that software is completely free of defects. It mainly helps:

- Detect existing defects.
- Reduce the probability of defects appearing in real environments.
- Provide information for assessing product quality and risk.

---

# Common causes of software defects

Software defects may come from many sources:

- Unclear or incomplete requirements.
- Misunderstood requirements.
- Inappropriate system design.
- Programming mistakes.
- Incorrect processing logic.
- Poor communication among team members.
- Requirement changes that are not fully updated.
- Incorrect integration between modules.
- Differences between the runtime and development environments.
- Unexpected input data.
- Changes in external systems or third-party services.
- Schedule pressure that causes testing activities to be skipped.

As software becomes more complex, relying only on developers to reread the code is usually not enough to identify every problem.

---

# Basic Software Testing methods

## Manual Testing

**Manual Testing** is a testing method in which a tester directly interacts with the application, enters data, observes the results, and compares them with the expected results without using automated scripts to execute the test.

Manual testing is suitable for:

- Exploratory testing.
- Usability testing.
- Ad-hoc testing.
- New features that change frequently.
- Situations that require human judgment and perception.

Limitations:

- It is time-consuming when tests must be repeated many times.
- It is more prone to human error.
- It is difficult to scale to a large number of test cases.
- It is unsuitable for continuous regression testing.

## Automation Testing

**Automation Testing** uses tools and scripts to:

- Execute tests automatically.
- Compare actual results with expected results.
- Record results.
- Report defects.
- Repeat tests across multiple environments or datasets.

Automation testing is especially suitable for:

- Regression testing.
- Frequently repeated tests.
- Testing with multiple datasets.
- Performance testing.
- CI/CD pipelines.
- Stable features with clearly defined results.

However, automation does not completely replace manual testing. Creating and maintaining automated scripts also requires time, cost, and technical skill.

---

# Main testing levels

## Unit Testing

Tests individual functions, methods, classes, or small modules.

## Integration Testing

Tests communication and interaction among multiple units or modules.

## System Testing

Tests the complete integrated system against functional and non-functional requirements.

## Acceptance Testing

Tests whether the product satisfies business needs and can be accepted by users or customers.

---

# What is Unit Testing?

**Unit Testing** is a testing method in which individual units or small components of source code are tested separately to confirm that they work correctly.

A unit may be:

- A function.
- A method.
- A class.
- A component.
- A small module.

The exact size of a unit depends on the software design. However, the most important principle is **isolation**.

When testing a unit, external dependencies such as databases, APIs, file systems, networks, email services, or message brokers should be replaced with mocks, stubs, or fakes so that the test focuses only on the logic of the unit being examined.

Example:

```python
def add(a, b):
    return a + b


def test_add():
    assert add(2, 3) == 5
```

The test confirms that the `add()` function returns the correct result when given `2` and `3`.

---

# Why is Unit Testing needed?

A common misconception is that skipping unit tests saves time. In reality, simple defects that are not detected at the unit level can become much harder to trace after modules have been integrated.

Unit testing provides the following benefits:

## Early defect detection

Defects are found close to the location where they were introduced, making the cause easier to identify and the fix faster to implement.

## Improving code quality

Code that is easy to unit test often has:

- Clear responsibilities.
- Fewer hidden dependencies.
- Smaller modules.
- A more maintainable structure.
- Lower coupling between components.

## Preventing regression defects

When refactoring or adding features, unit tests confirm that existing behavior still works.

## Increasing development speed

Automated unit tests provide fast feedback and reduce the time needed for manual testing and later debugging.

## Increasing confidence when changing code

Developers can modify or refactor code with less risk because the test suite acts as a safety net.

## Allowing early testing of individual parts

A module can be tested without waiting for the entire system to be completed.

## Acting as documentation

Unit tests show:

- What inputs a unit accepts.
- How the unit behaves.
- What results it returns.
- What behavior is expected in each situation.

New developers can read unit tests to understand how to use an API or component.

---

# Unit Testing process

## Step 1: Analyze the unit and define test cases

Identify the smallest behavior that needs to be tested and list:

- Happy paths: normal working cases.
- Edge cases: boundary situations.
- Error cases: failure situations.
- Inputs and outputs.
- Preconditions.
- Postconditions.

For example, for a function that divides two numbers, tests should cover:

- Dividing two positive numbers.
- Dividing negative numbers.
- Dividing by a decimal number.
- Dividing by zero.
- Invalid input.

## Step 2: Set up the test environment

Choose an appropriate testing framework and prepare the minimum required data.

Dependencies should be isolated using:

- Mocks.
- Stubs.
- Fakes.
- Fixtures.

The environment should remain lightweight so tests run quickly and are less likely to become unstable.

## Step 3: Write tests using the AAA Pattern

AAA consists of:

### Arrange

Prepare the object, input, test data, dependencies, and initial conditions.

### Act

Call the function or method being tested.

### Assert

Compare the actual result with the expected result.

```python
# Arrange
cart = Cart(tax_rate=0.1)

# Act
total = cart.total([Item("book", 100)])

# Assert
assert total == 110
```

Tests should focus on observable output behavior rather than depending too heavily on internal implementation details.

## Step 4: Run tests locally and in CI

First run the tests on the development machine. Then run them in a CI environment to ensure the code also works in a clean and consistent environment.

When a test fails, the logs should be clear enough for the developer to identify the cause quickly.

## Step 5: Analyze failures, fix defects, and refactor

When a test fails, determine whether:

- The production code is incorrect.
- The test case is incorrect.
- The requirement has changed.
- The test data is incorrect.
- The mocked dependency is inappropriate.

Avoid changing both the test and production code at the same time without first determining which one is wrong.

After the test passes, the developer can refactor while the test suite protects the existing behavior.

## Step 6: Rerun and maintain tests

After making a fix:

- Rerun the failed test.
- Run the entire test suite.
- Remove duplicate tests.
- Fix flaky tests.
- Optimize fixtures.
- Check coverage.
- Categorize slow tests so they can be run at an appropriate frequency.

Unit tests should be fast, independent, and named after the behavior they verify. A flaky test should be treated as a defect rather than simply ignored.

---

# Unit Testing techniques

## Black-box Testing

Testing based on:

- Inputs.
- Outputs.
- Functional requirements.
- Observable behavior.

The test writer does not necessarily need to know how the internal code is implemented.

## White-box Testing

Testing the internal structure and logic of the program, including:

- Branches.
- Conditions.
- Loops.
- Execution paths.
- Internal logic.

## Gray-box Testing

A combination of black-box and white-box testing. The tester has partial knowledge of the internal structure but still tests mainly from a behavioral perspective.

---

# Code Coverage in Unit Testing

Common types of coverage include:

- **Statement Coverage:** how many statements have been executed.
- **Decision Coverage:** how many logical decisions have been tested.
- **Branch Coverage:** how many `true` and `false` branches have been executed.
- **Condition Coverage:** how many subconditions have taken both true and false values.
- **Finite State Machine Coverage:** how many states and state transitions have been tested.

However, coverage is only an indicator used to identify untested code. High coverage does not automatically mean the tests are good.

A test may execute a line of code without asserting that the result is correct. Therefore, teams should not pursue 100% coverage at any cost.

Priority should be given to:

- Important business logic.
- Reusable components.
- High-risk code.
- Complex logic.
- Areas that change frequently.

---

# Mocks, Stubs, and Fakes in Unit Testing

## Why are Test Doubles needed?

Test doubles replace real dependencies to provide:

- **Isolation:** only the target unit is tested.
- **Determinism:** results remain stable and predictable.
- **Speed:** real databases or networks do not need to be called.
- **Edge-case simulation:** timeouts, exceptions, or unusual data can be simulated easily.

## Stub

A stub is a simple replacement object that returns predefined data.

Example:

```python
monkeypatch.setattr(
    "app.get_user_from_db",
    lambda _: {"id": 1, "name": "Alice"}
)
```

A stub mainly supplies data to the unit. It usually does not verify how the dependency was called.

Use a stub when only predefined data or a fake result is needed.

## Mock

A mock not only returns data but can also record and verify interactions.

It may verify:

- Whether an email-sending function was called.
- How many times it was called.
- Which arguments were passed.
- The order in which calls occurred.

Use a mock when interactions between the unit and a dependency must be verified.

## Fake

A fake is a simplified working implementation of a real dependency.

Examples:

- An in-memory database.
- A fake repository.
- A local file store.
- A fake message queue.

A fake provides real behavior without the full complexity of the production system.

General guideline:

```text
Need fixed data             → Stub
Need interaction checks     → Mock
Need a simple implementation → Fake
```

## Problems to avoid

- Mocking every dependency and making tests too dependent on implementation.
- Testing mocks instead of actual behavior.
- Creating overly long and difficult mock setups.
- Allowing tests to fail after refactoring even when behavior has not changed.
- Using mocks where a simple fake would be more suitable.

Mocks and stubs are tools for isolating a unit. They should not become the main focus of the test.

---

# Common Unit Testing tools

Popular frameworks include:

- **JUnit:** a unit testing framework for Java.
- **NUnit:** an open-source unit testing framework for .NET.
- **PHPUnit:** a unit testing framework for PHP.
- **pytest** or **unittest:** for Python.
- **Jest**, **Vitest**, or **Mocha:** for JavaScript.
- **xUnit** or **MSTest:** for .NET.
- **Go testing package:** for Go.
- **GoogleTest** or **Catch2:** for C++.

These frameworks usually provide:

- Assertions.
- Test runners.
- Setup and teardown.
- Fixtures.
- Parameterized testing.
- Mocking support.
- Test reports.
- Coverage integration.

---

# TDD and Unit Testing

**Test-Driven Development**, or TDD, is a method in which tests are written before production code.

The common cycle is:

```text
Red → Green → Refactor
```

## Red

Write a test for new behavior. The test fails because the feature has not yet been implemented.

## Green

Write the minimum amount of code needed to make the test pass.

## Refactor

Improve the code structure without changing its behavior. The test suite confirms that refactoring has not broken the feature.

TDD encourages:

- Small and testable units.
- Simple design.
- Incremental development.
- Avoiding unnecessary features.
- A regression test suite that grows with the codebase.

Unit testing is not the same as TDD. Unit tests may be written after production code. However, a unit-testing framework is an essential foundation for applying TDD.

---

# Unit Testing in CI/CD

When unit tests are integrated into a CI/CD pipeline, they become an automatic quality gate for every code change.

Benefits:

- Developers receive feedback almost immediately after committing code.
- Bugs are found before merge or release.
- A build is considered successful only when tests pass.
- Conflicts are reduced when multiple developers work simultaneously.
- Deployment becomes more reliable.
- Shift-left testing is supported by moving testing earlier in the development lifecycle.

A pipeline may perform:

```text
Checkout code
    ↓
Install dependencies
    ↓
Build
    ↓
Run unit tests
    ↓
Check coverage
    ↓
Run integration tests
    ↓
Package / Deploy
```

---

# Advantages of Unit Testing

- Detects defects early.
- Reduces the cost of fixing defects.
- Allows individual parts to be tested independently.
- Supports refactoring.
- Prevents regressions.
- Creates living documentation for the code.
- Improves software design.
- Provides faster feedback.
- Increases confidence before release.
- Can be automated and integrated into CI/CD.

---

# Limitations of Unit Testing

Unit testing cannot detect every defect.

It is not sufficient for fully detecting:

- Interaction defects between modules.
- Real database connection defects.
- Environment configuration defects.
- Real API communication defects.
- End-to-end defects.
- User interface defects.
- Whole-system performance problems.
- Business process defects that cross multiple components.

In addition:

- Every execution path in a complex system cannot always be tested.
- Writing and maintaining tests requires effort.
- Poorly designed tests may become brittle.
- Excessive mocking may create a false sense of confidence.
- High coverage does not guarantee that the product is defect-free.

Therefore, unit testing must be combined with integration testing, system testing, and other forms of testing.

---

# Unit Testing best practices

- Keep each test independent.
- Do not make tests depend on execution order.
- Make each test verify one main behavior.
- Name tests clearly according to the behavior being tested.
- Prepare only the minimum required test data.
- Avoid calling real networks or databases.
- Test happy paths, edge cases, and error cases.
- Fix defects found by unit tests before moving to later stages.
- Add a test when fixing a defect to prevent recurrence.
- Write tests alongside production code.
- Keep tests fast.
- Do not ignore flaky tests.
- Avoid over-mocking.
- Avoid depending too heavily on implementation details.
- Do not pursue 100% coverage when the tests provide little value.
- Prioritize important business logic and high-risk areas.

---

# What is Integration Testing?

**Integration Testing** is a testing level in which two or more units or modules that have already been unit tested are combined and tested as a group.

The main purpose is not to retest the internal logic of each module, but to test:

- Interfaces between modules.
- Data transferred from one module to another.
- Call order.
- Contracts between components.
- Communication with databases.
- Communication with APIs.
- Exception handling across modules.
- Coordination among multiple subsystems.

For example, an e-commerce system may contain:

- A login module.
- A shopping cart module.
- An order module.
- A payment module.
- An inventory module.
- An email module.

Each module may pass its unit tests, but the overall workflow may still fail if:

- The order module sends the wrong amount to the payment module.
- Payment succeeds but inventory is not updated.
- The order is saved but no email is sent.
- Two modules use different date or currency formats.

Integration testing is used to detect such defects.

---

# Why is Integration Testing needed?

Even when each module has been unit tested, defects may still appear when the modules are connected because:

- Different developers may build different modules.
- Modules may interpret interfaces differently.
- Data formats may be inconsistent.
- Data types may be incompatible.
- Calls may occur in the wrong order.
- API contracts may change.
- Exceptions may not be propagated or handled correctly.
- Modules may depend on timing, state, or transactions.
- External systems may respond differently from expectations.
- Database schemas may not match the code.
- Modules may work correctly in isolation but fail when coordinated.

Integration testing detects defects located at the connection points between components that work correctly on their own.

---

# Integration Testing example

Assume there are three modules:

```text
Login → Account → Transfer
```

Unit tests may confirm that:

- `LoginService` validates passwords correctly.
- `AccountService` retrieves the correct balance.
- `TransferService` calculates the transaction correctly.

An integration test must verify the complete interaction:

1. The user logs in.
2. A token is created.
3. The token is passed to Account Service.
4. Account Service returns the correct account.
5. Transfer Service checks the balance.
6. Money is deducted from the source account.
7. Money is added to the destination account.
8. The transaction is stored.
9. The status is returned to the user.

The defect may not exist inside a single unit, but in how the units use each other’s data.

---

# Integration Testing strategies

The four main approaches are:

1. Big Bang.
2. Top-Down.
3. Bottom-Up.
4. Sandwich or Hybrid.

## Big Bang Integration Testing

In Big Bang testing, all or most modules are integrated at the same time, and the complete combination is tested as one unit.

```text
Module A ┐
Module B ├── Integrated together → Test the complete integration
Module C ┤
Module D ┘
```

### Advantages

- Simple approach.
- No need to plan integration across multiple stages.
- Suitable for small systems.
- Can be performed once all modules are complete.

### Disadvantages

- It is difficult to identify the module causing a defect.
- Defects are detected relatively late.
- All modules must be completed first.
- Important interfaces may not be tested thoroughly.
- Debugging becomes complex when many modules are involved.
- It is unsuitable for large systems with many dependencies.

Big Bang is more suitable for small systems with few modules and relatively simple relationships among them.

## Incremental Integration Testing

In incremental testing, modules are integrated and tested step by step.

```text
A + B → Test
A + B + C → Test
A + B + C + D → Test
```

Advantages:

- Defects are found earlier.
- The defective area is easier to identify.
- Interfaces are tested systematically.
- There is no need to wait for the entire system to be completed.
- It is easier to manage than Big Bang.

Top-down, bottom-up, and sandwich are all forms of incremental integration.

## Top-Down Integration Testing

In top-down testing, integration begins with high-level modules and gradually adds lower-level modules.

```text
        Module A
        /      \
   Module B   Module C
      /           \
 Module D        Module E
```

The sequence may be:

```text
A
A + B
A + B + C
A + B + C + D
A + B + C + D + E
```

If a lower-level module is not ready, the tester uses a **stub** as a replacement.

### Stub in Top-Down testing

A stub simulates the behavior of a lower-level module.

For example, `OrderService` calls `PaymentService`, but `PaymentService` has not been completed. A stub may always return:

```json
{
  "status": "success"
}
```

### Advantages

- High-level control flow and logic are tested early.
- The main architecture is tested early.
- Design defects in important modules can be found early.
- A working prototype can be created relatively early.
- No drivers are required.

### Disadvantages

- Many stubs may need to be created.
- Lower-level modules are tested relatively late.
- Stubs may not accurately simulate real behavior.
- Detailed data-processing functions may not be tested thoroughly at the beginning.

Top-down is suitable when:

- High-level control flow is important.
- Higher-level modules are completed first.
- The architecture or main workflow must be tested early.

## Bottom-Up Integration Testing

In bottom-up testing, integration begins with lower-level modules and gradually connects them to higher-level modules.

```text
Module D + Module E
        ↓
     Module B
        ↓
     Module A
```

If a higher-level module is not ready, the tester uses a **driver** to call the lower-level modules.

### Driver in Bottom-Up testing

A driver is a temporary program that acts as a higher-level module.

It may:

- Send input to a module.
- Call a function.
- Receive output.
- Compare results.
- Simulate control flow from the upper layer.

### Advantages

- Foundational utilities and services are tested early.
- No stubs are required.
- Defects in lower-level modules are easier to detect.
- It is suitable when foundational modules are completed first.
- Detailed data processing can be tested early.

### Disadvantages

- Drivers must be created.
- High-level business flows are tested late.
- There is no complete system version at the beginning.
- Overall architectural defects may be detected late.

Bottom-up is suitable when:

- Lower-level modules are ready.
- Foundational data-processing logic is important.
- The system contains many shared services.
- Higher-level components are still being developed.

## Sandwich or Hybrid Integration Testing

Sandwich testing combines top-down and bottom-up testing.

Testing is performed simultaneously from:

- High-level modules downward.
- Low-level modules upward.

The two directions meet at the middle layer.

```text
Top modules
     ↓
Middle layer
     ↑
Bottom modules
```

This method uses both stubs and drivers.

### Advantages

- Combines the strengths of top-down and bottom-up.
- Allows parallel testing.
- Suitable for large multi-layer systems.
- High-level and low-level modules are tested early.
- May reduce the total integration time.

### Disadvantages

- Planning is more complex.
- More resources are required.
- Both stubs and drivers must be managed.
- The middle layer may be tested late.
- It is unsuitable for small, simple systems.

---

# Integration Testing process

## Step 1: Prepare the Integration Test Plan

Define:

- Which modules will be integrated.
- The integration order.
- The interfaces to test.
- The test scope.
- The environment.
- Dependencies.
- Tools.
- Entry criteria.
- Exit criteria.

## Step 2: Identify interfaces

List the communication points:

- Function calls.
- API endpoints.
- Databases.
- Message queues.
- File exchanges.
- Shared memory.
- Events.
- Authentication.
- Third-party services.

## Step 3: Prioritize important modules

Prioritize testing of:

- High-risk modules.
- Central modules.
- Interfaces used by many components.
- Payment.
- Authentication.
- Transactions.
- Data synchronization.

## Step 4: Design test cases

Test cases should cover:

- Valid data.
- Invalid data.
- Boundary values.
- Missing fields.
- Timeouts.
- Duplicate requests.
- Exceptions.
- Transaction rollback.
- Service unavailability.
- Incorrect formats.
- Incorrect API versions.
- Lost connections.
- Retry behavior.

## Step 5: Prepare test data and environment

An integration test environment is usually closer to production than a unit test environment and may include:

- A test database.
- A test API.
- Containers.
- Local services.
- A message broker.
- A third-party sandbox.
- Test credentials.
- Seed data.

## Step 6: Execute tests

Run the tests according to the selected strategy:

- Big Bang.
- Top-Down.
- Bottom-Up.
- Sandwich.

## Step 7: Record and fix defects

When a defect is found, determine whether:

- The sending module is incorrect.
- The receiving module misunderstood the data.
- The contract is inconsistent.
- The configuration is incorrect.
- The test data is incorrect.
- The external system is unstable.

## Step 8: Retest and perform Regression Testing

After fixing a defect:

- Rerun the failed test case.
- Run regression tests for related integrations.
- Confirm that the change has not broken other interfaces.

This process ensures that modules not only work individually but also coordinate correctly when connected.

---

# Important Integration Testing test cases

## Data contract

- Correct fields.
- Correct data types.
- Correct formats.
- Correct units.
- Correct time zones.
- Correct encoding.
- Correct versions.

## Error handling

- A service returns an error.
- The database is unavailable.
- A network timeout occurs.
- A message is invalid.
- A token expires.
- A request is rejected.
- One step in a transaction fails.

## State and transaction

- Data is committed correctly.
- Rollback works correctly after failure.
- Duplicate records are not created.
- Retry does not cause double processing.
- State remains consistent across modules.

## Interface performance

- Response time.
- Number of requests.
- Batch size.
- Connection pool.
- Queue backlog.

## Security

- Authentication.
- Authorization.
- Token propagation.
- Database access permissions.
- Sensitive data.
- Input validation.

---

# Advantages of Integration Testing

- Detects communication defects between modules.
- Confirms that components work together.
- Tests real data flow.
- Tests contracts and interfaces.
- Increases system reliability.
- Detects problems that unit testing cannot find.
- Verifies databases, APIs, and external services.
- Reduces risk before system testing.
- Supports testing workflows across multiple components.

---

# Limitations of Integration Testing

- It is slower than unit testing.
- The environment is more difficult to set up.
- External dependencies may make tests unstable.
- The cause of a failure is harder to identify than in unit testing.
- Test data is more complex.
- Stubs or drivers may be required.
- Maintenance costs are higher.
- Tests may be affected by networks, databases, or configuration.
- It does not replace system testing or acceptance testing.

---

# Unit Testing vs Integration Testing

| Criterion        | Unit Testing                    | Integration Testing                        |
|:-----------------|:--------------------------------|:-------------------------------------------|
| Scope            | One individual unit             | Multiple connected units or modules        |
| Goal             | Test internal logic             | Test interactions                          |
| Dependencies     | Usually mocked or stubbed       | Usually real or near-real dependencies     |
| Speed            | Very fast                       | Slower                                     |
| Performed by     | Mainly developers               | Developers or testers                      |
| Defects detected | Logic, conditions, calculations | Interfaces, contracts, data flow           |
| Environment      | Simple                          | More complex                               |
| Debugging        | Easier                          | More difficult                             |
| Quantity         | Usually very large              | Fewer than unit tests                      |
| Isolation        | High                            | Lower                                      |
| Example          | Test a sum function             | Test Order Service calling Payment Service |
| Position in SDLC | Earlier                         | After unit testing                         |

---

# Relationship among the three topics

Software testing is the overall concept, while unit testing and integration testing are two specific testing levels.

```text
SOFTWARE TESTING
│
├── Unit Testing
│   ├── Test functions
│   ├── Test methods
│   ├── Test classes
│   └── Isolate dependencies
│
├── Integration Testing
│   ├── Test interfaces
│   ├── Test data flow
│   ├── Test APIs or databases
│   └── Test interactions
│
├── System Testing
│   └── Test the complete system
│
└── Acceptance Testing
    └── Test user and business requirements
```

The first two levels complement each other:

- Passing unit tests does not mean the modules integrate correctly.
- Passing integration tests does not mean the internal logic of every unit has been tested thoroughly.
- Unit tests help locate defects quickly and accurately.
- Integration tests help locate defects at component boundaries.
- Both are necessary before testing the complete system.

---

# Complete example

Assume an order feature contains:

```text
OrderController
      ↓
OrderService
      ↓
InventoryService
      ↓
PaymentService
      ↓
OrderRepository
      ↓
NotificationService
```

## Unit Testing

Test each component separately:

- `OrderService` calculates the total correctly.
- `InventoryService` checks stock correctly.
- `PaymentService` rejects an invalid amount.
- `OrderRepository` maps objects correctly.
- `NotificationService` creates the correct email content.

Dependencies may be mocked.

## Integration Testing

Test coordination among components:

- Order Service reads inventory correctly.
- Payment Service receives the correct amount.
- A successful payment causes the order to be stored.
- A failed payment prevents the order from being confirmed.
- The database rolls back when inventory updating fails.
- Email is sent only after the order succeeds.
- Retry does not create two orders.
- The authentication token is passed correctly between services.

## System Testing

Test the complete system through the user interface:

1. The user logs in.
2. The user selects a product.
3. The user adds it to the cart.
4. The user makes a payment.
5. The user receives a confirmation.
6. The user checks the order history.

---

# Conclusion

**Software Testing** is the activity of evaluating and verifying software to detect defects, reduce risk, and ensure that the product meets requirements.

**Unit Testing** tests individual source-code units in isolation. It supports early defect detection, refactoring, regression prevention, and fast feedback. Unit tests should be independent, fast, behavior-focused, and integrated into CI/CD.

**Integration Testing** tests units or modules after they are connected. It focuses on interfaces, data flow, contracts, and interactions between components. The main approaches are Big Bang, Top-Down, Bottom-Up, and Sandwich.

A good testing strategy does not choose one or the other. It combines both:

```text
Many Unit Tests
      +
A reasonable number of Integration Tests
      +
A small number of important System or End-to-End Tests
```

Unit tests answer **whether each part works correctly**, while integration tests answer **whether the parts work correctly together**.
