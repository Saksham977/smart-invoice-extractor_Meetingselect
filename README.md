# smart-invoice-extractor_CommiIQ
AI-powered invoice parsing engine with OCR fallback and intelligent amount extraction.

Project Summary
The need:
Travel Industry usually earns commission on hotel and venue bookings, calculated as a percentage of specific billable line items on each client invoice — things like room nights, package charges, F&B, and room rental. Today, identifying and summing those commissionable line items means manually reading through long, inconsistently formatted hotel invoices (often 5–90+ pages), some of which are native text PDFs and some of which are scanned images with no selectable text at all. This is slow, repetitive, and error-prone — a missed or misread line item directly understates the commission owed.

What is being built?
Commi IQ is a browser-based invoice analyzer that runs entirely client-side — no invoice data is ever uploaded to a server. The user uploads a PDF, tells the app whether it is text-based or scanned, and the app extracts every line item, its date, its VAT rate, and its amount. The user then enters the billing terms they want to audit (e.g. "Night Stay", "Room", "Package Charge") and the app searches, sums, and splits each match into Net (excl. VAT), VAT amount, and Gross (incl. VAT) — both per VAT rate and as a combined total — so commission can be calculated directly from the correct base amount.

How it works, in brief
•	Text-based PDFs (selectable text, e.g. exported from a hotel PMS) are parsed directly from the embedded text layer using PDF.js — fast, and 100% accurate to what’s encoded in the file.
•	Scanned PDFs (flattened images, no text layer) are run through Tesseract.js, an in-browser OCR engine, to recognize the text before the same line-item extraction logic is applied.
•	A single forced choice (radio button) before upload tells the app which pipeline to use, since the two require fundamentally different handling and a misclassified PDF produces wrong results.
•	Extracted rows are grouped by date, merged across wrapped/multi-line descriptions, and parsed for VAT rate and amount using a rightmost-decimal-wins rule that mirrors how these invoices lay out their columns.
•	All results stay in the browser tab’s memory and auto-delete after 21 minutes — nothing is logged or transmitted.
•	Once an invoice is analyzed, a few extras reduce repeat-visit effort: frequency-based suggestion chips surface phrases that recur often in that invoice (no AI judgment, purely a count of repeated short phrases), a saved term-list feature remembers a hotel’s usual audit terms for next time, and each term’s result can be exported to a clean Excel file or copied at a glance in European number format for direct use on a commission line.

Current status
The extraction and audit logic has been validated against multiple real hotel invoice formats covering edge cases such as European decimal-comma formatting, multi-page two-column OCR layouts, negative/credit line items, VAT-exempt items, and duplicate total values. An automated regression suite covering 120+ specific cases — spanning extraction, the Excel export, saved presets, and the suggestion feature — currently passes in full.
