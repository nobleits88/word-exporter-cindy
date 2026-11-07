# InoCore Word/PDF Writer

Cindy Plugin for preserving Markdown files and converting Markdown documents into professionally formatted Word and PDF outputs.

- GitHub: <https://github.com/nobleits88/word-exporter-cindy>
- Minimum Cindy version: `0.1.122-beta`
- Current Plugin version: `1.0.1`

## Features of the Plugin

### Markdown export

- Export the original Markdown source as a UTF-8 `.md` file.
- Preserve the original Markdown syntax, content and line breaks.
- Use `export_md` for Markdown-only output.
- Optionally request companion Word or PDF output from the same command.

### Word export

- Convert Markdown to an editable Microsoft Word `.docx` file.
- Generate a valid native Word 2007+ document package.
- Support:
  - Document titles
  - Heading hierarchy
  - Paragraphs
  - Bold and italic text
  - Inline code and code blocks
  - Bulleted lists
  - Numbered lists
  - Block quotes
  - Markdown tables
  - Traditional Chinese text
  - A4 and Letter paper
  - Portrait and landscape orientation
  - Optional page-number fields
  - Author metadata
- Use single-line paragraph spacing and 6 pt paragraph-after spacing.
- Use native Word styles, numbering and table structures rather than flattened text.

### Professional PDF workflow

- Prepare Markdown as sanitized, self-contained HTML for PDF rendering.
- Use Cindy host Chromium to render the HTML into a PDF.
- Support professional formatting for agreements, contracts, reports and manuals.
- Support:
  - Legal-document title formatting
  - Party sections
  - Article headings
  - Repayment schedule tables
  - Total rows
  - Signature pages
  - Signature blocks
  - Legal-warning callouts
  - CJK font fallback
  - Page-break control
  - Table header repetition across pages
- The Plugin does not use the old handwritten PDF serializer.
- No Office, LibreOffice, cloud conversion service or external runtime package is required.

### Security and reliability

- Uses a local dependency-free Node JSON-RPC worker.
- Does not require credentials, API keys or network access.
- Rejects unsafe output filenames and path traversal.
- Escapes HTML/XML output.
- Does not execute Markdown as code.
- Provides file size and SHA-256 metadata for generated files.

## How to Use the Plugin

### Export Markdown only

Use the `export_md` command:

```json
{
  "markdown": "# My Document\n\nThis is the source content.",
  "formats": ["md"],
  "outputName": "my-document"
}
```

The result is:

```text
my-document.md
```

### Export Markdown and Word

```json
{
  "markdown": "# My Document\n\nThis is the source content.",
  "formats": ["md", "docx"],
  "outputName": "my-document"
}
```

The result contains:

```text
my-document.md
my-document.docx
```

### Export Markdown and PDF preparation output

```json
{
  "markdown": "# My Document\n\nThis is the source content.",
  "formats": ["md", "pdf"],
  "outputName": "my-document"
}
```

The Plugin returns:

```text
my-document.md
my-document.html
```

The `.html` file is the sanitized PDF preparation artifact. It is not itself a PDF. The host or Agent must then:

1. Call Cindy `render_pdf` with the generated HTML.
2. Call Cindy `inspect_pdf` on the resulting PDF.
3. Deliver the PDF only after the inspection succeeds.

### Export Word only

Use the `export_word` command:

```json
{
  "markdown": "# My Document\n\nThis is the source content.",
  "outputName": "my-document"
}
```

### Export PDF only

Use the `export_pdf` command:

```json
{
  "markdown": "# My Document\n\nThis is the source content.",
  "title": "My Document",
  "paper": "A4",
  "orientation": "portrait",
  "outputName": "my-document"
}
```

### Markdown file input

A Markdown file can be attached through Cindy's host-level file handoff. The Plugin receives it through `cindy_handoff_files`; do not put a local filesystem path inside the Markdown text or tool parameters.

### Chinese trigger phrases

The Plugin can be used when the user says phrases such as:

- `頛詨 Word`
- `åå Word`
- `鋆賭½ Word`
- `頛詨 PDF`
- `åå PDF`
- `鋆賭½ PDF`
- `頛詨 MD`
- `åå Markdown`

## Installation of the Plugin

### Install from a `.cindy` package

1. Download or build the Plugin package:

   ```text
   inocore-word-pdf-writer-1.0.1.cindy
   ```

2. Open Cindy's Plugin management interface.
3. Choose the option to install a local Plugin package.
4. Select the `.cindy` file.
5. Confirm the Plugin identity:
   - Name: `InoCore Word/PDF Writer`
   - ID: `inocore-word-pdf-writer`
   - Version: `1.0.1`
6. Enable the Plugin after installation.

### Build and install from source

The source repository is:

<https://github.com/nobleits88/word-exporter-cindy>

The project is dependency-free for the Plugin runtime. From the project directory, validate the source and pack it with Cindy Plugin Forge:

```text
ghost_forge_pack
```

Then install or update the generated `.cindy` package through Cindy Plugin Forge:

```text
ghost_forge_install
```

The source directory contains:

- `ghost.json` â Plugin manifest and tool schemas
- `main.js` â Cindy host entry point
- `node/worker.cjs` â local Markdown conversion worker
- `tests/` â worker protocol tests
- `RELEASE_NOTES.md` â version history

### Update an existing installation

When a new version is released:

1. Download the newer `.cindy` package.
2. Install it through Cindy Plugin management or Plugin Forge.
3. Select the existing `inocore-word-pdf-writer` installation when prompted.
4. Confirm the update.
5. Re-enable the Plugin if the host requires it.

Existing Plugin configuration and enabled state should be preserved by an in-place update.

## Development and Testing

This project uses SVN for source control.

```text
svn status
svn diff
svn commit -m "Release InoCore Word/PDF Writer v1.0.1 - add Markdown export"
```

Run the local checks from the project directory:

```text
node --check main.js
node --check node/worker.cjs
node tests/worker-protocol.test.cjs
```

Before formal or legal use, review all generated files. Legal agreements should be reviewed by a qualified lawyer before signing or publication.

## GitHub

Project repository:

<https://github.com/nobleits88/word-exporter-cindy>
