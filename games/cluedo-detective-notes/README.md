# Cluedo — Detective Notes

Open-source replacement for the paper detective sheet in Cluedo/Clue: a suspects / weapons / rooms ×
players grid where you mark what's ruled out or confirmed, so you don't waste paper. It reproduces the
functionality of the well-made reference app **"Get a Clue"** and adds the one thing it lacks — a free
**notepad**.

**Status:** working MVP — a single self-contained `index.html`, no dependencies, runs offline. Open it
in any phone or desktop browser. See `STATO.md` (Italian) for the current state and open questions.

**Features:** suspects / weapons / rooms × players grid with per-cell markers (has ✓ / doesn't have ✗
/ maybe ?), optional auto-deduction (ruling in one owner rules it out for the rest; a card ruled out
for everyone is flagged as the solution), player management, a "my cards" panel, editable card sets,
English/Italian UI, and a free-text notepad. Everything is saved locally on the device.

**Clean-room:** this project reproduces *functionality* only. It does not copy the reference app's code,
graphics, assets, or trademarks; "Get a Clue", "Clue" and "Cluedo" are trademarks, so the public name
will be a neutral one.
