## User Business Rules

### Whole Quantity Validation

**Category:** Validation

**Description:**  
Ensure that quantities in sales order lines are always whole numbers.

**Repository / Event / Condition:**  
- **Repository:** `Crm.Sales.SalesOrderLines`
- **Event:** `COMMIT`
- **Condition:**  None, handled in script

**Script:**
```js
// Cancel the operation if the quantity is not a whole number
if (subject.Quantity != null && subject.Quantity.Value % 1 !== 0) {
    Action.cancel("You have entered a decimal number as a quantity. Please check the data entered in the sales order lines and try again!");
```