# email-to-flourish

A Claude Code plugin that converts email thread PDFs into HTML formatted for the [Flourish Text Annotator](https://flourish.studio) template.

## What it does

Run `/email-to-flourish` in any Claude Code session and it will:

1. Read the PDF from an `input/` folder in your project
2. Convert the email thread into correctly structured HTML, handling redaction bars, signature blocks, nested quoted emails, and transition lines
3. Save the result to `output/` using the same filename as the PDF

## Installation

```bash
claude plugin install /path/to/email-to-flourish-plugin
```

## Usage

Drop a PDF into your project's `input/` folder, then run:

```
/email-to-flourish
```

Or specify a filename directly:

```
/email-to-flourish EFTA01234567.pdf
```

On first use, the skill will create the `input/` and `output/` directories if they don't exist.

## Output format

The generated HTML is ready to paste into the Flourish Text Annotator's HTML field. It uses inline styles throughout so no external stylesheet is needed.

## Origin

This plugin was created as part of the **Advanced Prompt Engineering for Journalists** MOOC, taught by Joe Amditis (Center for Cooperative Media).
