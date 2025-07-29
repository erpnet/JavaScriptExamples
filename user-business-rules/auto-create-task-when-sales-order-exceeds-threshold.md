## User Business Rules

### Notify user when sales order exceeds threshold

**Category:** Automation

**Description:**  
Calculate the total value of a sales order by summing the `LineAmount.Value` of all its lines.

If the total exceeds a threshold, take action (e.g., notify or escalate).

**Repository / Event / Condition:**  
- **Repository:** `Crm.Sales.SalesOrders`
- **Event:** `COMMIT`
- **Condition:** *Optional* (fully handled in script)

**Script:**
```js
var threshold = 50000;
var total = 0;
if (subject.Lines.Count > 0) {
    for (let i = 0; i < subject.Lines.Count; i++) {
	  total += line[i].LineAmount.Value;
	}
}

if (total > 50000) {
    Action.notify.user(
        <THE USER ID TO NOTIFY>,
        "Order " + subject.DocumentNumber + " exceeds the approval threshold (" + total + ")."
    );
}
```