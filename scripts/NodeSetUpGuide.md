# Node Setup Guide for whatAppMSToSheet Workflow

This guide explains how to configure each node in the workflow. Follow the steps for each node to ensure correct setup and operation.

---

## 1. Whatsapp Chat (Trigger)

- **Type:** `n8n-nodes-base.whatsAppTrigger`
- **Credentials:** WhatsApp OAuth account (e.g., "WhatsApp OAuth account 2")
- **Key Parameters:**
  - `updates`: ["messages"]
  - `options.messageStatusUpdates`: ["all"]
- **Notes:** This node triggers the workflow on new WhatsApp messages.

---

## 2. Split Out

- **Type:** `n8n-nodes-base.splitOut`
- **Key Parameters:**
  - `fieldToSplitOut`: `messages`
- **Notes:** Splits incoming WhatsApp messages for further processing.

---

## 3. Switch

- **Type:** `n8n-nodes-base.switch`
- **Key Parameters:**
  - Multiple rules to route messages by type (audio, image, location, text).
- **Notes:** Directs flow based on message content type.

---

## 4. Get Audio Link

- **Type:** `n8n-nodes-base.whatsApp`
- **Credentials:** WhatsApp BS account sys user
- **Key Parameters:**
  - `resource`: `media`
  - `operation`: `mediaUrlGet`
  - `mediaGetId`: `={{ $json.audio.id }}`
- **Notes:** Retrieves the download link for audio messages.

---

## 5. Get Image Link

- **Type:** `n8n-nodes-base.whatsApp`
- **Credentials:** WhatsApp BS account sys user
- **Key Parameters:**
  - `resource`: `media`
  - `operation`: `mediaUrlGet`
  - `mediaGetId`: `={{ $json.image.id }}`
- **Notes:** Retrieves the download link for image messages.

---

## 6. Dowload Audio

- **Type:** `n8n-nodes-base.httpRequest`
- **Credentials:** WhatsApp BS account sys user
- **Key Parameters:**
  - `url`: `={{ $json.url }}`
  - `authentication`: `predefinedCredentialType`
  - `nodeCredentialType`: `whatsAppApi`
- **Notes:** Downloads the audio file from WhatsApp.

---

## 7. Dowload Image

- **Type:** `n8n-nodes-base.httpRequest`
- **Credentials:** WhatsApp BS account sys user
- **Key Parameters:**
  - `url`: `={{ $json.url }}`
  - `authentication`: `predefinedCredentialType`
  - `nodeCredentialType`: `whatsAppApi`
- **Notes:** Downloads the image file from WhatsApp.

---

## 8. Recording to Text (OpenAI)

- **Type:** `@n8n/n8n-nodes-langchain.openAi`
- **Credentials:** OpenAI account
- **Key Parameters:**
  - `resource`: `audio`
  - `operation`: `transcribe`
- **Notes:** Transcribes audio to text using OpenAI.

---

## 9. Google Gemini Chat Model

- **Type:** `@n8n/n8n-nodes-langchain.lmChatGoogleGemini`
- **Credentials:** Google Gemini(PaLM) Api account
- **Key Parameters:**
  - `modelName`: `models/gemini-1.5-flash`
- **Notes:** Provides AI chat/completion using Gemini.

---

## 10. AI Agent

- **Type:** `@n8n/n8n-nodes-langchain.agent`
- **Key Parameters:**
  - `promptType`: `define`
  - `text`: Custom prompt for extracting invoice data.
- **Notes:** Extracts structured data from invoice text.

---

## 11. Simple Memory

- **Type:** `@n8n/n8n-nodes-langchain.memoryBufferWindow`
- **Key Parameters:**
  - `sessionIdType`: `customKey`
  - `sessionKey`: `={{ $('Whatsapp Chat').item.json.contacts[0].wa_id }}`
- **Notes:** Maintains conversational memory per WhatsApp contact.

---

## 12. Jsonize Code1 (Code)

- **Type:** `n8n-nodes-base.code`
- **Key Parameters:**
  - Custom JavaScript to clean and map AI output to final JSON structure.
- **Notes:** Adjusts and formats extracted invoice data for Google Sheets.

---

## 13. Geocode Address (HTTP Request)

- **Type:** `n8n-nodes-base.httpRequest`
- **Key Parameters:**
  - `url`: Uses OpenStreetMap Nominatim API for reverse geocoding.
- **Notes:** Converts latitude/longitude to a human-readable address.

---

## 14. Extract Address (Code)

- **Type:** `n8n-nodes-base.code`
- **Key Parameters:**
  - Custom JavaScript to format and store the address.
- **Notes:** Stores the formatted address in workflow static data.

---

## 15. Append Incoive (Google Sheets)

- **Type:** `n8n-nodes-base.googleSheets`
- **Credentials:** GSheets ND Account
- **Key Parameters:**
  - `operation`: `append`
  - `documentId`: Invoice spreadsheet ID
  - `sheetName`: `={{ $json.SheetName }}`
  - `columns`: Auto-mapped from input data
- **Notes:** Appends invoice data to the correct sheet.

---

## 16. Append Notes (Google Sheets)

- **Type:** `n8n-nodes-base.googleSheets`
- **Credentials:** GSheets ND Account
- **Key Parameters:**
  - `operation`: `append`
  - `documentId`: Notes spreadsheet ID
  - `sheetName`: `gid=0`
  - `columns`: `text`, `datetime`, etc.
- **Notes:** Appends transcribed notes to the log sheet.

---

## 17. Append Current Address (Google Sheets)

- **Type:** `n8n-nodes-base.googleSheets`
- **Credentials:** GSheets ND Account
- **Key Parameters:**
  - `operation`: `append`
  - `documentId`: Mileage table spreadsheet ID
  - `sheetName`: `NiceD`
  - `columns`: `Start_Address`, etc.
- **Notes:** Appends the current address to the mileage table.

---

## 18. Get Company Name (Code)

- **Type:** `n8n-nodes-base.code`
- **Key Parameters:**
  - Custom JavaScript to extract company name and address from message text.
- **Notes:** Maps the last character of the message to a company.

---

## 19. Append Final Address (Google Sheets)

- **Type:** `n8n-nodes-base.googleSheets`
- **Credentials:** GSheets ND Account
- **Key Parameters:**
  - `operation`: `append`
  - `documentId`: Mileage table spreadsheet ID
  - `sheetName`: `={{ $json.Company }}`
  - `columns`: Auto-mapped from input data
- **Notes:** Appends the final address and company to the mileage table.

---

> **Tip:** For all Google Sheets and WhatsApp nodes, ensure the correct credentials are set up in n8n under "Credentials" and have the necessary API access.
