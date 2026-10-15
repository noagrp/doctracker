# DocTracker

A small, offline-first accounts clerk PWA. No Firebase, login, server database or external runtime libraries.

## Core workflow
1. Open **Settings** and enter the main company profile.
2. Add entries in **Businesses**, ticking **Customer**, **Supplier**, or both.
3. Enter incoming supplier invoices or outgoing customer invoices.
4. Record receipts/payments, allocating each transaction to one or more invoices.
5. View monthly reports, print or save PDF through the browser print dialog, and export Excel-compatible `.xls` reports.

## Storage and offline use
- Data is automatically saved to **IndexedDB** on this device and browser origin.
- DocTracker limits serialized ledger data to **4.5 MiB**, presenting usage as a percentage.
- Install from your browser's PWA install option where supported.
- The service worker caches the app shell and icons for offline use after an initial successful visit.
- Each origin/browser/profile has separate data. Clearing browser data, private browsing, storage eviction or losing the device can remove records. **Make regular backups.**
- The application attempts to request persistent browser storage when supported, but persistence is not guaranteed.

## Backups and archives
- **Full Backup:** JSON export with all businesses, company settings, invoices, payments, notes and archive receipts.
- **Restore:** validates a DocTracker backup and replaces the current ledger after confirmation. Export a backup before restoring.
- **Export Invoices:** JSON snapshot of the invoice dataset; it does not remove working records.
- **Completed archive:** exports fully paid invoices together with linked payment and CN/DN records; deletion requires user confirmation **after** initiating download. Because browsers cannot reliably prove the file was saved, the user must verify the download before confirming.
- Mixed payment allocations spanning settled and unsettled invoices prevent archive deletion until those allocations can be safely moved together.
- **Import Archive:** restores archived records if they do not collide with working invoice IDs.

## Reporting
Excel export uses SpreadsheetML XML with the `.xls` extension (Excel-compatible, not native `.xlsx`). Some spreadsheet applications may display a format warning. PDF generation uses the browser **Print / Save PDF** dialog and does not create a PDF file directly.

The monthly **Invoice Summary** groups invoices by invoice date and shows their cumulative payments to date. The **Payment Activity** report groups by payment date. These are different views and should not be treated as equivalent monthly cash movement.

## Deployment
Static hosting is sufficient (for example, GitHub Pages). The app must be served from HTTPS or localhost for normal service-worker operation. PWA storage belongs to the hosting origin and path configuration.

## Scope and current limitations
- Intended for a single company ledger on one device, not multi-user synchronization.
- Payments can be allocated to multiple invoices for the same business.
- No attached documents, accounting journals, bank reconciliation, taxes or inventory.
- CSV/native XLSX generation, password protection, and automated filesystem write-back are not included.
- Current archive interface archives all eligible settled invoices at once; granular selection is not yet included.
- An archive record is a receipt, **not** an internal copy of the downloaded file.
- Before production use, test backup and restore on a separate browser profile, print formatting, amount rounding, large data capacity, and archival recovery.
