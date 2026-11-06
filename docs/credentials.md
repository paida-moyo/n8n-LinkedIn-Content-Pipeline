# Credentials

Create each credential in n8n under Credentials, then attach it to the nodes listed. Do not commit any key, token, or client secret to this repository.

## Google Sheets (OAuth2)

- **Used by:** Read Next Topic, UpdateSheet, LogError, UpdateContentWithError
- **What it is:** an OAuth connection to the Google account that owns the content calendar.
- **Permissions:** read and write access to Google Sheets, and access to Drive so n8n can list your files.
- **Where to create it:** on n8n Cloud, use the Sign in with Google option in the credential. When self-hosting, create an OAuth client in Google Cloud Console, enable the Google Sheets API and the Google Drive API, and paste the client ID and secret into n8n.

## OpenAI API

- **Used by:** Message a model
- **What it is:** an API key for the OpenAI account that pays for the model calls.
- **Where to create it:** in the OpenAI platform dashboard under API keys. Paste the key into the n8n credential.

## Notion API

- **Used by:** ArchiveToNotion
- **What it is:** an internal integration secret for your Notion workspace.
- **Permissions:** the integration needs to read and insert content, and the target database must be shared with it.
- **Where to create it:** in Notion under Settings, Connections, Develop or manage integrations. Then open the database, use the menu to add a connection, and choose the integration.

## Gmail (OAuth2)

- **Used by:** Send a message, EmailAlert
- **What it is:** an OAuth connection to the Gmail account that sends the draft email and the failure alerts.
- **Permissions:** permission to send email.
- **Where to create it:** on n8n Cloud, use Sign in with Google. When self-hosting, enable the Gmail API in Google Cloud Console and create an OAuth client the same way as for Sheets.
