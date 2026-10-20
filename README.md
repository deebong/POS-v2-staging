# FreshMart POS — Staging / Acceptance

**Reviewed:** 21 October 2026  
**Repository role:** isolated public staging deployment for candidate acceptance. This is not production and not the authoritative source repository.

- **Staging site:** https://deebong.github.io/POS-v2-staging/
- **Authoritative source:** https://github.com/deebong/POS-platform-v2
- **Production-facing site:** https://deebong.github.io/POS-v2/
- **Current test data:** use only the designated disposable `POS_test_v1` Google Sheet and its dedicated Apps Script deployment.

## Current deployment record

The previous staging README recorded source branch `8/merge`, source commit `8e1c296c0ebed691d91d57f22be02c9d2d6bc37e` and successful certification workflow run #734. Treat those values as the last recorded deployment in the old note, **not proof that the live staging site currently serves that exact commit**. Verify the latest staging deployment commit/service-worker stamp before testing and record it in the regression log.

## What staging is for

Use this site to reproduce bugs and test candidate builds against disposable data before production release. It is the required acceptance target. Do not enter real customer, sales, financial or production credentials/data.

The app is a static HTML/CSS/JavaScript PWA using IndexedDB and, when configured, Google Sheets through Google Apps Script. The browser may retain local data even if the cloud connection fails; a successful page load does not prove sync is healthy.

## Setup and test-data rules

1. Identify the intended disposable Sheet and its Apps Script `/exec` deployment URL before opening installation.
2. Confirm the staging build points to that deployment. Record the test label, Sheet, deployment, source commit and staging build privately.
3. After Apps Script edits, deploy a new Web App version and confirm the frontend uses the correct URL.
4. Verify a harmless read/write against the disposable Sheet before running transaction tests.
5. Do not clear IndexedDB, uninstall the PWA or create another Sheet until local data is backed up and the test purpose is clear.
6. If the application says the backend is already installed, identify the connected Sheet/deployment before attempting recovery. Do not bypass the warning blindly.

## Modules available to test

The current frontend includes code/UI for Dashboard, POS/Billing, Inventory, Product Labels, Invoices/Sales, Customers, Suppliers/Purchases, Returns/Exchanges, Cash Drawer, Day Close, Audit Log, Low Stock, Reports, Loyalty, Staff & Users, Settings, GST bill/export, barcode/QR helpers and PWA/offline support.

Their implementation status is mixed. Cash Drawer, Day Close, Audit Log and parts of Loyalty are local operational stores rather than fully centralized Sheets-backed modules; complete loyalty checkout integration, server-authoritative staff/session enforcement and multi-store isolation remain incomplete or unverified. Payment tender choices are not gateway integrations, and browser printing/scanning does not certify physical hardware.

## Acceptance checklist

Use the source repository's [Regression and Release Log](https://github.com/deebong/POS-platform-v2/blob/main/docs/REGRESSION-AND-RELEASE-LOG.md) as the detailed checklist. At minimum, record evidence for:

- Operator switching: Manager ↔ Cashier, dashboard redirect, fresh sidebar/navigation permissions and no stale protected content.
- Cash Drawer: open/close, cash-in/out, operator attribution and persistence.
- Product Labels: printed dimensions and scaling on the target printer.
- Returns: live backend transaction, stock adjustment, refund and authorization.
- Product editing: update the existing stable record/SKU.
- Tamil names through create/edit/import/export/search/labels/receipt/GST.
- Settings persistence across save, navigation, reload and re-login.
- Backup/restore destinations, permissions and data integrity.
- Wrong PIN, lockout, logout, session expiry and role changes.
- Sync retries, offline operations, separate store scopes and data isolation.
- PWA update/cache activation, installed-app reopen and offline recovery.
- Receipt/GST output including ₹ glyph and real printing.

Label every result as **code present**, **automated test passed**, **live backend verified**, **target device verified** or **not verified**. Do not call a test passed if it did not exercise that behavior.

## Deployment policy

- Candidate source changes belong in `POS-platform-v2`.
- Deploy to this staging repository first and run acceptance checks with disposable data.
- Do not manually patch staging files unless explicitly agreed as a temporary emergency fix; reconcile any such patch back to source.
- A green CI run is not proof of live Google Sheets, hardware, security or all workflows.
- Do not publish to `POS-v2` until a separate, explicit production release decision has been made and the workflow is confirmed to enforce that gate.

## Planned features — retained

The broader roadmap remains in scope: multiple payment gateways/UPI IDs; delivery and logistics; finance/expense tracking; thermal printers, barcode/QR scanners and weighing machines; per-client/per-industry dashboard themes; non-technical guided onboarding; configurable enable/disable plugins; grocery product-image plugin; Tally migration; ERP/accounting/other app integrations; multi-store/multi-counter capability; and additional industry-specific workflows.

This README describes the staging environment and acceptance process; the [source README](https://github.com/deebong/POS-platform-v2/blob/main/README.md) contains the fuller architecture, source file map, module inventory and roadmap.
