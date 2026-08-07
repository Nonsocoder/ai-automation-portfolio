# Email Classifier + AI Reply Agent

An n8n automation that watches an inbox, filters out noise, and drafts context-aware replies to support emails using a RAG (Retrieval-Augmented Generation) pipeline.

## Problem

Agencies and small businesses get a mix of promotional junk and real customer support requests in one inbox. Manually triaging every email wastes time, and support replies often require digging through docs/FAQs for the right answer.

## What it does

- **Gmail Trigger** — polls the inbox every minute for new mail
- **Email Body** — extracts the email snippet/content
- **Classify Email** — an LLM (via OpenRouter, DeepSeek) sorts each email into `promotional` or `Support Service`
  - Promotional emails are dropped automatically
  - Support emails are passed to an AI Agent that:
    - Queries a Supabase vector store for relevant knowledge-base context
    - Generates a reply grounded in that context
    - Creates a Gmail draft (not auto-sent — a human still reviews before sending)

## Tech used

- n8n (workflow engine)
- OpenRouter (DeepSeek Chat v3) — classification + agent reasoning
- Supabase Vector Store — knowledge base retrieval
- Google Gemini — embeddings
- Gmail API — trigger + draft creation

## Notes

- Drafts rather than sends replies, so nothing goes out without human sign-off — an easy safety feature to point out to clients.
- The classifier categories (promotional / Support Service) are easily extended — add more categories for routing to different departments.

## Setup to reuse

1. Import `Email_Classifier.json` into n8n.
2. Connect your own Gmail OAuth2, OpenRouter, Supabase, and Google Gemini credentials.
3. Populate the Supabase `documents` table with your knowledge base content.
