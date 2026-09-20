# Master Study Notes: Database Management Systems (DBMS)
## Course Code: 22MCA21 / MMC103 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: DBMS Architecture, Three-Schema ANSI/SPARC Architecture, Data Independence, Entity-Relationship (ER) Modeling, Extended ER (EER), ER-to-Relational Schema Mapping.
* **Module 2**: Relational Model Concepts, Relational Algebra Operators, SQL (DDL, DML, DCL, TCL), Complex Nested Subqueries, Aggregations, Views & Triggers.
* **Module 3**: Database Design Theory, Functional Dependencies (Armstrong's Axioms), Normalization (1NF, 2NF, 3NF, BCNF, 4NF), Lossless Decomposition & Dependency Preservation.
* **Module 4**: Transaction Processing, ACID Properties, Schedules & Conflict Serializability, Concurrency Control (Two-Phase Locking - 2PL, Timestamp Ordering), Deadlock Handling.
* **Module 5**: Physical Storage & Indexing, B-Trees & B+ Trees, Modern NoSQL Systems (MongoDB, Cassandra, Redis), CAP Theorem & ACID vs. BASE.

---

# MODULE 1: DBMS ARCHITECTURE & ER MODELING

## 1.1 Three-Schema Architecture (ANSI/SPARC)
1. **External Level (View Level)**: Describes the part of the database relevant to specific user groups; hides details from other users.
2. **Conceptual Level (Logical Level)**: Describes *what* data is stored in the database and the relationships among the data (Entities, Data types, Constraints).
3. **Internal Level (Physical Level)**: Describes *how* data is physically stored on storage disks (record formats, indices, compression).

* **Logical Data Independence**: Ability to modify the conceptual schema without altering external schemas or application programs.
* **Physical Data Independence**: Ability to modify the physical schema (e.g., changing disk storage or indexing) without altering the conceptual schema.

---

## 1.2 Entity-Relationship (ER) Modeling
* **Entity**: A distinct real-world object (e.g., `Student`, `Course`).
* **Entity Set**: Collection of entities of the same type.
* **Attributes**:
  * *Simple vs Composite*: `Age` vs `Address(Street, City, Zip)`.
  * *Single-valued vs Multi-valued*: `SSN` vs `{Phone_Numbers}` (represented as double ellipse).
  * *Derived Attribute*: Calculated from other attributes (e.g., `Age` from `DOB`, represented by dashed ellipse).
* **Key Attributes**: Primary key uniquely identifying entity (underlined in ER diagram).
* **Weak Entity Set**: An entity set that does not possess sufficient attributes to form a primary key. It depends on an **Identifying Owner Entity**. Represented by **double rectangles**, with its partial discriminator key indicated by a **dashed underline**.

---

# MODULE 2: RELATIONAL ALGEBRA & ADVANCED SQL

## 2.1 Fundamental Relational Algebra Operations
1. **Selection ($\sigma_{\text{condition}}(R)$)**: Filters horizontal rows satisfying predicate.
2. **Projection ($\pi_{\text{attributes}}(R)$)**: Extracts vertical columns; eliminates duplicates.
3. **Cartesian Product ($R \times S$)**: Combines every tuple of $R$ with every tuple of $S$.
4. **Union ($R \cup S$)** & **Set Difference ($R - S$)**: Requires Union Compatibility (same degree and corresponding compatible attributes).
5. **Natural Join ($R \bowtie S$)**: Equi-join on all common attributes followed by projection of duplicate columns:
   $$R \bowtie S = \pi_{\text{attributes}}(\sigma_{R.A = S.A}(R \times S))$$

---

## 2.2 SQL Deep-Dive: Triggers and Views
* **View**: A virtual table derived from the result of a SQL query:
  ```sql
  CREATE VIEW HighScoringStudents AS
  SELECT StudentID, Name, CGPA
  FROM Students
  WHERE CGPA >= 8.5;
  ```
* **Trigger**: A stored procedure that automatically executes in response to database events (`INSERT`, `UPDATE`, `DELETE`):
  ```sql
  CREATE TRIGGER AuditSalaryUpdate
  AFTER UPDATE OF Salary ON Faculty
  FOR EACH ROW
  BEGIN
      INSERT INTO SalaryAuditLog (FacultyID, OldSalary, NewSalary, ChangeDate)
      VALUES (:OLD.FacultyID, :OLD.Salary, :NEW.Salary, SYSDATE);
  END;
  ```

---

# MODULE 3: NORMALIZATION & RELATIONAL DESIGN

## 3.1 Functional Dependencies & Armstrong’s Axioms
A functional dependency $X \to Y$ states that the value of $X$ uniquely determines $Y$.
* **Reflexivity**: If $Y \subseteq X$, then $X \to Y$.
* **Augmentation**: If $X \to Y$, then $X Z \to Y Z$.
* **Transitivity**: If $X \to Y$ and $Y \to Z$, then $X \to Z$.

---

## 3.2 Normal Forms Summary Table

| Normal Form | Condition / Requirement | Anomaly Eliminated |
| :--- | :--- | :--- |
| **1NF** | All attributes contain atomic (indivisible) values; no repeating groups. | Multi-valued and nested structures. |
| **2NF** | In 1NF and **no Partial Dependency** (no non-prime attribute depends on a proper subset of any candidate key). | Partial functional dependencies. |
| **3NF** | In 2NF and **no Transitive Dependency** ($X \to Y$ must have $X$ as superkey OR $Y$ as prime attribute). | Transitive update anomalies. |
| **BCNF** | For every non-trivial $X \to Y$, **$X$ must be a Superkey**. | Overlapping candidate key anomalies. |
| **4NF** | Eliminates **Multivalued Dependencies ($X \twoheadrightarrow Y$)**. | Independent multi-valued facts. |

---

# MODULE 4: TRANSACTION PROCESSING & CONCURRENCY

## 4.1 ACID Properties
* **Atomicity**: All-or-nothing execution. Controlled by Transaction Manager & Recovery Manager (Rollback).
* **Consistency**: Transaction preserves database validity invariants from one consistent state to another.
* **Isolation**: Concurrent transactions execute without interfering with each other. Ensured by Concurrency Control.
* **Durability**: Committed updates persist permanently even across system crashes. Ensured by Write-Ahead Logging (WAL).

---

## 4.2 Conflict Serializability & Precedence Graphs
Two operations in a schedule conflict if:
1. They belong to different transactions.
2. They access the same data item $Q$.
3. At least one of them is a **Write** operation ($W(Q)$).

### Testing Serializability:
Construct a directed graph $G = (V, E)$ where nodes represent transactions:
* Draw a directed edge $T_i \to T_j$ if an operation of $T_i$ precedes and conflicts with an operation of $T_j$.
* **Serializability Theorem**: *A schedule $S$ is conflict serializable if and only if its precedence graph contains **no cycles**.*

---

## 4.3 Two-Phase Locking (2PL) Protocol
1. **Growing Phase**: Transaction may acquire locks, but cannot release any lock.
2. **Shrinking Phase**: Transaction may release locks, but cannot acquire any new lock.
* **Strict 2PL**: All Exclusive ($X$) locks must be held until the transaction **Commits or Aborts**. Guarantees strict, cascade-free schedules.

---

# MODULE 5: STORAGE, INDEXING & MODERN NOSQL

## 5.1 B-Trees vs. B+ Trees
* **B-Tree**: Keys and satellite record pointers stored at internal and leaf nodes.
* **B+ Tree**:
  * Satellite record pointers stored **strictly in leaf nodes**.
  * Internal nodes contain only routing keys, allowing much higher fan-out.
  * Leaf nodes are linked via a doubly linked list, enabling rapid $O(k)$ **range queries**.

---

## 5.2 NoSQL Categories & The CAP Theorem

```
                     Consistency (C)
                         /\
                        /  \
                       /    \
                      /  CA  \
                     / RDBMS  \
                    /          \
   Partition       /------------\   Availability (A)
   Tolerance (P)  /  CP      AP  \
                 MongoDB   Cassandra
```

* **CAP Theorem (Brewer)**: A distributed data store can simultaneously guarantee at most **two out of three** guarantees:
  1. **Consistency (C)**: Every read receives the most recent write.
  2. **Availability (A)**: Every non-failing request receives a non-error response.
  3. **Partition Tolerance (P)**: System operates despite network packet drops.

---

# 🎯 Model Exam Questions with Step-by-Step Solutions

### Q1. [Module 3 - 10 Marks]
**Given the relation $R(A, B, C, D, E)$ with functional dependencies:**
$$F = \{ A \to B, \; B C \to D, \; E \to C, \; D \to A \}$$
1. Find all Candidate Keys of $R$.
2. Determine the highest normal form of $R$.
3. Decompose $R$ into BCNF.

**Solution:**
1. **Candidate Key Determination**:
   * Attributes not appearing on the right-hand side of any FD: $E$ must be in every key.
   * Compute closure $(A E)^+$:
     * $(A E)^+ = \{A, E, B, C, D\} = R \implies \mathbf{AE}$ is a Candidate Key.
   * Compute closure $(B E)^+$:
     * Since $E \to C$, we have $B, E, C \implies B C \to D \implies D \to A \implies (B E)^+ = R \implies \mathbf{BE}$ is a Candidate Key.
   * Compute closure $(D E)^+$:
     * $D \to A \implies (D E)^+ \supseteq (A E)^+ = R \implies \mathbf{DE}$ is a Candidate Key.
   * **Candidate Keys**: $\mathbf{\{AE, BE, DE\}}$.
   * Prime attributes: $\{A, B, D, E\}$. Non-prime attribute: $\{C\}$.

2. **Testing Normal Forms**:
   * Consider $A \to B$:
     * $A$ is not a superkey. But $B$ is a prime attribute $\implies$ Satisfies 3NF.
   * Consider $E \to C$:
     * $E$ is not a superkey. $C$ is NOT a prime attribute.
     * Moreover, $E$ is a proper subset of candidate keys $AE, BE, DE$. Thus, $E \to C$ is a **Partial Dependency** of a non-prime attribute on a key subset.
   * Therefore, $R$ violates 2NF. **Highest Normal Form = 1NF**.

3. **BCNF Decomposition**:
   * Violating FD: $E \to C$ ($E$ is not a superkey).
   * Decompose $R$ into:
     * $R_1(E, C)$ with $E \to C$ (Key is $E$, in BCNF).
     * $R_2(A, B, D, E)$ with $F_2 = \{ A \to B, \; D \to A \}$.
   * In $R_2$, candidate keys are $AE, BE, DE$.
   * $A \to B$ violates BCNF ($A$ is not superkey in $R_2$).
     * Decompose $R_2$ into:
       * $R_{21}(A, B)$ with $A \to B$ (Key is $A$, in BCNF).
       * $R_{22}(A, D, E)$ with $D \to A$ (Key is $DE, AE$).
     * In $R_{22}$, $D \to A$ violates BCNF ($D$ is not superkey).
       * Decompose $R_{22}$ into:
         * $R_{221}(D, A)$ with $D \to A$ (Key is $D$, in BCNF).
         * $R_{222}(D, E)$ (All key attributes, in BCNF).
   * **Final BCNF Relations**: $\mathbf{\{ R_1(E, C), \; R_{21}(A, B), \; R_{221}(D, A), \; R_{222}(D, E) \}}$.
