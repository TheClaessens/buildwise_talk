# Under the hood: Claude at Buildwise

Standalone export of the talk (7 November 2026). Works offline, and is ready to publish as a GitHub Pages site as-is.

- `index.html` the deck. Keys: arrows / space / click to advance, N speaker notes, F fullscreen, R replay the animation, L cycle language. The EN / NL / FR buttons are top right. `index.html?lang=fr#workflow` opens a slide in a language.
- `handout.html` the handout (English). `handout.nl.html` and `handout.fr.html` are machine-translated and unreviewed.
- `linked/` the three documents the slides link to, bundled offline.
- `assets/` the three gameplay videos. Everything else (fonts, images) is embedded.

To publish on GitHub Pages: put these files in a repository, then Settings → Pages → deploy from branch, root folder. The site is served at `https://<owner>.github.io/<repo>/`.

Dutch and French slide text is machine translated. Speaker notes and the labels inside the animations stay English.
