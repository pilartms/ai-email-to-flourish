# Flourish Text Annotator — Email HTML Conventions

Rules for converting email thread PDFs into HTML for the Flourish Text Annotator template.

## Template selection

- Use `template_1.html` when the outer email header uses **To / From / Sent / Subject** field order.
- Use `template_2.html` when the outer email header uses **From / Date / To / Subject** field order.

## Line breaks

Flourish treats `<p>` tags as **inline**, not block. Do not rely on `<p>` to create new lines.

- **Line break** (consecutive lines, no blank line between them): use `<br>` within the same `<p>`:
  `<p style="margin:0 0 12px 0;">Line one.<br>Line two.</p>`
- **Blank line** (visible empty line between paragraphs): use `<br><br>` between the two `<p>` elements:
  `<p style="margin:0 0 12px 0;">Paragraph one.</p><br><br><p style="margin:0 0 12px 0;">Paragraph two.</p>`
- Use `<br><br>` to produce a blank line before a signature block.

## Signature blocks

Each signature line must be separated by `<br>`, not by separate `<p>` tags:

```html
<strong>Lord Mandelson</strong><br />
Chairman, Global Counsel LLP<br />
<span
  style="background:#000; color:#000; padding:2px 6px; display:inline-block; line-height:1; height:1em; box-sizing:border-box;"
>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span
><br />
1 Knightsbridge Green, London, SW1X 7NW
```

Always add `<br><br>` before the first signature line to create a blank line gap after the body text.

## Redaction bars

Redacted content is represented as a black bar:

```html
<span
  style="background:#000; color:#000; padding:2px 6px; display:inline-block; line-height:1; height:1em; box-sizing:border-box;"
>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span>
```

### When to wrap bars in `< >`

Wrap a bar in `&lt;` and `&gt;` **only** when the original PDF shows the email address in angle-bracket format:

| Context                                                        | Format             | Example                          |
| -------------------------------------------------------------- | ------------------ | -------------------------------- |
| Outer email `From:` — name only, no bar visible in PDF         | Name only          | `PETER MANDELSON`                |
| Outer email `From:` — name + visible redaction bar in PDF      | Name + `< bar >`   | `Peter Mandelson <[bar]>`        |
| Inner quoted email `From:`                                     | Name + `< bar >`   | `PETER MANDELSON <[bar]>`        |
| `--- On ... wrote:` transition line                            | Name + `< bar >`   | `PETER MANDELSON <[bar]> wrote:` |
| Entire `From:` value is redacted                               | Bar only, no `< >` | `[bar]`                          |

### Flex containers with bars

When a bar appears alongside text inside a `display:flex` container, keep the `< >` as `<span>` elements so they are treated as flex children:

```html
<span>PETER&nbsp;MANDELSON &lt;</span>
<span style="background:#000; ...">...</span>
<span>&gt;</span>
```

## Divider between emails

Do **not** use a horizontal rule (`border-top`) between the outer and inner emails if the original PDF doesn't have one.

## Transition line between emails

When the PDF includes a `--- On [date], [SENDER] <[email]> wrote:` line, reproduce it as plain text — do **not** bold the date or sender name:

```html
<p style="margin:0 0 12px 0;">
  --- On [date], [SENDER] &lt;<span
    style="background:#000; color:#000; padding:2px 6px; display:inline-block; line-height:1; height:1em; box-sizing:border-box;"
    >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span
  >&gt; wrote:
</p><br><br>
<p style="margin:0 0 12px 0;">&gt; Quoted text here</p>
```

Because Flourish renders `<p>` as inline, adjacent `<p>` tags flow onto the same line without an explicit break between them. Always add a break between consecutive `<p>` elements:

- `<br>` — single line break (no visible gap between lines)
- `<br><br>` — blank-line gap (use whenever the PDF shows an empty line between paragraphs, including between the transition line and the quoted text that follows it)

Place it after the outer email's closing content and before the inner email's header block.

## Confidentiality footer

Always **remove** Epstein's standard confidentiality/legal disclaimer footer when present. This includes the `--` separator line, the `*****...` asterisk row, and all text beginning "The information contained in this communication is confidential…" through "copyright -all rights reserved".

## No bold in body text

Do not use `<strong>` or `font-weight:bold` anywhere in the email body, transition lines, or signature content. Bold is reserved for the header field labels (From:, To:, etc.) which use `font-weight:bold` via the flex container CSS.

## Checklist before saving output

- [ ] Signature lines separated by `<br>`, not `<p>`
- [ ] Blank line (`<br><br>`) before each signature
- [ ] Outer `From:` shows name only (no bar) when name is known
- [ ] Inner `From:` wraps bar in `< >` when PDF shows angle-bracket format
- [ ] `--- On ... wrote:` transition line included if present in PDF (no bold on date/sender)
- [ ] `<br>` or `<br><br>` between every pair of consecutive `<p>` elements (single break for adjacent lines; double break when the PDF shows a blank line between them)
- [ ] No horizontal divider between emails if not present in PDF
- [ ] Epstein confidentiality footer removed
