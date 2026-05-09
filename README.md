# email-to-flourish

Convert email thread PDFs into HTML formatted for the [Flourish Text Annotator](https://flourish.studio) template — either via a **web app** (no install needed) or a **Claude Code skill** for terminal users.

Journalists who want to recreate and publish email threads using Flourish's Text Annotator currently have to format the HTML by hand, a time-consuming, error-prone process requiring knowledge of inline CSS, redaction bar markup, and Flourish's rendering quirks. This AI-assisted workflow (via Claude Code) removes that barrier entirely.

---

## Web app

A browser-based mockup is live at:

**[pilartms.github.io/ai-email-to-flourish](https://pilartms.github.io/ai-email-to-flourish/)**

Upload a PDF, click Convert, and copy or download the Flourish-ready HTML — no CLI, no account required.

The source is in `docs/index.html`.

---

## Claude Code skill — `/email-to-flourish`

For users working in the terminal with Claude Code.

### What it does

1. Reads the PDF from an `input/` folder in your project
2. Converts the email thread into correctly structured HTML, applying every rule in `CONVENTIONS.md`: redaction bars, signature blocks, nested quoted emails, transition lines, and confidentiality footer removal
3. Saves the result to `output/` using the same base filename as the PDF

### Installation

```bash
claude plugin install /path/to/email-to-flourish-plugin
```

### Usage

Drop a PDF into your project's `input/` folder, then run:

```
/email-to-flourish
```

Or specify a filename directly:

```
/email-to-flourish EFTA01234567.pdf
```

On first use, the skill creates `output/` if it doesn't exist.

### Formatting conventions

The skill applies a detailed set of rules defined in `CONVENTIONS.md`. Key ones:

- **Line breaks**: Flourish treats `<p>` as inline — use `<br>` for consecutive lines and `<br><br>` for blank lines between paragraphs
- **Signature blocks**: separated by `<br>`, with `<br><br>` before the first signature line
- **Redaction bars**: inline `<span>` with black background; wrapped in `< >` on inner `From:` fields and transition lines, name-only on outer `From:`
- **Transition lines**: `--- On [date], [SENDER] <[bar]> wrote:` reproduced as plain text (no bold), followed by `<br><br>`
- **Dividers**: horizontal rule only if visibly present in the PDF
- **Footer removal**: Epstein confidentiality disclaimer always stripped
- **No bold in body**: `font-weight:bold` reserved for header field labels only

A 10-point checklist at the end of `CONVENTIONS.md` (and embedded in the plugin's `SKILL.md`) must pass before saving.

### Output format

The generated HTML uses inline styles throughout — no external stylesheet needed. Paste it directly into Flourish's Text Annotator HTML field.

→ [View a live Flourish graphic built with this tool](https://public.flourish.studio/visualisation/28829016/)

---

## Repository layout

```
input/                        PDFs to convert (not committed)
output/                       Generated HTML files (not committed)
docs/index.html               Web mockup (served via GitHub Pages)
templates/                    Reference HTML templates (template_1, _2, _3)
CONVENTIONS.md                Full formatting rules and checklist
email-to-flourish-plugin/     Installable Claude Code plugin
.claude/skills/               Local skill definition (references CONVENTIONS.md)
```

---

## Origin

Created by Pilar Tomás as part of the **Advanced Prompt Engineering for Journalists** MOOC, taught by Joe Amditis (Center for Cooperative Media).
