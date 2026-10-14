# The Vault of Six Doors

A six-puzzle cipher challenge that runs entirely in the browser. It is a static site, so it works on GitHub Pages with no server.

| Door | Cipher | What the student does |
|------|--------|-----------------------|
| I | Caesar | **Decrypts by hand.** The key is the position in the alphabet of a riddle's answer. |
| II | Vigenère | **Decrypts by hand** using the Vigenère table on the page. The key is the answer to a trick question. |
| III | Caesar | **Encrypts by hand.** A message and shift are given; the ciphertext is the password to Door IV. |
| IV | Vigenère | **Encrypts by hand** using the Vigenère table. A message and key are given; the ciphertext is the password to Door V. |
| V | AES-256-CBC (symmetric) | The key is on a page reached through a hidden link, found with a clue. A decoder is provided. |
| VI | RSA-4096 (asymmetric) | Unsolvable by design. |

Each door opens only for the password earned from the previous one. The locked doors are genuinely encrypted inside `vault.js` (PBKDF2 then AES-GCM), so viewing the page source does not skip a player ahead. Passwords are matched ignoring capitals, spaces and punctuation.

There are no automatic decoders for the Caesar and Vigenère doors, and no auto-encrypters for the two encryption doors. Students work them out themselves. The only aid is the highlightable Vigenère table on Doors II and IV.

## Publish on GitHub Pages

1. Create a new GitHub repository and push this folder to it. The included `.gitignore` keeps `author/` (the answers) out of the repository.
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, pick your branch (for example `main`) and the **`/docs`** folder, then save.
4. After a minute or so the site is live at `https://<your-username>.github.io/<repo-name>/`.

## Folder layout

- `docs/` is the website. This is the only folder that gets published.
  - `index.html` is the main page and all the game logic.
  - `vault.js` is the generated, encrypted puzzle data.
  - `whisper.html` holds the Door V key (reached through the hidden link).
  - `style.css` is the shared styling.
- `author/` is private.
  - `ANSWERS.md` has every answer, including the correct ciphertexts for Doors III and IV.
  - `build.mjs` regenerates `docs/vault.js`.

## Changing the puzzles

Edit the secrets at the top of `author/build.mjs` (the Door III and IV messages, shift and key are there), then run `node author/build.mjs`. The script checks its own worked examples and round-trips before writing anything. If you change the Door V key, update `docs/whisper.html` to match. Re-running generates a fresh Door VI key pair and fresh ciphertexts, so avoid re-running after you have shared the link.

## Honest limits

This is a client-side game. Anyone willing to read the repository and brute-force it can get through the early doors, and nothing can stop that. Don't publish `author/`. Door VI cannot be opened by anyone, because its private key was never stored.
