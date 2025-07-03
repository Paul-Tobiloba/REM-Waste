# 🧾 REM-Waste Lead Scoring Workflow

A smart lead automation system built with **n8n** to capture, qualify, and score leads from Typeform, notify the sales team via Slack for high-value opportunities, and trigger automated follow-ups via email — all using free-tier tools.

---

## 📥 Submission Form  
[Typeform Link](https://form.typeform.com/to/a0Ol7Z89)

---

## 🧩 Workflow Preview

![Lead Scoring Workflow in n8n](https://github.com/Paul-Tobiloba/REM-Waste/blob/a602b11fd8830b5acd1e9229a8d14530b51532ee/Screenshot%202025-07-03%20235733.png)

---

## 🎯 Objective

This workflow captures, qualifies, scores, and stores incoming leads from a Typeform. It then:
- Sends a Slack notification if the lead is classified as "Hot"
- Triggers a follow-up email (Gmail)

---

## ⚙️ Tech Stack

| Tool         | Purpose                          |
|--------------|----------------------------------|
| **n8n**      | Workflow orchestration           |
| **Typeform** | Lead capture                     |
| **Airtable** | Lead data storage                |
| **OpenAI / LangChain** | AI-based lead scoring     |
| **Slack**    | Team notification                |
| **Gmail**    | Email follow-up                  |

---

## 🔄 Workflow Overview

### 1. Trigger: Typeform Submission
- Listens for new responses via the **Typeform Trigger** node.
- Captures: `First name`, `Last name`, `Phone`, `Email`, `Budget`, `Interest Level`, `Company`, `Company Size`, `Notes`.

### 2. Preprocess Full Name
- Merges first and last name using the `Edit FullName` node.

### 3. Store Initial Lead
- Stores the lead in Airtable (`Lead Submissions` table) using the `Airtable1` node.

### 4. Generate Airtable Record URL
- Constructs a link using:
https://airtable.com/{baseId}/{tableId}/{recordId}


### 5. Validation
- Ensures required fields `full_name`, `email`, `budget`, and `interest_level` are not empty using an `If` node.

### 6. AI-Based Lead Scoring
- Uses **LangChain + OpenAI** agent with this prompt:
> Based on the following lead data, classify the lead as "Hot", "Warm", or "Cold":
> - Full Name: …
> - Budget: £…
> - Interest Level: …
> - Company Size: …

### 7. Attach Score
- Adds the returned classification as `score` via the `Edit Fields` node.

### 8. Update Airtable Record
- Updates the original Airtable record with the score using the `Airtable` node (via record `id`).

---

## 🔔 Smart Notification Logic

### 9. Slack Notification (Only for Hot Leads)
- If the score is `"Hot"`, send this Slack message:

🚨 New Hot Lead: {{full_name}}, Budget £{{budget}}, Interest: {{interest_level}} – [Check Airtable for details](airtable_url)

---

## 💬 Automated Lead Follow-Up

### 10. Wait
- Delays the follow-up messages by **2 minutes** using a `Wait` node.

### 11. Email Follow-Up (Gmail)
- Sends a thank-you email:

Hi {{full_name}}, thanks for reaching out to us!
We’ve received your details and we’re excited to learn more about your goals.
A team member will get in touch shortly. In the meantime, feel free to reply here if you have any questions or would like to book a quick chat.
– REM WASTE


---

## ✅ Key Highlights

| Feature             | Description                                            |
|---------------------|--------------------------------------------------------|
| **Validation**      | Ensures required fields are complete                   |
| **AI Qualification**| Dynamically scores leads as Hot/Warm/Cold             |
| **Smart Routing**   | Notifies Slack only for high-value leads              |
| **Real-time Storage** | Leads saved directly to Airtable                    |
| **Lead Engagement** | Sends follow-up email and WhatsApp message            |
| **URL Linking**     | Slack message includes direct Airtable record link    |

---

## 🧪 Sample Test Data

| Field           | Value                          |
|------------------|-------------------------------|
| **Name**         | Paul Oke                      |
| **Phone**        | +12015551234                  |
| **Email**        | paul@advancedcloudpartners.com |
| **Budget**       | More than $10,000             |
| **Interest**     | High                          |
| **Company**      | Simple Developers             |
| **Company Size** | 201–500                       |
| **Notes**        | tested                        |

---



