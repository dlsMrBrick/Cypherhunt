# The Vault of Four Doors

A four-puzzle cipher challenge that runs entirely in the browser. It is a static site, so it works on GitHub Pages with no server.

| Door | Cipher | Key / how you get through |
|------|--------|---------------------------|
| I | Caesar | A riddle whose answer is a letter; its position in the alphabet is the shift |
| II | Vigenère | A trick question whose answer is the key |
| III | AES-256-CBC (symmetric) | The key is on a page reached through a hidden link, found with a clue |
| IV | RSA-4096 (asymmetric) | Unsolvable by design |

Each door opens only for the password hidden in the previous door's decrypted message. The locked doors are genuinely encrypted inside `vault.js` (PBKDF2 then AES-GCM), so viewing the page source does not skip a player ahead.

## Publish on GitHub Pages

1. Create a new GitHub repository and push this folder to it. The included `.gitignore` keeps `author/` (the answers) out of the repository.
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, pick your branch (for example `main`) and the **`/docs`** folder, then save.
4. After a minute or so the site is live at `https://<your-username>.github.io/<repo-name>/`.

## Folder layout

- `docs/` is the website. This is the only folder that gets published.
  - `index.html` is the main page and all the game logic.
  - `vault.js` is the generated, encrypted puzzle data.
  - `whisper.html` holds the Door III key (reached through the hidden link).
  - `style.css` is the shared styling.
- `author/` is private.
  - `ANSWERS.md` has every answer.
  - `build.mjs` regenerates `docs/vault.js`.

## Changing the puzzles

Edit the secrets at the top of `author/build.mjs`, then run `node author/build.mjs`. If you change the Door III key, update `docs/whisper.html` to match. Re-running generates a fresh Door IV key pair and fresh ciphertexts, so avoid re-running after you have shared the link.

## Honest limits

This is a client-side game. Anyone willing to read the repository and brute-force it can get through Doors I to III, and nothing can stop that. Keep the passwords reasonably long (they are two words each) and don't publish `author/`. Door IV cannot be opened by anyone, because its private key was never stored.
