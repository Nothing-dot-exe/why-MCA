# VTU MCA 2022/2024 Scheme - Database Management Systems (22MCA21)
## Full Solved Examination Paper with SQL, Normalization & ACID Properties
Time: 3 Hours | Max Marks: 100

================================================================================
MODULE 1: ER MODELING & RELATIONAL ALGEBRA
================================================================================

Q.1 (a) Design an ER Diagram for a Hospital Management System showing Entities, Attributes, Relationships, and Cardinality. [10 Marks]
Answer:
Entities & Attributes:
1. Patient (PatientID [PK], Name, Age, Gender, Contact, BloodGroup)
2. Doctor (DoctorID [PK], Name, Specialization, Phone, ConsultationFee)
3. Appointment (AppointmentID [PK], Date, Time, Status)
4. Department (DeptID [PK], DeptName, HeadDoctorID)

Relationships & Cardinality:
- Doctor to Department: Many-to-One (N:1). A doctor belongs to one department.
- Doctor to Patient: Many-to-Many (M:N) mediated by Appointment.
- Patient to Appointment: One-to-Many (1:N). A patient can book multiple appointments.

--------------------------------------------------------------------------------
Q.1 (b) Explain Normalization. Normalize a relation from 1NF to BCNF with a concrete example. [10 Marks]
Answer:
1. First Normal Form (1NF): All attributes must contain atomic (indivisible) values. No repeating groups.
2. Second Normal Form (2NF): Must be in 1NF and have NO Partial Dependency (every non-prime attribute must depend on the whole candidate key, not part of it).
3. Third Normal Form (3NF): Must be in 2NF and have NO Transitive Dependency (X -> Y and Y -> Z where Z is non-prime is disallowed).
4. Boyce-Codd Normal Form (BCNF): For every functional dependency X -> Y, X must be a Super Key.
