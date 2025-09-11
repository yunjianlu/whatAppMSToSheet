# Credential & API Key Setup Guide for whatAppMSToSheet Workflow

This guide explains how to set up the required credentials and API keys for the workflow nodes in n8n.

---

## 1. WhatsApp API Credentials

- **Used by:** Whatsapp Chat (Trigger), Get Audio Link, Get Image Link, Dowload Audio, Dowload Image
- **Credential Type:** WhatsApp OAuth / WhatsApp Business API
- **How to set up:**
  1.  In n8n, go to **Credentials** > **Create New** > search for `WhatsApp`.
  2.  Choose the correct credential type (OAuth or API key, as required by your provider).
  3.  Enter your WhatsApp Business API details (API URL, API Key/Token, Phone Number, etc.).
  4.  Save and test the connection.

---

## 2. Google Sheets OAuth2 Credentials

- **Used by:** Append Incoive, Append Notes, Append Current Address, Append Final Address
- **Credential Type:** Google Sheets OAuth2 API
- **How to set up:**
  1.  In n8n, go to **Credentials** > **Create New** > search for `Google Sheets`.
  2.  Select `Google Sheets OAuth2 API`.
  3.  Click **Connect OAuth2 Account** and follow the Google sign-in prompts.
  4.  Grant access to the required Google account.
  5.  Save and test the connection.

---

## 3. OpenAI API Credentials

- **Used by:** Recording to Text (OpenAI)
- **Credential Type:** OpenAI API
- **How to set up:**
  1.  Sign up or log in at [OpenAI](https://platform.openai.com/).
  2.  Go to **API Keys** and create a new secret key.
  3.  In n8n, go to **Credentials** > **Create New** > search for `OpenAI`.
  4.  Enter your API key and save.

---

## 4. Google Gemini (PaLM) API Credentials

- **Used by:** Google Gemini Chat Model
- **Credential Type:** Google Gemini (PaLM) API
- **How to set up:**
  1.  Go to [Google AI Studio](https://aistudio.google.com/) or your Google Cloud Console.
  2.  Create or access your Gemini/PaLM API key.
  3.  In n8n, go to **Credentials** > **Create New** > search for `Google Gemini` or `Google PaLM`.
  4.  Enter your API key and save.

---

> **Tip:** After creating credentials, assign them to the relevant nodes in your workflow. Always test each credential in n8n to ensure it is working before running the workflow.
