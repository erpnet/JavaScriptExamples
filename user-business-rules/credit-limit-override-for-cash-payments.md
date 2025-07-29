## User Business Rules

### Credit limit override for cash payments

**Category:** Validation / Automation

**Description:**  
Automatically set the 'Credit Limit Override' field on a sales order when the payment type's system type is "In Cash" (`SystemType = 0`).  
This replaces previous logic that relied on calculated attributes and complex business rule setup.

**Repository / Event / Condition:**  
- **Repository:** `Crm.Sales.SalesOrders`
- **Event:** Change of State (`RELEASING`)
- **Condition:** *(Optional, handled in script)*

**Script:**
```js
// Check if the sales order payment type is 'In Cash' (SystemType = 0)
if (subject.PaymentType != null && subject.PaymentType.SystemType === 0) {
    subject.CreditLimitOverride = true;
}
```