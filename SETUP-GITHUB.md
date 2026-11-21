# Move Orbit to GitHub Pages (iPhone steps)

**First, on your old Netlify Orbit app:** tap Export & backup → Full JSON backup. Keep that file in Files. It contains your personal tasks and must **not** be uploaded to GitHub.

1. Sign up for free at https://github.com/signup
2. Create a **public** repository called `orbit-todo` (GitHub Pages on a free account requires a public repo).
3. Unzip this folder and upload these five files to the **root** of the repository: `index.html`, `sw.js`, `manifest.webmanifest`, `icon-192.png`, `icon-512.png`. On GitHub, use **Add file → Upload files → Commit changes**. (You may also upload the markdown instructions, but **not personal JSON backups**.)
4. Open your repository **Settings → Pages**. Under **Build and deployment**, select **Deploy from a branch**, branch **main**, folder **/(root)**, then Save.
5. Your site will appear at `https://YOUR-USERNAME.github.io/orbit-todo/` (it might take a few minutes). Open that in **Safari**.
6. In Orbit: **Restore JSON backup** and pick the file downloaded from your old Netlify site. Verify your task list.
7. In Safari tap **Share → Add to Home Screen → Add**. Launch Orbit via its new icon *while online*, then test airplane mode and task persistence.
8. Only remove the Netlify app once you have verified the restored tasks and made a new JSON backup on GitHub Pages.

**Note:** GitHub Pages hosts app code for free (within usage limits), not a database. Existing tasks remain locally on the same iPhone installation; no automatic cloud sync or closed-app reminders. App code in the public repository is publicly visible. Changes to the source require uploading/committing updated files.
