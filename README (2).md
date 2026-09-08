# AI Invoice Processing & Risk Analysis Agent

An n8n automation workflow that receives an invoice PDF, extracts its text, uses an AI model to analyze the invoice, assigns a risk level, and stores the result in Google Sheets.

## Project Overview

**Invoice PDF → Text Extraction → AI Analysis → Structured Data → Risk Routing → Google Sheets**

The project automates basic invoice processing and provides a simple AI-assisted risk classification.

## Workflow

1. **Webhook** — receives an invoice PDF through a POST request. Webhook path: `invoice-upload`
2. **Extract From File** — extracts readable text from the uploaded PDF.
3. **AI Agent** — analyzes the extracted invoice text.
4. **Groq Chat Model** — provides the language model. Model used during testing: `openai/gpt-oss-120b`
5. **Structured Output Parser** — keeps the AI result in a consistent structure.
6. **Switch** — routes invoices into LOW, MEDIUM, or HIGH risk.
7. **Google Sheets** — appends the result to `Invoices` → `Sheet1`.

## Extracted Invoice Fields

| Field | Description |
|---|---|
| `vendor_name` | Name of the invoice vendor |
| `invoice_number` | Invoice/reference number |
| `invoice_date` | Invoice date |
| `due_date` | Payment due date |
| `subtotal` | Invoice subtotal |
| `tax` | Tax amount |
| `total_amount` | Final invoice amount |
| `currency` | Invoice currency |
| `risk_level` | LOW, MEDIUM, or HIGH |

## Risk Levels

- **LOW** — invoice appears normal based on the available information.
- **MEDIUM** — invoice may require additional review.
- **HIGH** — invoice contains stronger indicators that it should be reviewed carefully.

> Risk classification is AI-assisted and should be reviewed by a human before important financial decisions.

## Google Sheets Output

The `Invoices` sheet uses these columns:

`vendor_name | invoice_number | invoice_date | due_date | subtotal | tax | total_amount | currency | risk_level`

## Technologies Used

- n8n
- Groq
- AI Agent
- Structured Output Parser
- Google Sheets
- Webhook
- PDF text extraction

## Example Test Invoice

A text-based test invoice was used during development:

- Vendor: ABC SUPPLIES
- Invoice Number: INV-2026-001
- Invoice Date: 12 August 2026
- Due Date: 11 September 2026
- Currency: USD
- Subtotal: 3700
- Tax: 370
- Total Amount: 4070

## Testing

For a local n8n test, PowerShell can send the PDF to the webhook:

```powershell
curl.exe -X POST -F "data=@$env:USERPROFILE\Downloads\test_invoice_ABC_Supplies_text.pdf" "http://localhost:5678/webhook-test/invoice-upload"
```

Successful webhook submission returns:

```text
{"message":"Workflow was started"}
```

The processed invoice should then appear in Google Sheets.

## Security

**Never upload API keys, passwords, OAuth tokens, or private credentials to GitHub.**

Before exporting/uploading an n8n workflow, check that no secrets are included.

## How to Import

If the n8n workflow JSON is included:

1. Open n8n.
2. Use the workflow import option.
3. Select the JSON file.
4. Reconnect your own credentials if required.
5. Test with an invoice PDF.

## Project Status

- [x] Webhook configured
- [x] PDF text extraction configured
- [x] AI Agent configured
- [x] Groq Chat Model connected
- [x] Structured Output Parser configured
- [x] LOW / MEDIUM / HIGH routing configured
- [x] Google Sheets connected
- [x] End-to-end invoice test completed

## Future Improvements

- OCR support for scanned/image-only invoices
- Duplicate invoice detection
- More detailed fraud/risk rules
- Email alerts for HIGH-risk invoices
- Automatic invoice archiving
- Vendor history and risk tracking
- Dashboard/reporting

## Author

**Rimsha Batool**

AI-focused automation project built with n8n.
