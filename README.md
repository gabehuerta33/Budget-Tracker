# Budget Tracker

A personal budget and investment tracker with real accounts, cloud sync, and support for both military and civilian pay structures. Built as a single-file web app — no build step, no backend server, just static hosting and Firebase.

**This is a private project** — sign-up is invite-only. Access is limited to the people the owner shares the link and account details with directly.

## Features

- **Home dashboard** — a single "safe to spend" number, quick stats, and a quick-add form for logging purchases in seconds
- **Budget tab** — Necessities, Savings & Investing, and Discretionary/Misc against 50/30/20 targets, with fully custom categories you can add or remove yourself
- **Analytics tab** — category breakdown chart, spending trends over the last 12 months, budget health bars, and automatic callouts when a category runs above its normal average
- **Investments tab** — Roth IRA and Brokerage tracking with editable fund allocations and a contribution-room check against the current year's IRS limit
- **Pay & TSP tab** — supports both **Semi-Monthly** (e.g. active duty) and **Bi-Weekly** (e.g. GS/DA civilian) pay schedules, with the math handled correctly for each
- **Multi-user accounts** — sign in with email/password or Google; everyone's data is private and synced across their own devices
- **Guided tours** — a spotlight walkthrough for every tab, available anytime from Settings
- **Excel export** — download a snapshot of your data formatted to match a full workbook
- **Backup/restore** — export or import your full account as a `.json` file

## Tech stack

- Vanilla HTML/CSS/JavaScript — no framework, no build step
- [Firebase Authentication](https://firebase.google.com/docs/auth) — user accounts (email/password + Google)
- [Cloud Firestore](https://firebase.google.com/docs/firestore) — per-user data storage, locked down by security rules so each account can only read/write its own data
- [Chart.js](https://www.chartjs.org/) — analytics charts
- [SheetJS](https://sheetjs.com/) — Excel export
- Hosted free on [GitHub Pages](https://pages.github.com/)

## Access

Public self-signup is disabled (`INVITE_ONLY = true` near the bottom of `index.html`). To add someone:

- **Email/password:** in the Firebase console, go to **Authentication → Users → Add user**, create their account, and share the email/temporary password with them directly. They can change the password themselves from the login screen's "Forgot password?" link.
- **Google Sign-In:** a first-time Google sign-in attempt is automatically blocked and signed back out with a message telling them to contact you. There's currently no way to pre-provision a Google-linked account without a backend, so Google is really only useful for someone who already has an email/password account and wants to link it — anyone new should be given email/password credentials instead.

To open the app up for public self-signup later, set `INVITE_ONLY = false` in `index.html`.

## Setting up your own copy

1. Create a free [Firebase](https://console.firebase.google.com/) project.
2. Enable **Authentication** (Email/Password and/or Google) and **Cloud Firestore**.
3. Apply this Firestore security rule so users can only access their own data:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /users/{userId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
       }
     }
   }
   ```

4. Grab your Firebase config from **Project settings → General → Your apps**, and paste the six values into the `firebaseConfig` object near the bottom of `index.html`.
5. Upload `index.html` to a GitHub repository and enable **GitHub Pages** (Settings → Pages → deploy from the `main` branch).
6. In Firebase, add your new GitHub Pages domain under **Authentication → Settings → Authorized domains**.

## Notes

- This is a personal project, not a commercial product or official pay calculator. Double-check anything tax- or pay-related (like IRS contribution limits) against official sources — the app includes reminders for this where it matters.
- Each user's data lives in their own Firestore document, private to their account. Nothing is shared between users.
