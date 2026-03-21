# SimpleKit Loan Calculator

SimpleKit Loan Calculator is a static browser tool for estimating loan payments, comparing financing scenarios, modeling extra payments, reviewing a full amortization schedule, exporting/importing JSON scenario files, and printing a PDF-style report through the browser.

## What the tool does

- Calculates regular loan payments for monthly, semi-monthly, bi-weekly, and weekly payment frequencies
- Generates a line-by-line amortization schedule with payment date, scheduled payment, extra payment, principal, interest, and remaining balance
- Supports multiple saved scenarios for side-by-side loan comparison
- Models recurring extra payments and dated lump-sum payments
- Auto-saves current work in `localStorage`
- Exports and imports all scenarios as versioned JSON
- Provides a print-friendly report layout for browser PDF export

## Save and load JSON

Use the `Save JSON` button to export the current calculator state to a file named like:

```text
simplekit-loan-scenarios-YYYY-MM-DD.json
```

The export includes:

- `schemaVersion`
- `exportedAt`
- app settings such as currency
- selected scenario and comparison state
- every loan scenario, including extra payments and lump sums

Use the `Load JSON` button to import a previous export. The app validates the JSON structure before loading it and rejects unsupported schema versions or malformed payloads with a friendly message.

## Local auto-save

The calculator also writes the current state to browser `localStorage` under:

```text
simplekit-loan-calculator-state-v1
```

This protects against accidental refreshes on the same device and browser. The `Reset all` action clears the active in-memory state and removes the saved local copy after confirmation.

## Print and Save PDF

The `Print / Save PDF` action uses `window.print()` and a print stylesheet instead of a server PDF generator.

The print view is designed to:

- hide scenario editing controls and other non-essential UI
- keep the report summary visible and paper-friendly
- preserve key charts, comparison output, and amortization content
- avoid awkward card/table breaks when possible

From the browser print dialog, choose your normal printer or a PDF destination to save the report as a PDF.

## Amortization math assumptions

The financial engine uses these assumptions:

- Regular payments use the standard amortizing loan payment formula
- A zero-interest loan is handled as straight-line principal repayment across the scheduled number of payments
- For mixed compounding and payment frequencies, the code converts the nominal annual rate into an effective annual rate first, then converts that to the payment-period rate
- Upfront fees are treated as an out-of-pocket cost included in `total paid`, but they are not added to the financed balance
- Extra payments are applied on or before the next scheduled payment date, which allows recurring extras and lump sums to work even when their frequency differs from the base payment frequency
- Monthly payment dates preserve month-end behavior where practical; semi-monthly, bi-weekly, and weekly schedules use fixed day-count steps appropriate to their frequency
- Final payments are capped so the schedule does not overpay the remaining balance
- Currency values are rounded to cents throughout the schedule for user-facing consistency

Because lender contracts can use their own rounding rules, payment timing rules, fee treatment, and compounding conventions, the calculator is for planning and comparison rather than a binding lending quote.

## File structure

```text
/
  index.html
  assets/
    css/
      styles.css
    js/
      app.js
  README.md
```

## Shared SimpleKit integration

This tool keeps the existing SimpleKit integration intact:

- shared core shell from `https://core.simplekit.app`
- shared header, support, and footer mount points
- existing Google Analytics snippet in `index.html`

No build step or backend is required.
