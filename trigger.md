A **trigger** in SQL is a special type of stored procedure that automatically executes or fires in response to specific events on a particular table or view. Triggers are useful for enforcing business rules, maintaining data integrity, and automating tasks without requiring explicit commands from the user.

### Key Features of Triggers:

1. **Automatic Execution**: Triggers are invoked automatically when certain events occur, such as inserting, updating, or deleting data in a table.

2. **Types of Triggers**:
   - **Before Triggers**: Execute before the event (e.g., before an insert or update).
   - **After Triggers**: Execute after the event (e.g., after a delete or insert).

3. **Event-Driven**: They are tied to specific events that affect data.

4. **Cannot be Called Directly**: Triggers cannot be executed manually; they only run in response to an event.

### Use Cases for Triggers:

- **Data Validation**: Ensure that data meets specific criteria before it is inserted or updated.
- **Audit Trails**: Automatically log changes to a table (e.g., who made a change and when).
- **Enforcing Business Rules**: Automatically enforce business rules like preventing certain types of data changes.

### Example of a Trigger

Here's a simple example of a trigger that logs changes to an `Employees` table whenever an employee's salary is updated:

```sql
CREATE TABLE SalaryChanges (
    ChangeID INT PRIMARY KEY IDENTITY(1,1),
    EmployeeID INT,
    OldSalary DECIMAL(10, 2),
    NewSalary DECIMAL(10, 2),
    ChangeDate DATETIME DEFAULT GETDATE()
);

CREATE TRIGGER trg_AfterSalaryUpdate
ON Employees
AFTER UPDATE
AS
BEGIN
    INSERT INTO SalaryChanges (EmployeeID, OldSalary, NewSalary)
    SELECT 
        i.EmployeeID,
        d.Salary AS OldSalary,
        i.Salary AS NewSalary
    FROM 
        inserted i
    INNER JOIN 
        deleted d ON i.EmployeeID = d.EmployeeID
    WHERE 
        i.Salary <> d.Salary;  -- Only log if salary changed
END;
```

### Breakdown of the Example:

- **`SalaryChanges` Table**: A table to log changes in salary.
- **Trigger Creation**: The trigger `trg_AfterSalaryUpdate` is created on the `Employees` table to run after an update.
- **Insert Log**: When an employee’s salary is updated, the trigger inserts a new record into `SalaryChanges` with the old and new salary values.

### Summary

In summary, a trigger is an automatic, event-driven SQL procedure that helps maintain data integrity and enforce business rules by executing specific actions in response to data changes.
