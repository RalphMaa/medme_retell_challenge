# MedMe Take-Home Challenge – Appointment Scheduling Bot

## 📌 Overview
This project implements an **Appointment Scheduling Bot** using **Retell AI** integrated with **Cal.com**, **Google Sheets**, and **Make.com**.  
It enables patients to **book, reschedule, or cancel appointments** through a natural, conversational interface, while maintaining a persistent booking log.

---

## 🏗 Architecture & Key Components

**1. Retell AI Agent** – Voice/text conversational agent that:
- Collects patient info (Name, Email, Date/Time, Appointment Type) in a caring tone.
- Triggers:
  - `check_availability` → Fetches available slots from Cal.com.
  - `book_appointment` → Books, cancels, or reschedules appointments with edge-case handling.
  - `log_appointment_info` → Sends booking details to Google Sheets.

**2. Make.com Scenarios** – Low-code workflows integrating APIs:
- **Check Availability** – Converts natural-language dates to ISO 8601 (via GPT-4o-mini) and queries Cal.com.
- **Book Appointment** – Handles bookings, cancellations, and reschedules in a single flow, including unknown/multiple/zero match cases.
- **Log Appointment Info** – Sends appointment data to Google Sheets.

**3. External Services**
- **Cal.com API** – Appointment scheduling & calendar operations.
- **Google Sheets** – Persistent, shareable booking logs.
- **OpenAI GPT-4o-mini** – Cost-effective, accurate date/time parsing.
- **OpenAI GPT-5-mini** – Natural, engaging conversational AI for Retell AI.

**4. Data Flow**

User → Retell AI
  → check_availability (Make.com Webhook) → Cal.com
  → Retell AI → book_appointment (Make.com Webhook) → Cal.com
  → Retell AI → log_appointment_info (Make.com Webhook) → Google Sheets

---
## 🛠 Tools & Libraries

| Tool / Service      | Purpose & Reason                                                                 |
|---------------------|----------------------------------------------------------------------------------|
| **Retell AI**       | Voice-ready conversational interface with function calling.                      |
| **Make.com**        | Fast, low-code integration between APIs.                                         |
| **Cal.com API**     | REST endpoints for checking, booking, rescheduling, and cancelling appointments. |
| **OpenAI GPT-4o-mini** | Efficient, inexpensive date/time parsing.                                       |
| **Google Sheets**   | Collaborative, persistent appointment logging.                                   |
| **OpenAI GPT-5-mini** | High-quality conversation flow with balanced cost & performance.                 |

---

## 🔌 Integration Example

**Log Appointment Info to Google Sheets** (via Make.com webhook):

POST https://hook.us2.make.com/...
{
  "patient_email": "user@example.com",
  "patient_name": "User Name",
  "date_time": "2025-08-10T15:00:00Z",
  "status": "New",
  "agent_notes": "Requested checkup"
}

This webhook triggers Make.com to add the data as a new row in the Google Sheet.

---
## ⚠ Error Handling
- **Calendar API failure** – Return apology, offer human follow-up.
- **Unavailable slot** – Suggest alternative times.
- **No matching booking** – Reconfirm name/email; handle single, multiple, or zero matches.
- **Missing info** – Ask for up to **two missing fields at a time** (preferably one question at a time).
- **Google Sheets write error** – Log silently without breaking user flow.

---

## 🚀 If I Had More Time
- Implement Retell AI’s **Conversational Flow Agent** for better handling of unknown appointment times.
- Assign **unique appointment IDs** instead of name/email matching.
- Enhance memory to avoid re-asking known details.
- Refine tone for consistency.
- Conduct additional edge-case testing.

---

## 🧪 Testing Tips
- Pre-populate calendar with busy/free slots to test success and fallback flows.
- Simulate mismatched name/email and unknown appointment times.
- Verify booking logs update after every booking, reschedule, or cancellation.

---

## 📂 Supporting Material
- **Deployed Demo:** [Live Chatbot](https://ralphmaa.github.io/medme_retell_challenge/) (chat bubble in bottom-right)
- **Google Sheet Log:** [Live Booking Log](https://docs.google.com/spreadsheets/d/1o52Cz1DAp6u852UwiYu0ITEeD96gA_Ra_gze8zJts7E/edit?usp=sharing)
- **Retell AI Configs:** JSON file in `agent_json_configs` folder.
- **Make.com Workflows:** JSON files in `agent_json_configs`.
