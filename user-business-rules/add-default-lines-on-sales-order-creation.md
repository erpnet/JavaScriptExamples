## User Business Rules

### Add default lines on sales order creation

**Category:** Automation

**Description:**  
When a new sales order is created, automatically add default sales order lines if a specific condition is met (e.g., customer is a VIP).

**Repository / Event / Condition:**  
- **Repository:** `Crm.Sales.SalesOrders`
- **Event:** `CREATENEW`
- **Condition:** *Optional* (fully handled in script)

**Script:**
```js
// Automatically add default lines for VIP customers
if (subject.Customer != null && subject.Customer.CustomerType.Id == "SPECIFIC-CUSTOMER-TYPE-ID-HERE") {
    // Add a default product line
    var defaultProduct = Domain.General.Products.ProductsRepository.getById("YOUR-PRODUCT-ID-HERE");
    if (defaultProduct) {
        var newLine = Domain.Crm.Sales.SalesOrderLinesRepository.createNew();
        newLine.SalesOrder = subject;
        newLine.Product = defaultProduct;
        newLine.Quantity = new Domain.Types.Quantity(1, defaultProduct.MeasurementUnit);
        newLine.UnitPrice = 100; // Set default price if needed
    }
}
```