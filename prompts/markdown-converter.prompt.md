---
name: markdown-converter
description: "Use when: you want to convert selected plain text, notes, or snippets into clean, well-structured Markdown. Handles headings, lists, code blocks, inline formatting, tables, and examples."
author: Copilot
---

# Markdown Converter — Prompt

Purpose
- Convert arbitrary selected text into clean Markdown suitable for documentation, README, or notes.

Inputs
- `selection` — the text the user has selected in the editor (required).
- `preserveCodeBlocks` — boolean (optional, default: true). When true, detect and format code blocks.
- `headingStyle` — string (optional, default: "atx"). Options: `atx` (# Heading) or `setext` (Heading underline).

Output
- A Markdown-formatted string replacing (or returned for) the selected text.

Instructions (for the agent)
1. Parse the `selection` and detect structural elements: headings, numbered and bulleted lists, checklists, tables, code excerpts, inline code, bold/italic hints, links, and examples.
2. Emit Markdown that is idiomatic and human-readable:
   - Use `#`..`######` for headings (or `setext` if `headingStyle: setext`).
   - Convert bullet-like lines (leading `-`, `*`, or `•`) into `-` list items.
   - Convert numbered lines into `1.`, `2.` sequences preserving numbering.
   - Detect code blocks: group contiguous indented or fenced code and wrap with triple backticks, include language if detectable.
   - Convert key:value lines into definition-like lists or tables when appropriate.
   - Preserve blank lines between paragraphs and sections.
3. Normalize whitespace, trim trailing spaces, and wrap long paragraphs at ~80-100 characters where it improves readability.
4. If `preserveCodeBlocks` is false, inline small code tokens with single backticks instead of fenced blocks.
5. Return only the Markdown output — do not add commentary or extra instructions.

Examples

Input (selection):
```
Title: My API Guide
- First item
- Second item
Code:
    function sum(a,b){return a+b}
```

Output:

```
# My API Guide

- First item
- Second item

```javascript
function sum(a, b) {
  return a + b;
}
```
```

Usage notes
- Prefer `atx` headings by default.
- When converting tables, use pipe-delimited Markdown tables if columns are consistent.
- Keep the output minimal and focused on Markdown — the user will edit further if needed.

Quality constraints
- Do not invent facts or fill in missing technical details.
- Do not produce HTML inline; output should be pure Markdown.

Example invocation (natural language)
- "Convert selection to Markdown, preserve code blocks, use atx headings."


---

# End of Prompt
