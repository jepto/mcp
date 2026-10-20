# Xero MCP server

Connect Xero to Claude, ChatGPT, Cursor and any other MCP client with the hosted Jepto MCP server. Ask about invoices, payments, contacts, accounts and your profit and loss, and put revenue next to your marketing spend.

```
https://mcp.jepto.com
```

There's nothing to install: Jepto hosts the server and you sign in with OAuth. Your Xero data comes from the Xero account you've connected in Jepto, with the same 20 metrics and 55 dimensions you get in Jepto Reports and Data Chat. [Connection steps for each client](https://github.com/jepto/mcp#connect) are in the main README.

## What you can ask

- "How much is outstanding on invoices, by contact?"
- "What's the average number of days customers take to pay?"
- "Show my profit and loss by section for last quarter."

You don't need to name fields or tools. The assistant looks up the fields below with `Analytics_Schema` and runs the query with `Analytics_Query`.

Xero is read-only through the MCP: the assistant can query your data but can't change anything in Xero.

## Fields

75 fields (20 metrics, 55 dimensions), from version 1.1.1 of the Jepto data model. `schema.json` in this folder has the same list in machine-readable form.

### Invoices

Sales and purchase invoices with totals, amounts due, status and dates.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `xero.invoices.total` | Total | SUM | Currency | Total value of the invoice including tax in the invoice currency. |
| `xero.invoices.amountDue` | Amount due | SUM | Currency | Outstanding balance still owed on the invoice. |
| `xero.invoices.invoiceCount` | Invoices | COUNT_DISTINCT |  | Number of distinct invoices in the result. |
| `xero.invoices.outstanding` | Outstanding | Calculated: Total − Amount due | Currency | Amount still outstanding after payments: total minus amount due. |
| `xero.invoices.invoicePaidRate` | Paid rate | Calculated: Amount ÷ Total | Percentage | Share of invoice total that has been paid: payments divided by invoice total. |
| `xero.invoices.totalBase` | Total (base currency) | Calculated: Total ÷ Currency rate | Currency | Invoice total converted to the organisation base currency using the stored currency rate. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `xero.invoices.invoiceId` | Invoice ID | string | Unique identifier for the invoice in Xero. |
| `xero.invoices.invoiceNumber` | Invoice number | string | Human-readable invoice number shown in Xero. |
| `xero.invoices.contactId` | Contact ID | string | Xero contact linked to this invoice. |
| `xero.invoices.contactName` | Contact name | string | Name of the customer or supplier on the invoice. |
| `xero.invoices.type` | Invoice type | string | Whether the document is an accounts receivable or accounts payable invoice. |
| `xero.invoices.status` | Status | string | Current workflow status of the invoice in Xero. Jepto syncs all statuses including DRAFT, VOIDED, and DELETED — filter before reporting totals. |
| `xero.invoices.date` | Invoice date | time | Issue date of the invoice used for reporting periods. |
| `xero.invoices.dueDate` | Due date | time | Date payment is due on the invoice. |
| `xero.invoices.currencyCode` | Currency | string | Currency code for invoice amounts. |
| `xero.invoices.reference` | Reference | string | Optional reference text entered on the invoice in Xero. |
| `xero.invoices.fullyPaidOnDate` | Fully paid on | time | Date the invoice was fully paid in Xero. |
| `xero.invoices.currencyRate` | Currency rate | number | Exchange rate from invoice currency to the organisation base currency. |
| `xero.invoices.lineAmountTypes` | Line amount types | string | Whether invoice line amounts are Exclusive, Inclusive, or NoTax in Xero. |

### Invoice lines

Line items with quantity, unit amount, tax and account code.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `xero.invoice_lines.quantity` | Quantity | SUM |  | Sum of quantities across invoice line items. |
| `xero.invoice_lines.unitAmount` | Unit amount | SUM | Currency | Sum of unit prices before tax on line items. |
| `xero.invoice_lines.lineAmount` | Line amount | SUM | Currency | Gross line amount from Xero. Not P&L-grade on its own — use lineAmountNet for expense/revenue analysis. |
| `xero.invoice_lines.taxAmount` | Tax amount | SUM | Currency | Tax component on invoice line items. |
| `xero.invoice_lines.lineCount` | Line items | COUNT_DISTINCT |  | Number of distinct invoice line items. |
| `xero.invoice_lines.lineAmountNet` | Line amount (net) | Calculated: Line amount − Tax amount | Currency | Tax-exclusive line amount: line amount minus tax amount. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `xero.invoice_lines.lineId` | Line ID | string | Unique identifier for a line item on an invoice. |
| `xero.invoice_lines.invoiceId` | Invoice ID | string | Parent invoice for this line item. |
| `xero.invoice_lines.description` | Description | string | Text description of the line item. |
| `xero.invoice_lines.accountCode` | Account code | string | Chart of accounts code assigned to the line. |
| `xero.invoice_lines.accountName` | Account name | string | Chart of accounts name denormalised on the line. |
| `xero.invoice_lines.taxType` | Tax type | string | Tax treatment applied to the line item. |
| `xero.invoice_lines.date` | Line date | time | Invoice date carried on the line for time-based reporting. |

### Contacts

Customers and suppliers.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `xero.contacts.contactCount` | Contacts | COUNT_DISTINCT |  | Number of distinct contacts in the result. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `xero.contacts.contactId` | Contact ID | string | Unique identifier for the contact in Xero. |
| `xero.contacts.name` | Contact name | string | Display name of the customer or supplier in Xero. |
| `xero.contacts.email` | Email | string | Primary email address stored on the contact. |
| `xero.contacts.isCustomer` | Is customer | boolean | Whether the contact is flagged as a customer in Xero. |
| `xero.contacts.isSupplier` | Is supplier | boolean | Whether the contact is flagged as a supplier in Xero. |
| `xero.contacts.updatedAt` | Updated at | time | Last time the contact record was updated in Xero. |

### Accounts

Your chart of accounts.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `xero.accounts.accountCount` | Accounts | COUNT_DISTINCT |  | Number of distinct chart of accounts entries. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `xero.accounts.xeroAccountId` | Account ID | string | Unique identifier for the account in Xero. |
| `xero.accounts.code` | Account code | string | Chart of accounts code used on transactions. |
| `xero.accounts.name` | Account name | string | Name of the account in the chart of accounts. |
| `xero.accounts.accountType` | Account type | string | Xero account type such as revenue or expense. |
| `xero.accounts.accountClass` | Account class | string | High-level classification of the account. |
| `xero.accounts.status` | Status | string | Whether the account is active in Xero. |

### Payments

Payments received and made, and how long invoices took to pay.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `xero.payments.amount` | Amount | SUM | Currency | Total payment amount in the payment currency. |
| `xero.payments.paymentCount` | Payments | COUNT_DISTINCT |  | Number of distinct payments in the result. |
| `xero.payments.daysToPay` | Days to pay | Calculated: Payment date − Invoice date |  | Average days between invoice date and payment date for invoices that have been paid. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `xero.payments.paymentId` | Payment ID | string | Unique identifier for the payment in Xero. |
| `xero.payments.invoiceId` | Invoice ID | string | Invoice this payment was applied against. |
| `xero.payments.contactId` | Contact ID | string | Contact associated with the payment. Falls back to the linked invoice contact when the payment row has no contactId. |
| `xero.payments.contactName` | Contact name | string | Display name of the contact on the payment. Falls back to the linked invoice contact when the payment row has no denormalised name. |
| `xero.payments.date` | Payment date | time | Date the payment was recorded in Xero. |
| `xero.payments.currencyCode` | Currency | string | Currency code for the payment amount. |

### Organisation

Your Xero organisation's details, base currency and financial year.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `xero.organisation.organisationCount` | Organisations | COUNT_DISTINCT |  | Number of distinct Xero organisations synced for the connection. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `xero.organisation.organisationId` | Organisation ID | string | Unique identifier for the Xero organisation. |
| `xero.organisation.name` | Organisation name | string | Trading name of the Xero organisation. |
| `xero.organisation.legalName` | Legal name | string | Legal entity name registered in Xero. |
| `xero.organisation.baseCurrency` | Base currency | string | Organisation base currency used for converted totals and P&L reporting. |
| `xero.organisation.countryCode` | Country | string | Country code for the Xero organisation. |
| `xero.organisation.financialYearEndMonth` | Financial year end month | number | Month (1-12) when the organisation financial year ends in Xero. |
| `xero.organisation.financialYearEndDay` | Financial year end day | number | Day of month when the organisation financial year ends in Xero. |
| `xero.organisation.updatedAt` | Updated at | time | Last time organisation settings were updated in Xero. |

### Profit and loss

Profit and loss report lines by section, account and period.

#### Metrics

| Field | Label | Rollup | Format | Description |
|---|---|---|---|---|
| `xero.profit_and_loss.amount` | Amount | SUM | Currency | P&L amount in organisation base currency from the Xero Reports API (tax-exclusive layout). Sum only with profit_and_loss.rowKind=account unless intentionally using a summary row. |
| `xero.profit_and_loss.rowCount` | P&L rows | COUNT_DISTINCT |  | Number of distinct P&L report rows. |

#### Dimensions

| Field | Label | Type | Description |
|---|---|---|---|
| `xero.profit_and_loss.reportId` | Report row ID | string | Stable primary key for a flattened P&L report row (section, account, period). |
| `xero.profit_and_loss.periodStart` | Period start | time | Start date of the P&L report period. |
| `xero.profit_and_loss.periodEnd` | Period end | time | End date of the P&L report period. |
| `xero.profit_and_loss.section` | Section | string | P&L section such as Income or Less Operating Expenses. |
| `xero.profit_and_loss.accountCode` | Account code | string | Chart of accounts code for the P&L row. |
| `xero.profit_and_loss.accountName` | Account name | string | Account name from the Xero P&L report. |
| `xero.profit_and_loss.rowKind` | Row kind | string | account for leaf account lines; summary for Xero subtotal rows such as Gross Profit or Net Profit. Filter rowKind=account when summing revenue or expense accounts to avoid double counting. |
| `xero.profit_and_loss.currencyCode` | Currency | string | Organisation base currency for P&L amounts. |
| `xero.profit_and_loss.date` | Report date | time | Primary reporting date for P&L rows (period end). |

## Links

- [Xero on jepto.com](https://www.jepto.com/data-sources/xero)
- [Jepto MCP README](https://github.com/jepto/mcp): connecting, tools, security
- [Data Model](https://www.jepto.com/help/data-model)
- [Connect a data source](https://www.jepto.com/help/connect-a-data-source)

<sub>Generated from the Jepto data model by jepto-datamodel-schema/scripts/build_source_pages.py. Don't edit by hand.</sub>
