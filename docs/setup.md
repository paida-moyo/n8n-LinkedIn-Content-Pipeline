# Setup

## Before you start

You need accounts for Google (Sheets and Gmail), OpenAI, Notion, and n8n. Credential details are in [credentials.md](credentials.md).

## Steps

1. Open n8n. Use n8n Cloud, or install n8n yourself if you prefer to self-host.
2. Create the Google Sheet. Add two tabs named `Content` and `Errors`. Paste the headers from `sample-data/content-calendar-template.csv` into `Content` and from `sample-data/errors-template.csv` into `Errors`. The header names are case sensitive and must match exactly.
3. Create the Notion database with these properties: the title property (any name), `Angle` (text), `Day` (number), `Status` (select, with an option named `drafted`), and `Draft` (text). Share the database with your Notion integration.
4. In n8n, create a new workflow, open the menu, choose Import from File, and select `workflow-error-handler.json`. Save it.
5. Create another new workflow and import `workflow.json` the same way.
6. Create the four credentials listed in [credentials.md](credentials.md) and attach them to the nodes in the table below.
7. Replace each placeholder in the table below.
8. Open the settings of the main workflow and set Error Workflow to the error handler you imported in step 4.
9. Add a row to `Content` with `Status` set to `idea` and `Date Scheduled` set to today in `dd/MM/yyyy` format. Use a unique `Day` value.
10. Run the main workflow once with Execute Workflow. Check that the row changes to `drafted`, a Notion page appears, and the email arrives. Compare the shape of the result with `sample-data/output-example.json`.
11. Activate both workflows.

## Placeholders to replace

| File | Node | Field | Placeholder |
| --- | --- | --- | --- |
| workflow.json | Read Next Topic, UpdateSheet | Document | `YOUR_GOOGLE_SHEET_ID` |
| workflow.json | Read Next Topic, UpdateSheet | Credential | `YOUR_GOOGLE_SHEETS_CREDENTIAL` |
| workflow.json | Message a model | Credential | `YOUR_OPENAI_CREDENTIAL` |
| workflow.json | ArchiveToNotion | Database | `YOUR_NOTION_DATABASE_ID` |
| workflow.json | ArchiveToNotion | Credential | `YOUR_NOTION_CREDENTIAL` |
| workflow.json | Send a message | To | `YOUR_EMAIL@example.com` |
| workflow.json | Send a message | Sender name | `YOUR_NAME` |
| workflow.json | Send a message | Sheet link in the body | `YOUR_GOOGLE_SHEET_ID` |
| workflow.json | Send a message | Notion link in the body | `https://www.notion.so/YOUR_NOTION_DATABASE_URL` |
| workflow.json | Send a message | Credential | `YOUR_GMAIL_CREDENTIAL` |
| workflow.json | Workflow settings | Error Workflow | `YOUR_ERROR_WORKFLOW_ID` |
| workflow-error-handler.json | ExtractContext | `execution_url` host | `YOUR_N8N_HOST` |
| workflow-error-handler.json | LogError, UpdateContentWithError | Document | `YOUR_GOOGLE_SHEET_ID` |
| workflow-error-handler.json | LogError, UpdateContentWithError | Credential | `YOUR_GOOGLE_SHEETS_CREDENTIAL` |
| workflow-error-handler.json | EmailAlert | To | `YOUR_EMAIL@example.com` |
| workflow-error-handler.json | EmailAlert | Credential | `YOUR_GMAIL_CREDENTIAL` |

The sheet tab names are set to `Content` and `Errors` by name. If you rename the tabs, update the Sheet field in each Google Sheets node.

## Troubleshooting

- **Google nodes say the credential is invalid or expired.** Reconnect the Google credential in n8n. If your Google OAuth app is in Testing status, Google can expire the token after about seven days.
- **Read Next Topic returns nothing.** Check that `Status` is exactly `idea`, that `Date Scheduled` is today, and that the text in the cell matches `dd/MM/yyyy`. A date stored as a real date value can display differently from the text the filter expects.
- **UpdateSheet does not change the row.** The update matches on `Day`. The value must exist in the sheet and be unique.
- **A Sheets node reports a missing column.** The headers in your tabs must match `sample-data/content-calendar-template.csv` and `sample-data/errors-template.csv` exactly, including case.
- **ArchiveToNotion fails.** Confirm the database is shared with your integration and that the property names and types match step 3.
- **The model node fails with a rate limit or quota message.** That is an OpenAI account limit. The error workflow will log it as transient if the HTTP code is 429 or 500 or higher.
- **Nothing runs overnight.** The Schedule Trigger only fires when the workflow is active. Manual runs use today's date for the sheet lookup.
- **No alert email arrives.** Confirm the Error Workflow is set in the main workflow's settings and that both workflows are active.
