# JAMB CBT – Powered by Step By Step Digital

Static site (one `index.html`) + Firebase (Auth + Firestore). Deploys to Vercel with no build step.

## 1. Create the Firebase project (5 minutes)
1. Go to https://console.firebase.google.com → **Add project**.
2. **Build → Authentication → Get started → Email/Password → Enable**.
3. **Build → Firestore Database → Create database** (production mode).
4. **Project settings (gear) → Your apps → Web (</>)** → register app → copy the `firebaseConfig` values.
5. Open `index.html`, find `firebaseConfig` near the top of the `<script>` and paste your values.
6. In the same place, set `ADMIN_EMAIL` to the email you will use as admin (lowercase).

## 2. Publish the security rules
1. Open `firestore.rules`, replace `youradmin@email.com` with the same admin email.
2. Firebase console → **Firestore → Rules** → paste the whole file → **Publish**.

These rules are what actually protect your paid questions and stop students editing prices, bank details, or their own premium status. Do not skip this step.

## 3. Deploy to Vercel
- Upload this folder to a GitHub repo, then **vercel.com → Add New → Project → Import** it (Framework: *Other*, no build command), or run `vercel` in this folder.
- In Firebase **Authentication → Settings → Authorized domains**, add your Vercel domain (e.g. `jamb-cbt.vercel.app`).
- To get the name "jamb cbt", name the Vercel project `jamb-cbt` (or attach a custom domain).

## 4. First-time admin setup
1. Open the site and **Sign up with your admin email first** (before anyone else can).
2. An **Admin** button appears in the header.
3. **Questions → Load sample questions** to test (30 original practice items), or paste real questions with **Bulk import**.
4. **Price & bank** tab: set the price, bank name, account number (`2013463188` is preloaded), account name and contact. Changes are live immediately.

## Bulk import format
```json
[
  {
    "subject": "Mathematics",
    "year": "2023",
    "question": "Solve for x: 3x + 5 = 20.",
    "options": ["3", "5", "15", "25"],
    "answer": 1,
    "explanation": "3x = 15, so x = 5."
  }
]
```
`answer` is 0–3 (A–D). Subject names must match the list in the app (e.g. `English Language`, `Physics`). The first 5 questions of each subject are automatically free trial questions; you can flip any question to Free/Premium in **Questions → Browse**.

## How payments work
Students transfer to your Kuda account, then submit their name, amount and reference. You check your Kuda alert, open **Admin → Payments** and tap **Approve**. That sets their account to premium. This is manual verification, so no payment gateway fees. For automatic verification you would need Paystack or Flutterwave.

## Banning users
**Admin → Users** → search → **Ban**. Banned users are logged out and cannot read questions or log back in. **Unban** restores access.

## Important notes
- **Questions:** the app ships with sample practice items only. Add your own real past questions (2021–2025) via bulk import. Make sure you have the right to use them.
- Firestore free tier (Spark) is enough to start; monitor reads as students grow.
- If a query shows a permission error, check that the rules were published with your admin email.
