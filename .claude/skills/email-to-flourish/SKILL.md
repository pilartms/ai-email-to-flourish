---
name: email-to-flourish
description: Convert an email thread PDF from input/ into Flourish Text Annotator HTML and save it to output/
argument-hint: [filename.pdf]
---

## Task

Convert the email thread PDF into HTML formatted for the Flourish Text Annotator template.

### Step 1 — Read the rules and references

Read these files before writing any HTML:

- `CONVENTIONS.md` — rendering rules and formatting conventions (required)
- `templates/template_1.html`, `templates/template_2.html`, `templates/template_3.html` — style references

### Step 2 — Identify the input file

Files currently in input/:

!`ls input/`

If the user specified a filename as an argument, use that. Otherwise, use the file listed above.

Read the PDF from `input/`.

### Step 3 — Generate the HTML

Apply every rule in `CONVENTIONS.md`. Run through the checklist at the bottom of that file before saving.

Key rules to never forget:

- Use `<br>` for line breaks (Flourish treats `<p>` as inline)
- `<br><br>` before each signature block
- No horizontal divider between emails if not present in PDF
- Outer `From:` name only (no bar); inner `From:` wraps bar in `< >`
- Include `--- On [date], [SENDER] <[bar]> wrote:` if present in the PDF

### Step 4 — Save the output

Save to `output/` using the same base filename as the PDF (e.g. `input/EFTA01833959.pdf` → `output/EFTA01833959.html`).
