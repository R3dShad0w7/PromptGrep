# PromptGrep

# 📜 README: Semgrep Rules for LLM Prompt Injection Detection

## Objective

These Semgrep rules aim to **detect potential prompt injection vulnerabilities** in Python projects that use Large Language Models (LLMs).

They focus on finding **places where untrusted user input (like web request parameters) is directly or indirectly passed into LLM prompts or generation calls**.

Prompt injection is a critical security issue in LLM-based apps. If an attacker can influence the prompt without proper sanitization, they can subvert or exfiltrate data from the model.

---

## Rule Types

This repo includes:

✅ **Heuristic Rules (pattern-based):**  
Detect common ways of building prompts unsafely using string interpolation, concatenation, `.format`, JSON templates, etc.

✅ **Sink-based Rules:**  
Flag calls to LLM methods like `predict`, `invoke`, `generate`, `ask`, etc. using potentially tainted data.

✅ **Taint Analysis Rules:**  
Track data flow from user-controlled sources (like `request.get_json()`) to LLM calls.

---

## ⚙Setup

1️⃣ **Install Semgrep**

If you haven’t already:

```bash
pip install semgrep
