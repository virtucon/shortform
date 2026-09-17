# Step 1: Understand the source

Read, do not run. No browser, no dev server, no screenshots. The goal is enough real material to make a video that could only be about this subject.

## Project mode

Read in this order and stop when the rubric is answered:

1. `README`, `PRODUCT.md`, `package.json` / `pyproject.toml` / equivalent: name, one-line pitch, who it is for.
2. The landing page or main screen: `index.html`, `app/page.*`, `src/App.*`, `pages/index.*`. Take the real headline, subhead, button labels and feature names.
3. Styles: `:root` custom properties, Tailwind config, theme files. Take the real colours, fonts, corner radius and spacing feel.
4. The core flow: the routes or components for the one thing a user comes to do. Note entry, key action, result.
5. `public/` or `assets/`: logo, icons, product images that can be copied into the composition.

Record exact strings. "Send invoices in 30 seconds" is usable; "fast invoicing" is not.

## Brief-only mode

The brief is the source. Pull out the subject, the claims the user made, any numbers, and any named media. Do not research or add facts the user did not give. If the brief is too thin to fill the length, plan a shorter video rather than padding.

## User media

If the brief names files or a folder (images, screen recordings, clips):

- Confirm each path exists. Report missing ones and continue without them.
- Probe video with `ffprobe` for duration, dimensions and orientation.
- Landscape media in a vertical frame goes in a framed card (device frame or rounded panel) in the middle of the safe zone, never stretched or cropped to fill.
- Media is a scene ingredient. The hook and captions still come from the plan.

## Rubric

You are ready to plan when you can answer all of these in one line each:

1. What is it, in the subject's own words?
2. Who is the viewer, and what do they care about right now?
3. What is the objective, and which single action or feeling proves it worked?
4. What is the most surprising, specific or visual true thing about the subject?
5. What does the viewer see of the real thing (which screen, flow, number or image)?
6. What are the brand colours, fonts and visual personality?
7. What tone fits the subject and the audience?
8. Which exact phrases from the source belong on screen?
9. What must not be claimed because the source does not support it?
