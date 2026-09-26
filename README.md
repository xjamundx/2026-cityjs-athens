# Agentic Image Generation — Slidev

The talk is in `slides.md`. Every slide is separated by `---`.

The original notes are preserved without rewriting in
`source-notes-verbatim.txt`. Keep that file as the source of truth while the
slide outline evolves.

## Preview

Install Node.js, then run:

```sh
npm install
npm run dev
```

Slidev opens a live browser preview. Edit `slides.md` and save to update it.

## Images

Create a `public/images` directory and put images there. Reference them from
the slides like this:

```markdown
![Description](/images/example.png)
```

The current image references are placeholders and intentionally point to
filenames that do not exist yet.

## Export

```sh
npm run export
```

Slidev can export to PDF, PNG, and PPTX. Interactive features may not remain
editable when converted to another format.
