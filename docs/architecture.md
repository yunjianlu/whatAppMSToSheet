# Architecture

This document describes the high-level architecture and flow of the n8n invoice processing workflow.

---

## 🗺️ High-Level Architecture

```mermaid
flowchart TD
  A[WhatsApp Message Received] --> B{Contents Condition}
  B -->|Voice| C1[Get Voice Content]
  B -->|Image| C2[Get Image Content]
  B -->|Address (Text)| C3[Get Address Content]
  B -->|Location| C4[Get Location Content]
  C1 --> D[AI Analysis/Extraction/Formatting]
  C2 --> D
  C3 --> D
  C4 --> D
  D --> E[Store in Database (Google Sheets)]
```

---

## 📦 Layers & Flow

### 1. Input Layer (User Interaction)

- **WhatsApp Chat:** Users send messages (text, audio, images, or location) to a phone number you define.

### 2. Message Routing Layer

- **Split Out:** Splits message into multiple events if needed.
- **Switch (Rules Engine):** Directs messages by type:
  - Audio
  - Location
  - Image
  - Text

### 3. Processing Layer (Based on Type)

- **Audio Flow:**
  - Get Audio Link → Download Audio → Transcribe (Recording to Text) → Append Notes (Google Sheets)
- **Location Flow:**
  - Geocode Address → Extract Address → Append Current Address (Google Sheets)
- **Image Flow:**
  - Get Image Link → Download Image → AI Agent → Jsonize Code → Append Invoice (Google Sheets)
- **Text Flow:**
  - Extract Company Name → Append Final Address (Google Sheets)

### 4. AI & Memory Layer

- **AI Agent (Gemini Chat Model + Simple Memory):** Handles interpretation, invoice extraction, and structured JSON formatting.

### 5. Storage Layer

- **Google Sheets (Multiple Sheets):**
  - Notes (from audio)
  - Current Address (from location)
  - Invoice (from images via AI)
  - Final Address (from text/company name)

---

## 📝 Notes

- The workflow is modular and can be extended to support more message types or integrations.
- Error handling and notifications are built into each processing branch.

---

_For more details, see the workflow JSON and README._

```

```
