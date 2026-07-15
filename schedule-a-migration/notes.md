# Contract Schedule A Fee Rate Extraction (Notes)

## What it does

Processes Schedule A contract PDFs uploaded to Google Drive. When a file lands, the pipeline runs a 6-stage LLM chain to pull out structured fee rate data, loads the results to PostgreSQL and Google Sheets, and sends Gmail notifications to stakeholders.

## Business context

CloudXSystems manages 26+ Schedule A contracts, each with fee rate tables buried in PDFs. Extracting and loading that data was done by hand. This workflow does it automatically.

## Migration context

Migrated from a 30-node Zapier workflow to self-hosted n8n as part of CloudX's cost-reduction program (Zapier charges per task; n8n is self-hosted). The migration took under half a day with no regression in business logic.

A second phase converted all 7 HTTP Request Claude nodes to native `@n8n/n8n-nodes-langchain.anthropic` nodes, taking the workflow from v4 to v8.

## Technical highlights

- 30 nodes, behavior identical to the Zapier original
- AI provider switched from OpenAI GPT-4o/mini to Claude Haiku via native Anthropic node
- PDF processing: replaced a 4-step PDF.co chain (OCR, rotate, convert, fetch) with n8n native binary file handling
- Zapier Python steps converted to JavaScript for n8n Code nodes
- Integrations: Google Drive, Google Sheets, Gmail, PostgreSQL, Anthropic API
- Output path `$json.content[0].text` is the same for both HTTP Request and native Anthropic node types, so no downstream nodes needed changes when swapping

## Scale

- 26+ Schedule A contracts processed
- Live in production on a self-hosted n8n instance

## Proof (available on request)

The exported n8n workflow (v8, final) plus workflow canvas and execution-log screenshots are kept privately and available to prospective employers on request. The public version omits the workflow file because it contains embedded credentials and internal endpoints.
