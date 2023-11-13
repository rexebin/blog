# Understanding Unit Test From The Best Book On The Topic

## Introduction

Learning to write good and effective unit tests is very hard. Because of it, I used to lie to myself that if I follow best practices, adopt good software architecture and patterns, break code into manageable and straightforward code pieces, I don't need tests and could produce the same quality software. 

Writing good tests is the only thing that can lead us to best practices, to suitable architectures and patterns.

I now religiously believe that it is a good test suite that defines the quality of any software. What is a good test? To answer this question, we must break the question into two questions: What is a unit test? What makes a test a good test?

There are many answers out there. The best answers are below, from the book "UNIT TESTING: PRINCIPLES, PRACTICES, AND PATTERNS" by Vladimir Khorikov.

## What is a unit test?

A unit test is a test that
* Verifies a single unit of behaviour,
* Does it quickly,
* And in isolation from other tests.

## What makes a test good?

A good unit test has the following four attributes
* Protection against regressions
* Resistance to refactoring
* Fast feedback
* Maintainability

There we have it. A unit test verifies a single unit of behaviour, not a single piece of code, not a single class, not a single function etc., but a single unit of behaviour.

## How to write good tests?
To try to write good tests, we must be mindful of the following:

1. Try to get as close to an actual business use case as possible and as far as possible from implementation details. Mimic user actions and assert user expectations. Never assert what the code should do internally. It is worth to mention that for JavaScript, the library `@testing-library` embraces and enforces this principle.

2. Isolate business logic to pure functions/methods (without side effects, given the same input, they always return the same output). Use pure functions anywhere when possible because Input/Output based tests are the best tests.

3. If one behaviour is too complicated to test, break it up. Ok, I tried my best, but it is still not easy to test? There is more room for improvement. Keep trying until they are easily testable.

4. Only mock external unmanaged dependencies if possible. If not, do not assert against managed dependencies

5. If it's not a behaviour, don't test it.

## Warning! Bad tests are worse than no tests!

If tests verify a single piece of code and assert what they do internally, they are pinning down the implementations. It is the worst thing that could happen to a project. They don't help deliver what the software is supposed to do for its users. They make the code base impossible to refactor hance hinder the team's ability to improve the software and add/change features. It is a waste of time to write them. It is worse than no tests at all.