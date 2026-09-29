# Taskboard

A fast, private to-do app you can install on your phone or desktop. No build step and no dependencies.

**Features:** a dashboard with a personal greeting, quick add that understands "tomorrow at 3pm", notifications, assignments, a week calendar with daily schedule, today's tasks with progress, a 25-minute focus timer and category progress rings. Also a full Tasks view (filters, search, subtasks, repeats), a month Calendar view, priorities and categories, light and dark themes, offline use and install as an app (PWA). With a Firebase backend it adds Google sign-in, live sync across devices and Google Calendar import.

## Project layout
```
public/                 the website (this is what gets deployed)
  index.html            the app
  config.js             your Firebase config goes here
  manifest.webmanifest  app install info
  sw.js                 offline support
  icon.svg, icon-*.png  app icons
firestore.rules         database security rules
firebase.json           Firebase Hosting + Firestore settings
netlify.toml            Netlify settings
vercel.json             Vercel settings
.github/workflows/      GitHub Pages auto-deploy
```

## 1. Try it locally
Run `npx serve public` and open the address it prints. Opening `index.html` directly also works, but offline mode and Google sign-in need a server or a deployed site.

## 2. Put it on GitHub
```
git init && git add . && git commit -m "Taskboard"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/taskboard.git
git push -u origin main
```

## 3. Launch it (pick one)
- **GitHub Pages (free):** in the repo, go to Settings > Pages and set Source to **GitHub Actions**. Every push to `main` deploys `public/` automatically.
- **Firebase Hosting (best fit, since you use Firebase anyway):** `npm i -g firebase-tools`, then `firebase login`, `firebase init` (pick your project and keep the existing files), then `firebase deploy`.
- **Netlify or Vercel:** import the GitHub repo. The included config points both at `public/`. No build command.

For a custom domain, add it in your host's settings and follow its DNS steps.

## 4. Turn on Google sign-in and cloud sync
1. Create a project at console.firebase.google.com and add a Web app (the `</>` icon). Copy its config object.
2. Build > Authentication > Sign-in method: enable **Google**.
3. Authentication > Settings > Authorized domains: add your live domain (for example `yourname.github.io`). `localhost` is already there.
4. Build > Firestore Database: create a database, open Rules, paste the contents of `firestore.rules`, and publish. (With the Firebase CLI, `firebase deploy --only firestore:rules` does this.)
5. Paste your config into `public/config.js` in place of `null`, then commit and push.

The Firebase web config is not a secret. Your data is protected by the rules in step 4, which let each user read and write only their own document.

## 5. Calendar and reminders
- **Any calendar (no setup):** every item has an "Add to Google Calendar" button, and Calendar > Export .ics downloads a file that Apple Calendar, Outlook and Google Calendar can import.
- **Live Google Calendar import:** needs the Firebase setup above. Also in Google Cloud Console (same project as Firebase), enable the **Google Calendar API**, and on the OAuth consent screen add the scope `.../auth/calendar.readonly`. While your app is in "Testing" mode, add your own Google account as a test user. Then use Sync Google Calendar in the Calendar view or the profile menu. Events are imported as read-only "Google" items for the last 7 and next 60 days. Press Sync again to refresh.
- **Reminders:** click Enable reminders in the Notifications card. Timed items alert 10 minutes before they start, and the focus timer alerts when it ends. These alerts work while the app is open in a tab or installed window. Alerts when the app is fully closed need push notifications (Firebase Cloud Messaging plus a small server function), which are not included.

## 6. Install as an app
On the live site, use "Install" in the browser address bar (Chrome/Edge) or Share > Add to Home Screen (iOS).

## Notes
- Without a Firebase config, tasks stay in each browser's localStorage and do not sync.
- Each user's tasks are one Firestore document (1 MB limit), which is plenty for a personal list. If two devices edit at the same moment, the last save wins.
- If you change `index.html` after deploying, users get the update on their next load. To force-refresh cached files, change `V` in `sw.js`.
