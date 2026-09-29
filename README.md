# Invoice Ninja MCP Server

An MCP (Model Context Protocol) server for [Invoice Ninja](https://invoiceninja.com) v5. Create invoices, manage clients, record payments, and more through Claude.

## ⚠️ Security Notice

**Never commit API tokens or credentials to version control.** This repository's Git history may contain exposed credentials. If you are using this project:

1. Review the [SECURITY.md](./SECURITY.md) guide
2. Assume any tokens in public forks are compromised
3. Rotate your Invoice Ninja API tokens immediately
4. Use only fresh, restricted tokens generated after this date

## Features

26 tools across 7 categories:

**System**
- `test-connection` - Verify API credentials and instance connectivity

**Invoices** (primary)
- `list-invoices` - List/filter/search invoices
- `get-invoice` - Get full invoice details with line items
- `create-invoice` - Create draft invoices
- `update-invoice` - Update invoice fields and line items
- `delete-invoice` - Soft-delete invoices
- `send-invoice-email` - Email invoices to clients
- `bulk-invoice-action` - Mark sent/paid, archive, cancel, clone to quote

**Clients**
- `list-clients` - List/search clients
- `get-client` - Get client details with contacts
- `create-client` - Create clients with contacts
- `update-client` - Update client details
- `delete-client` - Soft-delete clients

**Products**
- `list-products` - List products for line item reference
- `get-product` - Get product details and pricing

**Payments**
- `list-payments` - List payment records
- `get-payment` - Get payment details
- `create-payment` - Record payments against invoices

**Quotes**
- `list-quotes` - List quotes
- `get-quote` - Get quote details
- `create-quote` - Create quotes (same structure as invoices)
- `update-quote` - Update quotes
- `bulk-quote-action` - Approve, convert to invoice, mark sent

**Expenses**
- `list-expenses` - List expenses
- `get-expense` - Get expense details
- `create-expense` - Create expense records

## Configuration

You need two things:

1. **Invoice Ninja URL** - Your Invoice Ninja instance URL (e.g., `https://invoicing.co` or your self-hosted URL)
2. **API Token** - Generate from Invoice Ninja: Settings > Account Management > API Tokens
   - **Use a restricted token with minimal necessary permissions**
   - Do not use admin or super-user tokens

## Installation

### Claude Desktop

Add to your Claude Desktop config file:

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "invoice-ninja": {
      "command": "npx",
      "args": ["-y", "invoice-ninja-mcp-server"],
      "env": {
        "INVOICE_NINJA_URL": "https://your-instance.invoicing.co",
        "INVOICE_NINJA_API_TOKEN": "your_api_token_here"
      }
    }
  }
}
```

Then restart Claude Desktop.

### Claude Code

```bash
claude mcp add invoice-ninja \
  -e INVOICE_NINJA_URL=https://your-instance.invoicing.co \
  -e INVOICE_NINJA_API_TOKEN=your_api_token_here \
  -- npx -y invoice-ninja-mcp-server
```

### Cursor

Add to `.cursor/mcp.json` in your project root (or `~/.cursor/mcp.json` for global):

```json
{
  "mcpServers": {
    "invoice-ninja": {
      "command": "npx",
      "args": ["-y", "invoice-ninja-mcp-server"],
      "env": {
        "INVOICE_NINJA_URL": "https://your-instance.invoicing.co",
        "INVOICE_NINJA_API_TOKEN": "your_api_token_here"
      }
    }
  }
}
```

## Development

### Prerequisites

- Node.js >= 18
- pnpm

### Setup

```bash
pnpm install
pnpm run build
```

### Watch mode

```bash
pnpm run watch
```

### Testing locally

Create a `.env` file in the project root with your credentials (`.env` is in `.gitignore` and will not be committed):

```
INVOICE_NINJA_URL=https://your-instance.invoicing.co
INVOICE_NINJA_API_TOKEN=your_api_token_here
```

Then point your MCP config to the local build:

```json
{
  "mcpServers": {
    "invoice-ninja": {
      "command": "node",
      "args": ["/path/to/invoice-ninja-mcp/dist/index.js"],
      "env": {
        "INVOICE_NINJA_URL": "https://your-instance.invoicing.co",
        "INVOICE_NINJA_API_TOKEN": "your_api_token_here"
      }
    }
  }
}
```

## Example Workflow

1. "List my clients" -> `list-clients`
2. "Create an invoice for Acme Corp with 10 hours of consulting at $150/hr" -> `list-clients` (find ID) -> `create-invoice`
3. "Send that invoice" -> `send-invoice-email`
4. "Mark it as paid" -> `bulk-invoice-action` with `mark_paid`

## License

MIT
