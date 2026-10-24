# Pro Motion

Audio-first motion-graphics launch, brand and TVC films with Claude — planned around the music, rendered in code (canvas → MP4), cut to the beat.

Ask for a launch video, brand film or promo with (or without) a track. The skill plans the music as a marketing asset (sonic logo, hooks, Suno/Udio prompt, post-production), analyses the beat, pulls reference contact sheets, shows three style frames, writes a protagonist-driven shot map, builds the film with a motion library, renders it in parallel and checks a frame sheet.

## Install

In a Claude Code session:

```
/plugin install pro-motion --marketplace jack-reson8/pro-motion
```

Or from the shell:

```
claude plugin marketplace add jack-reson8/pro-motion
claude plugin install pro-motion@pro-motion
```

Requirements: Node 18+, Python 3 with numpy/scipy/matplotlib, Chrome or Chromium. Rendering and scripts run in Claude Code; on claude.ai the skill still guides planning, music prompts and shot maps.

Full docs, showcase films and source: https://github.com/jack-reson8/pro-motion
