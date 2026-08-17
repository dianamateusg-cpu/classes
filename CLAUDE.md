# CLAUDE.md

Context for working on this repository.

## What this is

Class materials for Diana Mateus, who teaches Spanish and English. Static
HTML — no framework, no build step, no dependencies. Students open lesson
pages on phones and laptops, often by typing an address read aloud in
class.

Live at **diana-teaches.com**

## Hosting

- **GitHub Pages**, deployed from branch `main`, folder `/` (root)
- Custom domain set in repo Settings → Pages; a `CNAME` file at the repo
  root holds it. Do not hand-edit that file — changing the domain in the
  Pages settings makes GitHub rewrite it and commit directly to `main`,
  so edit it there and `git pull` afterwards.
- `diana-teaches.com` is an **apex domain**, so DNS is four A records to
  the GitHub Pages addresses (185.199.108–111.153), not a CNAME record —
  apex names cannot use CNAME. `www` is a CNAME to
  `dianamateusg-cpu.github.io` and redirects to the apex.
- The site previously lived at `diana-mateus-classes.seanberrie.com`.
  That address now 404s: GitHub Pages serves only one custom domain.
  Squarespace still hosts an unrelated site at seanberrie.com — nothing
  here should touch it.
- `.nojekyll` at the root disables Jekyll. **It must stay.** Without it
  GitHub ignores any folder beginning with an underscore, which would
  delete `_template/` from the live site.

Pushing to `main` publishes. There is no staging environment — a bad
commit is immediately visible to students.

## Structure

Folders are URLs, one to one:

    english/unit1/vocabulary/index.html
      → diana-teaches.com/english/unit1/vocabulary

Current tree:

    /                       language picker
    /english/               unit list
    /english/unit1/         lesson list
    /english/unit1/vocabulary/
    /english/unit1/grammar/
    /english/unit2/         empty placeholder
    /spanish/
    /spanish/ib-unit1-identidades/
    /spanish/ib-unit1-identidades/vocabulario/
    /_template/             copy this to start a lesson
    /404.html
    /styles.css
    /robots.txt

**Every folder gets an `index.html`.** Students navigate by deleting
segments off the end of an address, so no level may 404.

## Naming rules

Folder names: **lowercase, hyphens, ASCII only.** No spaces, no accents,
no capitals. `ib-unit2-migraciones`, never `IB- Unit 2 - Migraciones`.
Spaces become `%20` and accents percent-encode into unreadable URLs that
students cannot type.

Display names are a separate thing and live inside the HTML — the `<h1>`,
`<title>` and `.eyebrow` can carry full capitals, accents and punctuation.
Accented characters are fine there; every page declares UTF-8.

## Adding a lesson

1. Copy `_template/` into the right unit folder, renamed per the rules
   above.
2. Edit `index.html`: `<title>`, `.eyebrow`, `<h1>`, `.standfirst`, then
   the body content.
3. Fix the `.trail` breadcrumb at the top — it is hand-written on every
   page. Use display names matching the headword each level is reached
   by (`Home / English / Unit 1`), not the raw folder names.
4. **Add an `<li>` entry to the parent unit's `index.html`.** Copy an
   existing one and change `href`, headword, `.path` and `.gloss`. A
   lesson that isn't linked is invisible. `.path` is the address
   students type, so it must match `href` exactly — lowercase, and no
   trailing slash.
5. Keep `<meta name="robots" content="noindex, nofollow">` in the head.

Assets go in the lesson's own folder, referenced relatively:

    <img src="map.png" alt="...">
    <audio controls src="dialogue.mp3"></audio>

Video is linked out or embedded from YouTube/Vimeo, never hosted here.

## Styling

One stylesheet, `/styles.css`, linked root-relative from every page.
**Keep the leading slash** — pages sit at varying depths and a relative
path would break them.

Set `<body data-lang="en">` or `data-lang="es"`. This switches the accent
colour so students can see which language section they're in. It is the
only per-language styling; do not add more.

The visual language is a bilingual dictionary entry. Available classes:

| Class | Use |
|---|---|
| `.trail` | Breadcrumb of display names, in mono |
| `.eyebrow` | Small uppercase label above the heading |
| `.standfirst` | Intro sentence under the `<h1>` |
| `.entries` / `.entry` | Link list; each has `.headword`, `.tag`, `.path`, `.gloss` |
| `.lesson` | Wrapper for lesson body content |
| `.vocab` | `<dl>` of term/translation pairs |
| `.note` | Boxed aside — homework, warnings |
| `blockquote` | Model sentences and dialogues |

Fonts are Fraunces, IBM Plex Sans and IBM Plex Mono from Google Fonts,
with system fallbacks.

## Constraints

- **No build tooling.** No npm, bundlers, frameworks, or preprocessors.
  Plain HTML and CSS, editable by a non-developer in VS Code. This is
  deliberate — do not "improve" it by adding a build step.
- **No JavaScript** unless a lesson genuinely needs interactivity, and
  then inline in that page only.
- **The repo is public.** Never commit student names, grades, contact
  details, or anything not intended for strangers.
- Pages carry `noindex` and `robots.txt` disallows crawlers. That's
  obscurity, not access control, and new pages must keep it.
- Don't restructure existing folders without asking — students may have
  bookmarked or written down addresses.

## Local preview

Use the Live Server extension in VS Code, or `python3 -m http.server`.
Opening a file directly via `file://` renders it unstyled, because the
root-relative stylesheet path resolves against the filesystem root. That
is expected and not a bug to fix.

## Before pushing

- Every new folder has an `index.html`
- The parent index links to the new lesson
- The `.trail` uses display names; `.path` matches `href` exactly
- Folder names are lowercase-hyphen-ASCII
- `noindex` meta tag present
- No student personal data
