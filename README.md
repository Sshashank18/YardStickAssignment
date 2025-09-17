# 🚀 Groq Conversation Manager & Classifier

This repository demonstrates how to build a **conversation manager** and a **message classifier** using the [Groq API](https://groq.com). It showcases two main tasks:

1. **Conversation Management** with history summarization and truncation.
2. **Message Classification** with structured outputs (category, sentiment, entities).

---

## 📂 Features

### ✅ Task 1: Conversation Manager
- Maintains conversation history with a user.
- Supports **periodic summarization** after every *k* interactions.
- Provides **truncated history** based on:
  - `max_turns` → Keeps last N turns (user + assistant pairs).
  - `max_chars` → Keeps as many latest messages as fit in the limit.
- Summaries replace older history for efficiency, preserving context.

### ✅ Task 2: Classifier Tool
- Classifies user messages into structured JSON format.
- Outputs:
  - **Category**: (`support_request`, `sales_inquiry`, `general_query`, `feedback`, `other`)
  - **Sentiment**: (`positive`, `neutral`, `negative`)
  - **Entities**: Extracts product names, emails, and order IDs when present.
- Uses **Groq tool calling** with function schema for deterministic results.

---

## 🛠️ Installation

```bash
pip install groq
```

---

## 🔑 Setup

1. Get a **Groq API key** from your Groq dashboard.
2. Set the API key in your script:

```python
GROQ_API_KEY = "your_api_key_here"
client = Groq(api_key=GROQ_API_KEY)
```

---

## 📖 Usage

### ▶️ Task 1: Conversation Manager

```python
manager = ConversationManager(client, model="llama-3.1-8b-instant", summarize_every_k=3)
response = manager.chat("Tell me about Mars.")
print(response)
```

**Demo Output (Summarization after 3rd run):**
```
--- [Attempting Summarization at Run 3] ---
Condition met (k=3). Summarizing history...
New Summary: [Concise conversation summary here]
```

Truncation example:
```python
truncated = manager.get_truncated_history(max_turns=1)
```

---

### ▶️ Task 2: Classifier Tool

```python
classification = classify_message("Hi, my X-1000 rocket boots are broken. Order #G-123")
print(json.dumps(classification, indent=2))
```

**Sample Output:**
```json
{
  "category": "support_request",
  "sentiment": "negative",
  "entities": {
    "order_id": "G-123",
    "user_email": "buzz@light.year"
  }
}
```

---

## 📊 Example Results

### Conversation Summarization
- After 3 interactions, history is replaced by a **summary** while retaining context.
- Example summary:
  - Mars exploration facts
  - Travel time to Mars
  - Jupiter’s Great Red Spot

### Classification Examples
- Support request with order details.
- Sales inquiry about products.
- General queries (weather, UI feedback).
- Sentiment detection (positive, neutral, negative).

---

## 📌 Notes
- Summarization ensures scalability for long chats.
- Truncation allows context-limited API calls.
- Classification uses **Groq Tool Calling** for reliable structured outputs.

---
