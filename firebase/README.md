# Firebase rules and existing staff accounts

These rules are for the Firebase project configured in `.firebaserc` (`bishop-of-souls`). Review the files before publishing them.

## Firestore rules

In Firebase Console, open **Firestore Database → Rules**, replace the current rules with the complete contents of `firebase/firestore.rules`, then publish. The rules use a one-time first-administrator claim guarded by a setup code. The website and rules currently use `BISHOP-SETUP-2026`. Before first use, replace it with a new private one-time value in both this rules file and the dashboard's Go live code; keep the two values identical. After the first administrator claim, the rules prevent another bootstrap.

## Use the two existing email/password accounts

1. Make sure **Authentication → Sign-in method → Email/Password** is enabled and Firestore exists.
2. From the hosted website, open `/dashboard.html` (the footer **Staff login** link now opens this page directly).
3. If the Firebase site has not been bootstrapped, open **Go live**. Enter the first existing Firebase email and its current password in the account/password fields, along with the one-time setup code. The dashboard signs into an existing email when it is already registered, claims it as the first Super Administrator, and publishes any local preview content.
4. Sign in with the second existing email. Its profile starts as **Pending**. The first Super Administrator can open **Users & roles** and assign that account its role.
5. If this Firebase project already has the `config/bootstrap` marker, skip Go live and sign in normally. A new account's profile is created as Pending on first sign-in.

Add the deployed domain to **Authentication → Settings → Authorized domains** if Firebase reports `auth/unauthorized-domain`. The email/password values stay in Firebase Authentication; never add passwords or service-account keys to this website ZIP.

## Deploy

The Firebase CLI configuration stays at the website root so deployment uses the root HTML pages. To publish Hosting, Firestore rules/indexes, and Storage rules from this folder, run `firebase deploy --project bishop-of-souls`. For Hosting only, run `firebase deploy --only hosting --project bishop-of-souls`. The command requires Firebase CLI login. The HTML pages are at the root; browser assets are in `website-files/`; rule files are in `firebase/`.

`firebase/storage.rules` is included for the configured Storage bucket. The current staff image workflow stores resized image data in Firestore documents, so Firestore rules are the primary access controls for that feature. No Firebase service-account key is included.

## Local preview and YouTube playback

Opening an HTML page directly as a ile:// URL is useful for checking the layout, but browsers do not send the website HTTP referrer from local files. YouTube therefore returns Error 153 for embedded playback. Test video playback on the deployed HTTPS site (or serve this folder from a local HTTP server). The player embeds now send the page origin and request the origin referrer when served over HTTP/HTTPS. Videos disabled for embedding by their owner may still need to be watched on YouTube.
