# Contributing

Most valuable contributions, in order:
1. **Lessons** (`references/lessons.md`): a bug or a taste mistake you hit, what fixed it.
2. **Motion primitives / transitions** in `assets/template/motion-lib.js` — must stay deterministic (`render(t)` pure, no `Math.random` at draw time) and documented in the header index.
3. **Technique sheets** in `references/research-notes.md` — palette · line · texture · shape · depth · camera · transition · timing · signature move. Describe; don't paste copyrighted frames.
4. Script portability (Linux/Windows Chrome paths, Python versions).

Please render the template demo (`node tools/render-fast.js film.html out/demo.mp4 --dur 8`) before opening a PR that touches the library or scripts, and include a contact sheet if the change is visual.
