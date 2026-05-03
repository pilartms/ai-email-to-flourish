---
name: email-to-flourish
description: Convert an email thread PDF from input/ into Flourish Text Annotator HTML and save it to output/
argument-hint: [filename.pdf]
---

## Task

Convert the email thread PDF into HTML formatted for the Flourish Text Annotator template.

### Step 1 — Identify the input file

If `input/` or `output/` directories do not exist, create them before proceeding.

Files currently in `input/`:

!`ls input/`

If the user specified a filename as an argument, use that. Otherwise use the file listed above. Read the PDF from `input/`.

### Step 2 — Generate the HTML

Apply every rule in the Conventions section below. Run through the Checklist before saving.

---

## Conventions

### Template selection

Choose the outer wrapper based on the outer email's header field order:

**Template 1** — outer header is To / From / Sent / Subject:
```html
<div style="
  background:#ffffff;
  width:calc(100% + 32px);
  margin:0 -16px;
  padding:0;
  box-sizing:border-box;
">
```

**Template 2** — outer header is From / Date / To / Subject (or From-first variants):
```html
<div style="max-width:100%; width:100%; overflow:visible; box-sizing:border-box; margin:0; padding:0;">
```

### Header field structure

Every field row uses this flex layout:

```html
<div style="display:flex; margin:0; padding:0; line-height:1.1; align-items:flex-start;">
  <div style="font-weight:bold; width:78px; margin:0; padding:0;">From:</div>
  <div style="
    margin:0; padding:0;
    flex:1 1 auto; min-width:0; max-width:100%;
    overflow-wrap:anywhere; word-break:break-word;
  ">
    value here
  </div>
</div>
```

Add `white-space:normal;` to the value div on the first field of the outer header (To or From).

When a field value contains a redaction bar alongside text, add `display:flex; flex-wrap:wrap; align-items:center;` to the value div.

### Line breaks

Flourish treats `<p>` as **inline** — margins on `<p>` produce no visual spacing.

- **Line break** (consecutive lines, no blank line in PDF):
  ```html
  <p style="margin:0 0 12px 0;">Line one.<br>Line two.</p>
  ```
- **Blank line** (empty line visible between paragraphs in PDF):
  ```html
  <p style="margin:0 0 12px 0;">Paragraph one.</p><br><br><p style="margin:0 0 12px 0;">Paragraph two.</p>
  ```
- **Before a signature block**: always `<br><br>` before the first signature line.

### Signature blocks

```html
<br><br>
Name here<br>
Title here<br>
<span style="background:#000; color:#000; padding:2px 6px; display:inline-block; line-height:1; height:1em; box-sizing:border-box;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><br>
Address here
```

Separate each line with `<br>`, not separate `<p>` tags. No `<strong>` on the name.

### Redaction bars

```html
<span style="background:#000; color:#000; padding:2px 6px; display:inline-block; line-height:1; height:1em; box-sizing:border-box;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>
```

**When to wrap in `< >`:**

| Context | Format | Example |
|---------|--------|---------|
| Outer `From:` — name only, no bar in PDF | Name only | `PETER MANDELSON` |
| Outer `From:` — name + bar visible in PDF | Name + `< bar >` | `Peter Mandelson <[bar]>` |
| Inner quoted `From:` | Name + `< bar >` | `PETER MANDELSON <[bar]>` |
| `--- On ... wrote:` transition line | Name + `< bar >` | `PETER MANDELSON <[bar]> wrote:` |
| Entire `From:` value redacted | Bar only, no `< >` | `[bar]` |

When a bar sits inside a `display:flex` container alongside text, use `<span>` children for the `< >` so they flex correctly:

```html
<span>NAME &lt;</span>
<span style="background:#000; color:#000; padding:2px 6px; display:inline-block; line-height:1; height:1em; box-sizing:border-box;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>
<span>&gt;</span>
```

### Divider between emails

Only use a CSS horizontal rule if a visible horizontal line is present in the PDF:

```html
<div style="border-top: 2px solid #cfcfcf; margin: 2px 0 12px 0;"></div>
```

### Transition line

Reproduce `--- On [date], [SENDER] <[email]> wrote:` lines as plain text — **no bold** on date or sender:

```html
<p style="margin:0 0 12px 0;">--- On [date], [SENDER] &lt;<span style="background:#000; color:#000; padding:2px 6px; display:inline-block; line-height:1; height:1em; box-sizing:border-box;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&gt; wrote:</p>
```

Place it after the outer email's body and before the inner email's header block.

### Confidentiality footer

Always **remove** Epstein's standard disclaimer: the `--` separator, the `*****...` asterisk row, and all text from "The information contained in this communication is confidential…" through "copyright -all rights reserved".

### No bold in body

Do not use `<strong>` anywhere in body text, transition lines, or signatures. Bold applies only to header field labels via `font-weight:bold` in the flex CSS.

---

### Step 3 — Save the output

Save to `output/` using the same base filename as the PDF:
`input/EFTA01833959.pdf` → `output/EFTA01833959.html`

---

## Checklist before saving

- [ ] Outer wrapper matches header field order (template_1 vs template_2)
- [ ] All header fields use the flex layout pattern
- [ ] Line break vs blank line matches the PDF (`<br>` inside `<p>` vs `<br><br>` between `<p>` tags)
- [ ] `<br><br>` before each signature block; signature lines use `<br>` not `<p>`
- [ ] Outer `From:` name only when no bar; name + `< bar >` when bar visible in PDF
- [ ] Inner `From:` wraps bar in `< >`
- [ ] Transition line present if in PDF; no bold on date/sender
- [ ] Horizontal divider only if present in PDF
- [ ] Confidentiality footer removed
- [ ] No `<strong>` in body, transitions, or signatures
