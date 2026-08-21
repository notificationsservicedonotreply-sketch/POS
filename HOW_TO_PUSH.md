# How to get this onto GitHub

You have two files:

- **`POS-NEW-STORE-feature-pos-enhancements.zip`** — the full project source, exactly as it looks on the new `feature/pos-enhancements` branch (one commit on top of your current `main`).
- **`pos-enhancements.patch`** — the same change as a git patch, in case you'd rather apply it to your own local clone directly instead of copying files.

## Option A — easiest (upload the branch via GitHub's web UI)
1. Unzip `POS-NEW-STORE-feature-pos-enhancements.zip`.
2. On GitHub, go to your repo → **Add file → Upload files**, switch the target branch dropdown (top left, next to "commit to") to a **new branch**, name it `feature/pos-enhancements`, and drag in the unzipped files (this will overwrite/add just what changed since GitHub diffs by path).
3. Open a Pull Request from `feature/pos-enhancements` into `main`.

## Option B — from your terminal (recommended, keeps history clean)
```bash
git clone https://github.com/KINGMANCHY/POS-NEW-STORE.git
cd POS-NEW-STORE
git checkout -b feature/pos-enhancements
git am /path/to/pos-enhancements.patch
git push -u origin feature/pos-enhancements
```
Then open a PR from `feature/pos-enhancements` into `main` on GitHub as usual.

If `git am` complains about a conflict, it means `main` has moved since this was built — use `git am --abort`, then `git apply --3way /path/to/pos-enhancements.patch` instead and resolve by hand.

## Before you deploy
1. Run these migrations against your existing database (all are safe/idempotent - they only insert/create if missing). Skip all three if you're seeding a brand-new DB from `pos_store.sql`, which already includes them.
   - `database/migration_receipt_template_settings.sql`
   - `database/migration_cash_reconciliation.sql`
   - `database/migration_pwd_senior_discount.sql`
2. The barcode/QR **camera** tab needs HTTPS (or `localhost`) to get camera permission from the browser — that's a browser requirement, not something this code can work around.
3. Everything else (receipt templates, reports filters, full-screen, dark mode, item/qty totals, split-payment cash lock, Payment Complete modal, Transaction Record & Reconciliation, per-item price/discount, Senior/PWD discount) works with no extra setup.

## What changed
Six commits on `feature/pos-enhancements`:
1. Receipt Templates, Barcode/QR Search, Reports filters, Full-screen, Dark/Light mode, cart totals.
2. Dark mode contrast fix, split-payment cash lock, Payment Complete modal, Transaction Record + End of Day Reconciliation.
3. Every split-payment method (not just cash) now locked to one use, Receipt now shows before Payment Complete, Change amount enlarged in Payment Complete.
4. "End of Day" button + modal on the POS Screen (reuses the Reconciliation page's own logic), and per-item bargained price/discount in the POS cart.
5. Moved "End of Day" out of the POS Screen and into the Reconciliation page (button + modal there instead); POS cart price/discount now open "Edit Price" / "Add discount on this item" modals instead of inline textboxes.
6. Fixed the cart's per-line Total (was double-adding tax), aligned the End of Day button's size, and added a configurable Senior Citizen / PWD statutory discount (Settings + Payment modal + receipt).

See each commit message (or the top of the patch file) for the full breakdown.
