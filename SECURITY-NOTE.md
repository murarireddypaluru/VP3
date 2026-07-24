# Security Note

This project originally committed a live Firebase configuration (API key + Realtime Database URL) in its
source. Those values have been replaced with placeholders (`YOUR_FIREBASE_API_KEY`,
`https://YOUR_PROJECT.firebaseio.com`) and the git history has been rewritten to remove the real key.

A Firebase **web API key is an identifier, not a true secret** — the real protection for your data is the
Realtime Database **security rules**. If the database this app used is still live, secure it:

1. Open <https://console.firebase.google.com> → this project → **Realtime Database → Rules**.
2. Set rules to deny public access, e.g. `{ "rules": { ".read": false, ".write": false } }`, or delete the
   project if it was only a throwaway demo.

To run this project yourself, drop your own Firebase config into the placeholder fields.
