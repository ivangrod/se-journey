---
title: "Code Review"
date: 2020-04-13T18:00:20+02:00
---

> Code review is a human communication process

## 3-steps process

1. The developer who wrote the code creates a pull request (PR) and lets the team know that the PR was created. (**Slack bot**)
   
2. Once the PR is created, someone else starts reviewing the code, in accordance with a [guide](#guide-for-an-effective-code-review) providing rules, standards and processes.
   
3. After one or two developers have reviewed the code, one or both steps should be followed:

    * The code needs improvement. Then, the developer could either start a discussion regarding the best way to solve the problem, or just follow the suggestions. This step is repeated until all developers are involved and agree on the fact that the code is good enough. The iterations stop and the code can be merged back into the development branch.
    * If the code is good enough it can be merged into the development branch.

## Guide For An Effective Code Review

### Code style guide (Severity = Low)
---

In may be used to limit method body length, cyclomatic complexity, usage of legacy methods and much more. All this while requiring the format of the code to be common.

#### Linting and Code Style Check

Leave static code analysis and coding style check to machines with tools like **SonarQube** and **ESLint**, and spare human eyes for important parts like business logics and algorithms.

[Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)

* https://github.com/google/google-java-format

### Software Architecture (Severity = Medium)
---

Software architecture is the process of converting software characteristics such as flexibility, scalability, feasibility, reusability, and security into a structured solution that meets the technical and the business expectations.

### Software Design (SOLID) (Severity = Medium)

### Change Traceability
---

It is assumed that changes to the code are done based on specifications. These specs should be re-iterated very briefly in:

* Change log. // TODO  
* [Git commit messages]({{< ref "git/git-commit-message.md" >}}).
* The user performing the changes must also be identifiable.

> Git history shall be kept clean by properly rebasing feature branches before merging them back into the main development branch. // TODO Rebase feature branches.

### Documentation Coverage (Severity = Medium)
---

Documentation of the code is not to be a separate process, but rather be done together with the implementation.

> Focus also on the “why” and “how” not only on the “what”.

#### Provide enough context for creating meaningful pull request

- [Issue template]({{< ref "templates/issue.md" >}})
- [Pull request template]({{< ref "templates/pull-request.md" >}})

### New Developer On-boarding
---

The review process provides an opportunity for mentorship and collaboration, and, minimally, diversifies the understanding of code in the code base.

### Dependencies (Severity = Medium)
--- 

The security, licensing and versioning implications of 3rd party and open source solutions must be considered and monitored.

### Antipatterns (Severity = High)

### Maintainability (Severity = High)
---

  * Extending
  * Testing
  * Debugging
  * Configuring
  * Deploying
  * Automating

May be scored based on the efforts needed for the application

### Extensibility (Severity = Medium)

### Legibility (Severity = High)
---

Legible code is more reusable, bug-free, and future-proof

### Reusability (Severity = High)
---

Components used across the application(s) should be identified and modularised for better reusability and maintainability.

### Security (Severity = High)
---

{{%attachments title="OWASP Secure Code Review Guide" pattern="OWASP_Code_Review_Guide_v2.pdf"/%}}

### Performance (Severity = High)
---

  * Asynchronous
  * Parallel execution
  * Caching
  * Appropriate usage of resources
  * Memory leaks

### Scaleability (Severity = High)
    
### Usability (Severity = Medium)
---

The interfaces for other services, applications or 3rd parties shall be well documented and follow standards in order to be usable. 

The APIs should be implementable and testable solely using the documentation available without any dedicated support personnel.

### Testing Coverage (Severity = High)
    
### Automation Of Builds And Deployments

### Motivation
---

* Committers are motivated
* Sharing knowledge

### Scope and size (Severity = Medium)
---

Changes should have a narrow, well-defined, self-contained scope that they cover exhaustively.

#### Keep changes small

Treat each PR as a releasable unit (feature, bug fix) or a cohesive idea which is meaningful to the PR.

> Single-piece flow -> LEAN manufacturing

If a CR makes substantive changes to more than ~5 files, or took longer than 1–2 days to write, or would take more than 20 minutes to review, consider splitting it into multiple self-contained CRs.

#### Review often and shorten sessions

Code reviews in reasonable quantity, at a slower pace, for a limited amount of time result in the most effective code review.

> **3PM code review rule** -> Every day at 3PM is Code Review time. Improving burndown charts.

### Refactoring changes
---

> Refactoring changes should not alter behavior
> 
> A behavior-changing changes should avoid refactoring and code formatting changes.

* Refactoring changes often touch many lines and files and will consequently be reviewed with less attention.
* Large refactoring changes break cherry-picking, rebasing, and other source control magic.
* Expensive human review time should be spent on the program logic rather than style, syntax, or formatting debates.

### Measure the progress
---

> If you can’t see it, you can’t measure it, you can’t improve it!

* SonarQube project dashboard
* Team based code coverage
* PR Comment Notification
* PR size and resolution time. Making a releasable task/change small is a valid skill in DevOps. We try to promote this idea with the **Resolution time vs. PR size chart**
  
![bubble_graph](/se-journey/img/bubble_graph_pr.png)


## Checklist for Developers

**Implementation**

- [ ] My code compiles
- [ ] I have considered proper use of exceptions
- [ ] I have made appropriate use of logging
- [ ] I have eliminated unused imports
- [ ] I have eliminated IDE warnings
- [ ] I have considered possible NPEs
- [ ] Was performance considered?
- [ ] Was security considered?
- [ ] Does the code release resources? (HTTP connections, DB connection, files, etc)
- [ ] Can any code be replaced by calls to external reusable components or library functions?
- [ ] Thread safety and possible deadlocks
  
**Legibility and style**

- [ ] My code is tidy (indentation, line length, no commented-out code, no spelling mistakes, etc)
- [ ] The code follows the coding standards

**Maintainability**

- [ ] My code includes javadoc where appropriate
- [ ] My code has been developer-tested and includes unit tests
- [ ] Are there any leftover stubs or test routines in the code?
- [ ] Are there any hardcoded, development only things still in the code?
- [ ] Corner cases well documented or any workaround for a known limitation of the frameworks

## Checklist for Reviewers

**Purpose**

- [ ] Does this code accomplish the author’s purpose?
- [ ] All functions and classes exist for a reason.

**Implementation**

- [ ] The functionality fits the current design/architecture
- [ ] Types have been generalized where possible
- [ ] Parameterized types have been used appropriately
- [ ] Frameworks have been used appropriately
- [ ] Does the change follow standard patterns?
- [ ] Do you see potential for useful abstractions?
- [ ] Command classes have been designed to undertake one task only
- [ ] The code does not use unjustifiable static methods/blocks
- [ ] Logging used appropriately (proper logging level and details)
- [ ] NPEs and AIOBs
- [ ] Exceptions have been used appropriately
- [ ] Does the change add compile-time or run-time dependencies (especially between sub-projects)?
- [ ] Potential threading issues have been eliminated where possible
- [ ] Any security concerns have been addressed
- [ ] Performance was considered

**Legibility and style**

- [ ] Repetitive code has been factored out
- [ ] The code complies to coding guidelines and code style
- [ ] Does this code have TODOs?. Have the author submit a ticket on GitHub Issues or JIRA and attach the issue number to the TODO.

**Maintainability**

- [ ] Comments are comprehensible
- [ ] Comments are neither too numerous nor verbose
- [ ] Unit tests are present and they covering interesting cases
- [ ] Does this code need integration tests?
- [ ] Does this CR introduce the risk of breaking test code, staging stacks, or integrations tests?
- [ ] Does this change break backward compatibility?
- [ ] Is it OK to merge the change at this point or should it be pushed into a later release?
- [ ] Was the external documentation updated?

## Tips in Comments: concise, friendly, actionable

* Written in neutral language
* Critique the code, not the author.
* Avoid possessive pronouns
* Avoid absolute judgements
* Try to differentiate between:
  * Suggestions (e.g., “Suggestion: extract method to improve legibility”)
  * Required changes (e.g., “Add @Override”)
  * Points that need discussion or clarification (e.g., “Is this really the correct behavior? If so, please add a comment explaining the logic.”)

#### Examples

```java
class MyClass {
  private int countTotalPageVisits; //R: name variables consistently
  private int uniqueUsersCount;
}
```

```java
interface MyInterface {
  /** Returns {@link Optional#empty} if s cannot be extracted. */
  public Optional<String> extractString(String s);
  /** Returns null if {@code s} cannot be rewritten. */
  //R: should harmonize return values: use Optional<> here, too
  public String rewriteString(String s);
}
```

```java
//R: remove and replace by Guava's MapJoiner
String joinAndConcatenate(Map<String, String> map, String keyValueSeparator, String keySeparator);
```

```java
int dayCount; //R: nit: I usually prefer numFoo over fooCount; up to you, but we should keep it consistent in this project
```

```java
//R: This performs numIterations+1 iterations, is that intentional?
//   If it is, consider changing the numIterations semantics?
for (int i = 0; i <= numIterations; ++i) {
  ...
}
```

```java
otherService.call(); //R: I think we should avoid the dependency on OtherService. Can we discuss this in person?
```

