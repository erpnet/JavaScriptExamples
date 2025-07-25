## User Business Rules

### Validate customer credit status via external service

**Category:** Integration / Validation

**Description:**  
Before allowing a sales order to be committed, the script calls an external credit service (via HTTP GET) to check the customer's credit status.

If the service response is not `"APPROVED"`, the commit is cancelled.

**Repository / Event / Condition:**  
- **Repository:** `Crm.Sales.SalesOrders`
- **Event:** `COMMIT`
- **Condition:** *Optional* (fully handled in script)

**Script:**
```js
// Build the external API URL, including the customer's unique code
var url = "https://credit.example.com/api/check?customerCode=" + encodeURIComponent(subject.Customer.Number);

// Optionally add authorization or other headers
var headers = "Authorization: Bearer YOUR_TOKEN_HERE";

// Call the external service
var response = Action.http.get(url, headers);

// Assume the service returns a JSON object, e.g. { "status": "APPROVED" }
var creditStatus = JSON.parse(response).status;

if (creditStatus !== "APPROVED") {
    Action.cancel("Customer credit status is not approved. The order cannot be committed.");
}
```