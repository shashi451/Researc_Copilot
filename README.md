# Researc_Copilot
This project builds an automated Research Copilot using n8n. It fetches webpage content, extracts readable text, generates an AI summary, and sends the result via email.
<img width="1500" height="406" alt="image" src="https://github.com/user-attachments/assets/4c4e1dfc-5ccd-40d9-8b97-9defa8074480" />

Here is a clean, ready-to-paste README for your Git repo 👇


## 🚀 Workflow Overview

The automation follows this pipeline:

```
Manual Trigger → HTTP Request → HTML Extract → Basic LLM Chain → Gmail Send Message
```

### 🔹 What it does

* Scrapes a webpage
* Cleans HTML into readable text
* Generates a 5-point AI summary
* Emails the summary automatically

---

## 🧩 Nodes Used

### 1️⃣ Manual Trigger

Starts the workflow manually for testing.

**Purpose:** Entry point for the pipeline.

---

### 2️⃣ HTTP Request

Fetches raw HTML from the target webpage.

**Key settings:**

* Method: `GET`
* Response Format: `Text`
* Example URL: `https://www.amazon.in/`

**Output field:**

```
$json.data
```

---

### 3️⃣ HTML Node (Extract HTML Content)

Extracts readable text from the webpage body.

**Configuration:**

* Input: JSON
* JSON Property: `data`
* CSS Selector: `body`
* Return Value: `Text`
* Key: `content`

**Output field:**

```
$json.content
```

---

### 4️⃣ Basic LLM Chain

Generates the AI summary of the scraped content.

**Model:** OpenAI Chat Model (e.g., gpt-4o-mini)

**Prompt (Expression mode):**

```javascript
={{"Summarize the following webpage content in 5 clear bullet points.\nKeep it short and simple.\n\nCONTENT:\n" + $json.content}}
```

**Output field:**

```
$json.text
```

---

### 5️⃣ Gmail — Send a Message

Sends the AI summary to the configured email.

**Important settings:**

* Email Type: `Text`
* Subject: `Research Copilot Summary`
* Message Body (Expression):

```javascript
={{$json["text"]}}
```

---

## ⚙️ Prerequisites

Before running the workflow, ensure:

* n8n installed and running
* OpenAI credentials configured
* Gmail/SMTP credentials configured
* Internet access enabled

---

## 🧪 How to Run

1. Import the workflow into n8n
2. Update the target URL in HTTP Request
3. Configure OpenAI credentials
4. Configure Gmail credentials
5. Click **Execute Workflow**

You should receive the summary email.

---

## 📬 Expected Output

The email will contain:

* Clean AI summary
* 5 bullet points
* Key information from the webpage

---

## 🔧 Troubleshooting

### ❗ No content in summary

Check:

* HTML node output contains `content`
* Prompt is in Expression mode
* Model is connected

---

### ❗ Email not received

Check:

* Gmail credentials
* Spam folder
* Email Type set to Text

---

### ❗ Website returns empty content

Some sites block scraping. Try:

* news sites
* blogs
* documentation pages

---

## 🚀 Possible Enhancements

Future improvements may include:

* Webhook-based dynamic URL input
* Scheduled monitoring
* Multi-page research
* Google Sheets logging
* Vector database memory
* Slack/Teams notifications
