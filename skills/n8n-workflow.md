# n8n Workflow Design

This skill provides best practices for building n8n workflows.

## Core Principles
- Validate a lean working version before adding complexity
- Reuse existing credentials across workflows
- Use Code nodes for data transformation logic
- Always set timezone to Australia/Sydney

## HTTP Request Nodes
- Use Raw body mode (not JSON mode) when body contains n8n expressions
- Add Accept-Encoding: identity header to prevent gzip issues with downstream Code nodes
- Enable Retry On Fail (3 retries, 5s intervals) for external APIs

## Accessing Data from Non-Adjacent Nodes
Use: $('NodeName').first().json
NOT: items[0].json

## Telegram Formatting
- Plain text is the stable default — avoid Markdown parsing
- Chunk long messages in Code nodes rather than relying on prompt-level limits

## Error Handling
- Leave Error Workflow field blank on error-monitor workflows to prevent loops
- Use HTTP error branches to handle API failures gracefully

## Credentials
- Supabase: use Secret key, not Publishable key
- Google Sheets OAuth2 redirect URI: https://arso.apps.n8nitro.com/rest/oauth2-credential/callback
```

4. **"Commit new file"**

---

Once both files are committed, your repo will look like:
```
jarvis-skills/
  README.md
  skills/
    find-skills.md
    n8n-workflow.md
