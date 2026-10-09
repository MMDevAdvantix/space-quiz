# Repository guidance

## Build, test, and lint

- The app is a dependency-free static page. Open `index.html` directly in a browser; no server or build step is required. In PowerShell, use `Invoke-Item .\index.html`.
- There is no package manifest, test runner, or linter configured, so no automated build, test, or lint commands are available.
- For a focused manual smoke test, open `index.html`, use Tab and Enter/Space to select answers and advance, confirm correct and incorrect feedback and score changes, finish the quiz, and use the results-screen replay control to confirm the score resets.

## Architecture

- The app lives in `index.html`: semantic markup, all CSS, the question data, and the quiz controller are kept together so the page works offline without external assets or libraries.
- The `questions` array is the source of quiz content. Each entry contains a prompt, four choices, and a zero-based `answer` index.
- Quiz state (`questionIndex`, `score`, `answered`) is private to the self-invoking script. `renderQuestion` rebuilds the answer buttons; `chooseAnswer` locks the current question, reports feedback, and updates the score; `renderProgress` reflects completion; `showResults` selects the end message and celebration; replay resets state and returns focus to the question.

## Codebase conventions

- Keep changes self-contained in `index.html` unless the app is deliberately being restructured. Avoid adding a framework, dependency, or server requirement to this offline page.
- Keep visual tokens in `:root` and provide their dark-theme counterparts in `@media (prefers-color-scheme: dark)`. Preserve the compact, centered layout and responsive rules.
- Implement controls as native buttons. Keep dynamic question and answer text assigned with `textContent`; maintain visible `:focus-visible` styles, question/result focus management, and live announcements for answer feedback.
- Treat the orbital console artwork and celebration confetti as decorative (`aria-hidden`). Honor `@media (prefers-reduced-motion: reduce)` for nonessential motion.
- When editing quiz behavior, keep progress, score, feedback, disabled-answer state, results, and keyboard replay in sync; verify both correct and incorrect answer paths in a browser.
