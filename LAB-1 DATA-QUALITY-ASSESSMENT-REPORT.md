
# Data Quality Assessment Report

## Task 1: Identified Data Quality Issues

| Dimension      | Violation Identified in Dataset                | Explanation                                                                                                   |
|---------------|------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Accuracy      | P002 (Ama Serwa):<br>Phone number 244789012    | In Ghana, mobile numbers must be 10 digits (e.g., 0244...). This number is only 9 digits, making it factually incorrect/unusable. |
| Completeness  | P004:<br>P004,0555234567,,...                   | There is a double comma after the ID, meaning the **BasicName** field is empty (null). This explains why reminders might fail. |
| Consistency   | **Oboe**/Oboe Dr. Osei vs. dr. osei            | The same doctor is represented with different casings. This causes issues when grouping reports by doctor.    |
| Timeliness    | P003 (Kofi Amna):<br>01/26/2025                 | While not strictly "outdated," the date format for this entry is MM/DD/YYYY, whereas others use YYYY-MM-DD. This mix-up leads to data being "wrongly dated" in a unified system. |
| Validity      | P002 (Ama Serwa):<br>15/10/2025                 | This entry uses a DD/MM/YYYY format, while the established system rule (seen in P001) appears to be YYYY-MM-DD. It violates the defined format rules. |
| Uniqueness    | P001 (Kwame Mensah)                             | Kwame Mensah appears twice in the snippet (once on Oct 15 and once on Oct 20). If these aren't meant to be separate appointments, it's a duplicate record. |

---

## Task 2: Business Impact Assessment

| Data Quality Issue         | Operational Problem           | Most Affected Function      |
|---------------------------|-------------------------------|----------------------------|
| Invalid Phone Numbers     | SMS Failure                   | Operation                  |
| Missing Patient Names     | Communication and Billing loss| Operation & Finance        |
| Duplicate patient IDs     | Inflated patient count/response| Operations                 |
| (Mixed Date Formats)      | Scheduling & Billing Failures | Operation                  |
| Inconsistent payment status| Billing status may not be recognized by status | Finance         |

---

## Task 3: Recommendation Solutions

To get **MedCare Ghana** back on track, we should prioritize the issues directly causing financial and communication failures.

---

## Task 4: Technical Solutions & Explanations

1. **Fix Invalid Date Formats (Validity):**
	- **Technical Solution:** Implement a standardization script (using a library like Python's pandas) to parse all dates into ISO 8601 format (YYYY-MM-DD), and add frontend validation/rules to the data entry form to prevent future format errors.
	- **Responsibility:** Junior Developer for the script; Senior Developer for the frontend validation.
	- **Verification:** Run a SQL query to check for any date records that do not match the YYYY-MM-DD pattern.

2. **Fix Phone Number & Contact Data (Accuracy/Completeness):**
	- **Technical Solution:** Apply regex validation rules at the point of entry. For Ghana, ensure numbers start with a prefix like 0 or +233 and contain exactly 10 digits (not local format).
	- **Responsibility:** DevOps/Backend Developer to implement database-level constraints.
	- **Verification:** Attempt to send a test SMS batch; track the "Delivery Success Rate" metric to see if failures drop.

3. **Fix Duplicate Records (Uniqueness):**
	- **Technical Solution:** Run a de-duplication script that merges records based on a composite key (e.g., BasicID + 0555234567). Implement a Unique Constraint on the **BasicID** field in the database.
	- **Responsibility:** Database Administrator (DBA) or Lead Developer.
	- **Verification:** Generate a "Patient Count" report and verify that there are zero instances of the same ID appearing for the same appointment slot.

---

## Task 4 (continued): Why Data Consistency Matters

As the Backend Engineer on this project, poor data consistency is not just a minor reporting issue, it is structural threat to the entire system. The database serves as the engine of the application, and like any high-performance engine, it depends on precision and standardization. When data is stored in inconsistent formats—such as "Dr. Osei" versus "dr. osei" or "Paid" versus "paid"—the system may appear functional at first, but hidden failures begin to surface under real-world usage.

Inconsistent data breaks relational integrity. Simple JOIN queries can fail because values do not match exactly, leading to orphaned or "invisible" records that exist in the database but cannot be properly accessed by the application. To compensate, developers are often forced to write extra data-cleaning logic, such as converting values to lowercase or trimming spaces. Over time, this creates messy, inefficient code that is harder to maintain and scale.

In distributed systems, the consequences are even more serious. If one service records a status as "Paid" while another searches for "paid," critical workflows like triggering notification messages can silently fail. Additionally, inconsistent strings bloat database indexes and slow down queries, producing misleading results in reports and analytics.

Ultimately, messy data undermines system reliability, performance, and developer productivity. For a backend engineer, maintaining strict data consistency is essential to building a stable, scalable, and trustworthy system.
