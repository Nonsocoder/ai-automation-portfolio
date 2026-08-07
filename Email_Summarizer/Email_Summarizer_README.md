# Email Summarizer

An n8n automation that summarizes incoming emails with an LLM and distributes the summaries via Gmail, Telegram, and/or Google Sheets — including a scheduled digest path.

## Problem

A busy inbox is hard to scan quickly. Long emails and threads take time to read even when the actionable content is a sentence or two.

## What it does

- **Gmail Trigger** — fires on new incoming mail
- **AI Agent** (OpenRouter) — summarizes the email snippet, returning only the summary text
- **Send a text message (Telegram)** / **Send a message (Gmail)** — delivers the summary immediately
- **Append row in sheet (Google Sheets)** — logs the summary for later reference
- A parallel **Schedule Trigger** path reads back the day's logged rows from Google Sheets, filters/aggregates them, and sends a digest-style message
- A **Google Drive Trigger** branch also watches for new files, extracts their content, and runs it through an Information Extractor for structured summarization

## Tech used

- n8n (workflow engine)
- OpenRouter — summarization
- Gmail API — trigger + send
- Telegram — instant notification
- Google Sheets — logging + digest source
- Google Drive — file-based summarization branch

## Notes

- The system prompt is written to return only the summary text, with no filler like "let me know if you need anything else" — worth keeping if you reuse the prompt elsewhere.
- The Google Sheets log doubles as an audit trail and a source for the scheduled digest.

## Setup to reuse

1. Import `Email_Summarizer.json` into n8n.
2. Connect your own Gmail OAuth2, OpenRouter, Telegram, and Google Sheets/Drive credentials.
3. Point the Google Sheets nodes at your own spreadsheet, and set the Telegram chat ID for notifications.
4. Adjust the Schedule Trigger to match how often you want digest sends.
