# PromptGrep

# Semgrep Rules for LLM Prompt Injection Detection

## Objective

These Semgrep rules aim to **detect potential prompt injection vulnerabilities** in Python projects that use Large Language Models (LLMs).

They focus on finding **places where untrusted user input (like web request parameters) is directly or indirectly used to build prompts or passed to LLM generation calls**.

Prompt injection is a critical security issue in LLM-based apps. If an attacker can influence the prompt without proper sanitization, they can subvert the model, exfiltrate data, or produce malicious responses.

---

## Rule Types

This repo currently includes:

✅ **Heuristic Rules (pattern-based):**  
- Detect common ways of building prompts unsafely:
  - String interpolation
  - Concatenation
  - `.format()`
  - f-strings
  - Templates without validation

✅ **Sink Detection Rules:**  
- Flag direct calls to LLM methods such as:
  - `predict`
  - `invoke`
  - `generate`
  - `ask`
  - `chat`
  - App-specific wrappers like `generate_response`, `query_llm`, etc.

These rules work purely by **pattern matching**, without data-flow (taint) tracking. They are simple, explainable, and easy to customize.

---

## Rule Files

**✔️ [`rules/prompt-injection-pattern.yaml`](rules/prompt-injection-pattern.yaml)**

- Heuristic patterns for unsafe prompt construction.
- Sink patterns for LLM callsites that can receive user-controlled data.
- Easy to customize for your app’s function names.

Example rule snippet:

pattern-either:
  - pattern: f"...{...}..."
  - pattern: $PROMPT.format(...)
  - pattern: $LLM_FUNCTION($PROMPT)

⚙️ Setup

Install Semgrep

If you haven’t already:

```
pip install semgrep
```
✅ How to Run

Scan your project with:

```
semgrep --config PATH_TO_RULES_DIRECTORY .
```

✅ Customizing Rules

If your app uses custom LLM wrappers (like my_llm_call() or call_gpt()), you can extend the sink list in the YAML:

pattern-either:
  - pattern: my_llm_call($PROMPT)
  - pattern: call_gpt($PROMPT)

This lets you adapt detection to your specific codebase with minimal effort.
✅ Recommended Practices

    Review Findings: These rules may yield false positives. Always review flagged code to confirm vulnerabilities.

    Sanitize User Input: Validate, filter, or constrain user input before including it in prompts.

    Use Structured Templates: Prefer parameterized, safe templating over string concatenation or interpolation.

📜 Notes

    This repo only includes pattern-based rules. Taint-analysis rules (tracking user input to sinks) are excluded for simplicity and to reduce false positives.

    Semgrep’s pattern matching is fast and explainable, making it suitable for CI pipelines, developer education, and lightweight security scans.

📣 Contributions

Feel free to open issues or PRs to:

    Add new patterns for common LLM libraries

    Improve detection coverage

    Reduce false positives
