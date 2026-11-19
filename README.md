# Citizen Benefit Case Management System (Microsoft Dynamics 365 / Power Platform)

An enterprise-grade, low-code case adjudication and citizen benefits management solution built on **Microsoft Dataverse** and **Power Apps (Model-Driven App)**. Modeled after Government of Canada service delivery workflows, this application enforces relational data integrity, declarative business logic, and Role-Based Access Control (RBAC) for public-sector audit compliance.

---

## Architecture & Solution Components

The entire solution is packaged inside an unmanaged Power Platform solution (**`Citizen Case Management`**, v1.0.0.1) under the **`GC Portfolio`** publisher (`gc_` prefix) for clean ALM (Application Lifecycle Management) portability.

### 1. Relational Data Model (`1:N` Schema)
* **Parent Table — `Citizen` (`gc_citizen2`):** Stores applicant demographics, contact details, province/territory, and annual income records.
* **Child Table — `Benefit Case` (`gc_benefitcase1`):** Tracks benefit applications (e.g., Employment Insurance, Old Age Security, Disability Support) linked to the parent `Citizen` table via a `1:N` lookup relationship, complete with auto-numbered case identifiers (`CASE-1000+`), Quick View applicant cards, and subgrid views.

### 2. Server-Side Business Logic
* **Business Rule (`Require Payout on Approval`):** Evaluates the `Case Status` field on the `Benefit Case` entity in real time:
  * **When `Case Status = Approved`:** Dynamically reveals the **`Approved Payout`** currency field and enforces **`Business Required`** validation so no case can be approved without a verified disbursement amount.
  * **When `Case Status ≠ Approved`:** Hides **`Approved Payout`** and removes the mandatory field constraint to keep intake and rejection forms clean.

### 3. Model-Driven Application (`GC Citizen Case Portal`)
* Unified left-pane navigation grouping **Citizens** and **Benefit Cases** for case officers.
* Custom Main forms, Quick View forms, and filtered Active views for rapid intake, eligibility review, and adjudication notes.

### 4. Role-Based Access Control (`GC Case Officer` Security Role)
* Cloned from **Basic User** and customized with least-privilege permissions across custom entities (`gc_citizen2` and `gc_benefitcase1`):
  * **Create, Read, Write, Append, Append To:** **User Level** (Case officers can only view and adjudicate records assigned to them).
  * **Delete:** **None** (Prevents record deletion to preserve public-sector audit trails).

---

## Proof of Work & Screenshots

### 1. Live Model-Driven App — Approved Benefit Case & Dynamic Payout Field
![Live App Approved Case](screenshots/01_live_app_approved_case.png)

### 2. Declarative Business Rule (`Require Payout on Approval`)
![Business Rule Flow](screenshots/02_business_rule_flow.png)

### 3. RBAC Security Role Matrix (`GC Case Officer`)
![RBAC Security Role](screenshots/03_rbac_security_role.png)

---

## Repository Structure

```text
Dynamics365-Citizen-Case-Management/
├── CitizenCaseManagement_1_0_0_1.zip   # Exported Unmanaged Power Platform Solution
├── screenshots/
│   ├── 01_live_app_approved_case.png   # Model-Driven App UI & dynamic field validation
│   ├── 02_business_rule_flow.png       # Activated Dataverse Business Rule designer flow
│   └── 03_rbac_security_role.png       # Custom GC Case Officer privilege matrix
└── README.md
