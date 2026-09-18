A showcase repo — no source code, just the article and the screens.

**The README** runs as a proper write-up: why it was built (the spreadsheet problem), a walk through each screen with the screenshot above it, the feature list, a security section, how it's built, the versioning discipline, the testing, and the status. It ends by saying plainly that the repository is a write-up and the source isn't published — so nobody opens an issue asking for the code.

**Seven screenshots**, all generated fresh for this: sign-in, dashboard in dark and light, an enquiry opened, team messages, and the phone in both themes.

Two deliberate choices you should know about:

1. **Every name in the screenshots is invented** — Sunrise Textiles, Meridian Foods, Arclight Pharma, and staff called "R. Kamat", "Sales desk". No real customer, no colleague's name, no real enquiry. Only your own name appears, as the signed-in user.
2. **The security section describes outcomes, not internals.** No property names, no function names, no sheet names, no login IDs. It reads as strong engineering without handing anyone a map of a live system.

**To publish it:**

1. On GitHub, **New repository** → name it `enquiry-tracker` → **Public** → don't tick "Add a README".
2. On the empty repo page, click **uploading an existing file**.
3. Drag in `README.md`, then create the folder path by dragging the images and renaming the upload path to `docs/img/` — or simpler: drag the whole `docs` folder from your Downloads if you keep the structure `docs/img/*.png`.
4. Commit. The README renders on the front page with the screenshots in place.

Keep the folder structure exactly as delivered — `README.md` at the top, images in `docs/img/` — or the image links won't resolve.

If you'd like a short one-paragraph repo description and topic tags for the sidebar, or a LinkedIn version of this article, say the word.
