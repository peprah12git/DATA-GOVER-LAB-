Corrected Data Flow Diagram (Annotated Corrections)
Below are the required corrections with detailed explanations:
1.	Correction at Step 1: User Mobile App (Excessive Collection)
Change: Removed collection of unnecessary data such as full contact lists and unrelated device metadata.
Reason: The original design collected excessive personal data, violating the Data Minimization principle under Ghana’s Data Protection Act (Act 843). Only data strictly necessary for loan assessment (income, ID, phone number, repayment history) should be collected
2.	Correction Between Step 2: Raw Data Database.
Change: Introduced an explicit Consent Capture and Logging Service before storing data in the Raw Data Database.
Reason: The original pipeline stored data without documented user consent. Act 843 requires lawful and transparent processing of personal data.
3.	Correction at Step 3 – Data Classification & Retention Policy Applied
Change: Classified stored data as Sensitive and implemented encryption and retention controls (e.g., delete inactive user data after defined period).
Reason: Financial and identity data require higher protection and cannot be stored indefinitely.
4.	Correction at Step 4 – Data Quality & Bias Controls Added
Change: Added validation rules, duplicate detection, and feature review before data enters the ML model.
Reason: Poor data quality affects model accuracy, and unchecked proxy variables may introduce bias.
5.	Correction at Step 7 – Decision Logging & Explainability
Change: Implemented decision logging with reason codes for approvals and denials.
Reason: Automated decisions must be transparent and auditable.
6.	Correction at Step 9 & 10 – Data Masking Before Analytics & Sharing
Change: Applied anonymization and tokenization before storing in Analytics DB or sharing with third parties.
Reason: Prevents exposure of personally identifiable information outside core processing systems.




