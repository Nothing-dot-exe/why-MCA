# Master Study Notes: Software Engineering & Agile Methodology
## Course Code: 22MCA23 / MMC204 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: Software Process Models (Waterfall, Spiral, V-Model), Agile Manifesto & Principles, Scrum Framework (Roles, Artifacts, Ceremonies), Kanban.
* **Module 2**: Requirements Engineering, Functional vs. Non-Functional Requirements, IEEE 830 Standard for SRS, User Stories & Acceptance Criteria.
* **Module 3**: UML 2.0 System Modeling (Use Case, Class, Sequence, Activity Diagrams), Architectural Patterns (Layered, Microservices, Event-Driven).
* **Module 4**: Software Testing Levels (Unit, Integration, System, Acceptance), Black-Box Testing (BVA, Equivalence Partitioning), White-Box Testing (Control Flow Graphs & Cyclomatic Complexity).
* **Module 5**: Software Cost Estimation (COCOMO II, Function Points), Risk Management (RMMM Plan), CI/CD Automation & Modern DevSecOps Pipelines.

---

# MODULE 1: PROCESS MODELS & AGILE / SCRUM

## 1.1 Prescriptive Process Models
1. **Waterfall Model**: Linear sequential phases (Requirements $\to$ Design $\to$ Implementation $\to$ Verification $\to$ Maintenance). Best for stable, well-understood requirements. High risk of late defect discovery.
2. **Spiral Model (Boehm)**: Risk-driven iterative model structured around 4 quadrants (Determine Objectives, Identify & Resolve Risks, Develop & Test, Plan Next Phase).
3. **V-Model**: Pairs each development phase directly with its corresponding verification testing level.

---

## 1.2 Agile Principles & The Scrum Framework
* **Agile Manifesto Core Values**:
  1. Individuals and interactions over processes and tools.
  2. Working software over comprehensive documentation.
  3. Customer collaboration over contract negotiation.
  4. Responding to change over following a plan.

### Scrum Roles, Artifacts & Ceremonies:
* **Roles**:
  * *Product Owner (PO)*: Defines requirements, manages and prioritizes the Product Backlog.
  * *Scrum Master*: Servant-leader who coaches the team and clears sprint impediments.
  * *Development Team*: Cross-functional, self-organizing engineering unit (5–9 members).
* **Artifacts**:
  * *Product Backlog*: Prioritized inventory of user stories and feature requests.
  * *Sprint Backlog*: Subset of user stories committed for completion in the current sprint.
  * *Increment (Burndown Chart)*: Shippable, high-quality product increment meeting the "Definition of Done".
* **Ceremonies**: Sprint Planning, Daily Scrum (15-min standup), Sprint Review (Demo), Sprint Retrospective.

---

# MODULE 2: REQUIREMENTS ENGINEERING & SRS

## 2.1 Functional vs. Non-Functional Requirements
* **Functional Requirements (FR)**: Statements of services the system should provide, how the system reacts to particular inputs, and how it behaves in specific situations (e.g., *“The system shall encrypt password using bcrypt before storing”*).
* **Non-Functional Requirements (NFR)**: Constraints on services/functions (Performance, Scalability, Security, Maintainability, Availability $\ge 99.9\%$).

## 2.2 IEEE 830 Standard Structure for SRS
1. **Introduction**: Purpose, Scope, Definitions, Acronyms, References.
2. **Overall Description**: Product perspective, Product functions, User characteristics, General constraints, Assumptions and dependencies.
3. **Specific Requirements**: External interfaces (User, Hardware, Software, Comm), Detailed functional requirements, Performance and security requirements.

---

# MODULE 3: SYSTEM MODELING & ARCHITECTURE

## 3.1 UML 2.0 Diagram Types
* **Structural Diagrams**: Class Diagram (classes, attributes, methods, associations, aggregations, compositions), Component Diagram, Deployment Diagram.
* **Behavioral Diagrams**: Use Case Diagram (Actors, Include/Extend relationships), Sequence Diagram (Lifelines, Messages, Synchronous/Asynchronous calls), Activity Diagram (Workflows, Decision diamonds, Swimlanes).

## 3.2 Monolith vs. Microservices Architecture
* **Monolithic**: Single code repository; unified deployment; tight coupling; single point of failure; hard to scale independently.
* **Microservices**: Decomposed into independently deployable, loosely coupled services communicating via REST APIs or Message Queues (Kafka/RabbitMQ); fault isolated; polyglot persistence.

---

# MODULE 4: SOFTWARE TESTING STRATEGIES

## 4.1 Black-Box vs. White-Box Testing
* **Black-Box Testing (Functional)**: Tests system functionality without knowledge of internal code structure.
  * *Equivalence Partitioning (EP)*: Divides input domain into valid and invalid classes.
  * *Boundary Value Analysis (BVA)*: Tests values at extremes ($min, min+, nominal, max-, max$).
* **White-Box Testing (Structural)**: Examines internal procedural paths and logic.

---

## 4.2 Basis Path Testing & Cyclomatic Complexity (McCabe)
Cyclomatic complexity measures the number of linearly independent paths through code.

### Formula Options:
1. $V(G) = E - N + 2 P$
   * Where $E = \text{number of edges}$, $N = \text{number of nodes}$, $P = \text{connected components}$ ($P = 1$ for single program).
2. $V(G) = \text{Number of Predicate Nodes (Decision points } D\text{)} + 1$:
   $$V(G) = D + 1$$
3. $V(G) = \text{Number of enclosed regions in planar graph} + 1$.

---

# MODULE 5: COST ESTIMATION & DEVSECOPS

## 5.1 Basic COCOMO Model (Boehm)
$$E = a \cdot (\text{KLOC})^b \quad \text{[Person-Months]}$$
$$D = c \cdot (E)^d \quad \text{[Development Time in Months]}$$

| Project Mode | Complexity & Team Size | $a$ | $b$ | $c$ | $d$ |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Organic** | Small teams, familiar in-house environment | 2.4 | 1.05 | 2.5 | 0.38 |
| **Semidetached**| Medium teams, mixed experience, complex interfaces | 3.0 | 1.12 | 2.5 | 0.35 |
| **Embedded** | Strict hardware/regulatory constraints, real-time | 3.6 | 1.20 | 2.5 | 0.32 |

---

# 🎯 Model Exam Questions with Step-by-Step Solutions

### Q1. [Module 4 - 10 Marks]
**A program takes an integer input representing student percentage (0 to 100). Scores $\ge 70$ get 'FCD', 60–69 get 'FC', 50–59 get 'SC', and $< 50$ get 'Fail'. Design test cases using Boundary Value Analysis (BVA) and Equivalence Class Partitioning (EP).**

**Solution:**
1. **Equivalence Class Partitioning (EP)**:
   * Valid Partitions:
     * $EC_1: [70, 100]$ (First Class with Distinction) $\to$ Test value: `85`
     * $EC_2: [60, 69]$ (First Class) $\to$ Test value: `65`
     * $EC_3: [50, 59]$ (Second Class) $\to$ Test value: `55`
     * $EC_4: [0, 49]$ (Fail) $\to$ Test value: `35`
   * Invalid Partitions:
     * $EC_5: \text{Input } < 0$ $\to$ Test value: `-5`
     * $EC_6: \text{Input } > 100$ $\to$ Test value: `105`

2. **Boundary Value Analysis (BVA) Test Cases**:
   * Evaluates values at the exact boundaries ($min, min+, max-, max$):
     * Lower boundary ($0$): Test values ` -1` (invalid), `0` (min), `1` (min+)
     * Second class boundary ($50$): Test values `49` (Fail), `50` (SC), `51` (SC)
     * First class boundary ($60$): Test values `59` (SC), `60` (FC), `61` (FC)
     * Distinction boundary ($70$): Test values `69` (FC), `70` (FCD), `71` (FCD)
     * Upper boundary ($100$): Test values `99` (max-), `100` (max), `101` (invalid)

---

### Q2. [Module 4 - 10 Marks]
**Calculate the Cyclomatic Complexity for the following control flow segment:**
```c
int findMax(int a, int b, int c) {
    int max;
    if (a > b) {
        if (a > c)
            max = a;
        else
            max = c;
    } else {
        if (b > c)
            max = b;
        else
            max = c;
    }
    return max;
}
```

**Solution:**
1. **Identify Predicate (Decision) Nodes**:
   * Decision 1: `if (a > b)`
   * Decision 2: `if (a > c)`
   * Decision 3: `if (b > c)`
   * Number of decision conditions $D = 3$.

2. **Calculate Complexity $V(G)$**:
   $$V(G) = D + 1 = 3 + 1 = \mathbf{4}$$

3. **Linearly Independent Paths ($V(G) = 4$)**:
   * Path 1: `a > b` (True) $\to$ `a > c` (True) $\to$ `max = a` $\to$ return
   * Path 2: `a > b` (True) $\to$ `a > c` (False) $\to$ `max = c` $\to$ return
   * Path 3: `a > b` (False) $\to$ `b > c` (True) $\to$ `max = b` $\to$ return
   * Path 4: `a > b` (False) $\to$ `b > c` (False) $\to$ `max = c` $\to$ return
