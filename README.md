# diana-teaches.com

Class materials for Spanish and English lessons. Plain HTML — no build
step, no framework, nothing to install.

## Folder structure

The folders ARE the URLs. This:

    english/unit1/vocabulary/index.html

is served at:

    diana-teaches.com/english/unit1/vocabulary

Every folder has an `index.html`, so students can delete the last part of
any address to go back up a level.

## Adding a lesson

1. Copy the `_template` folder.
2. Rename it to the lesson name — lowercase, hyphens instead of spaces,
   no accents. So `pronombres-reflexivos`, not `Pronombres Reflexivos`.
3. Put it inside the right unit folder.
4. Edit the text in `index.html`.
5. Add an entry to the unit's `index.html` so it appears in the list.

Images, audio and any other files go in the same folder as the lesson,
linked with plain relative paths:

    <img src="map.png" alt="Map of the city centre">
    <audio controls src="dialogue.mp3"></audio>

## Adding a unit

Copy an existing unit folder, empty it out, and add a link on the
language's `index.html`.

## Styling

One stylesheet at `/styles.css` covers every page. It's linked with a
leading slash, so it works at any folder depth. Editing it changes every
lesson at once.

Useful classes inside `<div class="lesson">`:

- `<dl class="vocab">` — two-column term/translation list
- `<blockquote>` — model sentences and dialogues
- `<div class="note">` — homework boxes and asides

Set `<body data-lang="es">` on Spanish pages and `data-lang="en"` on
English ones. That switches the accent colour so students can see which
section they're in.

## Publishing

Drag this whole folder onto the Cloudflare Pages upload area. It replaces
the live site with whatever is in the folder, so keep this copy as the
single source of truth — one folder, one machine, always drag this one.

## Privacy

Pages carry `<meta name="robots" content="noindex">` and there's a
`robots.txt` disallowing crawlers. That keeps material out of search
results. It is NOT access control — anyone with the address can open it.
Fine for worksheets; don't put student names or grades here.
