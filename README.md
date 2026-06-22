## Introduction

This is the **knee & lower-limb injury-prevention manual** (the STOP-X program from
the German Knee Society) translated into English.

It is built the same way as the expenses sheet: a site generator (`hugo`) turns a
JSON data file into the final pages, using a template. The difference is what comes
out the other end — this time Hugo renders a **website (HTML)**, not a Markdown file.

## FAQ

_Where do the exercises come from?_

From one JSON data file: [`data/exercises.json`](data/exercises.json). Each exercise
is one object in the array, with its title, category, focus, equipment, photo, and
the `goal` / `execution` / `blocks` bullet lists.

_How does HUGO know how to turn that JSON into the manual?_

The template ranges over the data and builds the page — exactly like the expenses
template did:

```
{{ range .Site.Data.exercises }} ... {{ end }}
```

The template lives in [`layouts/index.html`](layouts/index.html), and it groups the
exercises by `category` into the five blocks of the program.

_Where is the intro text (the "Why this matters" part)?_

That part is written by hand in Markdown, in [`content/_index.md`](content/_index.md).
So the manual is part **data** (the exercises, from JSON) and part **content** (the
intro, from Markdown) — Hugo stitches them together.

_Do we edit the final HTML directly? Why?_

No. Hugo generates it automatically into the `public/` folder. You only edit the
JSON, the Markdown, the template, or the styles — never the generated output. The
generated output is the **artefact**.

_Why is there no `.md.md` template like in the expenses sheet?_

Because that trick was for generating a Markdown artefact. Here the artefact is a
normal website, so we use Hugo's normal HTML templates (`.html`) — no `pandoc` step
afterwards. (Remember from the expenses README: a template that generates HTML.)

_What is the schema for?_

[`schemas/exercises.json`](schemas/exercises.json) describes the **shape** every
exercise must have (which fields are required, that `category` must be one of the
five known blocks, that `goal` and `execution` are lists, and so on). VS Code is
wired to it in [`.vscode/settings.json`](.vscode/settings.json), so a mistake in
`data/exercises.json` gets a red underline **as you type it** — before you ever run
`hugo`. Same early bug-catching idea as the expenses schema, just for a slightly
richer shape (lists, and a list of `blocks`).

_How is it published?_

On every push, a **GitHub Action** ([`.github/workflows/hugo.yml`](.github/workflows/hugo.yml))
installs Hugo, builds the site, and deploys it to **GitHub Pages**. So the live
manual updates itself whenever the data changes.

## Credit & safety

The exercises and demonstration photos are from the official **STOP-X** home-training
booklet of the German Knee Society (Deutsche Kniegesellschaft, DKG). This is a
training guide, **not** medical advice — it was written as a knee/ACL prevention
program, so it is a general lower-limb foundation rather than rehab for any one
injury. Always agree the exercises, intensity and timing with a doctor or
physiotherapist before starting, and stop anything that causes sharp pain.
