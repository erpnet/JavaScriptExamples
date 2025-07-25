## User Business Rules

### Validate customer VAT number with VIES

**Category:** Integration / Validation

**Description:**  
On sales order commit, validate the customer’s VAT number using the official EU VIES REST API (POST request).

If the VAT number is not valid, the operation is cancelled.

**Repository / Event / Condition:**  
- **Repository:** `Crm.Sales.SalesOrders`
- **Event:** `COMMIT`
- **Condition:** *Optional* (fully handled in script)

**Script:**
```js
if (subject.Customer && subject.Customer.Party && subject.Customer.Party.Country) {
    var url = "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number";
    var body = JSON.stringify({
        countryCode: subject.Customer.Party.Country.CountryCode,
        vatNumber: subject.Customer.Party.PartyUniqueNumber,
        requesterMemberStateCode: "BG",
        requesterNumber: "XXXXX"
    });
    var headers = "Content-Type: application/json";

    // POST request to VIES
    var response = Action.http.post(url, body, headers);

    // Parse response (expected: { countryCode, valid, ... })
    var viesResult = JSON.parse(response);

    if (!viesResult.valid) {
        Action.cancel("Customer VAT number is not valid according to VIES.");
    }
}
```