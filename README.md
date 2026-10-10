# Taxwise India — Personal Tax & Finance Planning System

A browser-based Indian tax estimation and personal finance planning application. This release extends the supplied Phase 1 tax calculator with residential-status assessment and a set of interactive planning calculators.

> **Status:** Demonstration estimator. It is not a tax filing product, a source of professional advice, or ready for production use with real identity or financial data. Review the limitations below before relying on any result.

## What is included

### Tax calculation

- Annual income inputs for salary, house property, business/profession, other income, foreign income, and Section 111A/112A equity gains.
- Old-regime and new-regime estimates for **FY 2025–26 (AY 2026–27)**, including a simplified age-band option for old-regime slabs.
- Standard deduction and selected deductions (80C, 80D, 80CCD(1B), and home-loan interest), Section 87A rebate, surcharge estimate, cess, and foreign-tax-credit estimate.
- TDS, advance-tax, and self-assessment-tax inputs with estimated refund or balance payable.
- Detailed tax breakdown, user-selected final regime, downloadable PDF report for either regime, print view, and a locally saved draft.
- Residential-status questionnaire with a reasoning trace.

### Personal finance planning tools

- **Loans:** EMI and amortization, loan eligibility, debt-to-income, prepayment, foreclosure, and balance-transfer estimates.
- **Investments:** SIP and step-up SIP, lump sum, FD, RD, PPF, NPS, mutual-fund return, goal-based and retirement projections.
- **Tax helpers:** HRA, Section 44ADA, advance-tax, and tax-paid/refund estimates.
- **Salary and personal finance:** in-hand salary, EPF, gratuity, leave encashment, bonus, budget, emergency fund, savings rate, and net worth.
- Planning results use user-supplied assumptions. Amortization and planning-tool results can be exported as CSV.

### Application and API

- Responsive web interface with a 20px base text size and the selected nature-inspired background.
- React/Vite frontend and FastAPI backend.
- REST-backed calculation services with decimal arithmetic and input validation.
- FastAPI interactive API documentation at `/docs`.
- Account registration, login, refresh, logout, and `/api/auth/me` API endpoints.
- PostgreSQL and Python runtime are bundled in the Windows package.

## Run locally on Windows

1. Download and extract the release package, keeping `tax-calc-system` beside its `native-runtime` folder.
2. Open the project folder and run `start-native.bat`.
3. Open the frontend at [http://localhost:5173](http://localhost:5173) or the API reference at [http://localhost:8000/docs](http://localhost:8000/docs).

The frontend build requires Node.js. Alternatively, run `docker compose up --build` from the project folder if Docker is installed.

## Project structure

- `frontend/` — React/Vite user interface and client-side PDF/CSV/report interactions.
- `backend/app/` — FastAPI application, tax and finance calculation services, API routes, and tests.
- `native-runtime/` — bundled Windows PostgreSQL and Python runtime (distributed in the downloadable package).
- `start-native.bat` — Windows launcher.
- `docker-compose.yml` — container-based development/run configuration.

## Key API routes

- `POST /api/finance/calculate` — validate and calculate a supported planning tool.
- `/docs` — interactive OpenAPI/Swagger documentation.
- Authentication routes provide registration, login, refresh, logout, and current-user access. Refer to the backend OpenAPI document for exact request and response schemas.

## Development and checks

### Frontend

```powershell
cd frontend
npm ci
npm run build
```

### Backend

```powershell
cd backend
python -m unittest discover -s app/tests -p "test_tax_calculation_service.py" -v
python -m unittest app.tests.test_finance_tools_service -v
python -m pytest app/tests/test_residential_status_service.py -q
```

The tests require the backend dependencies listed by the project. The residential-status test uses pytest.

## Tax rules and financial assumptions

Rules are embedded for FY 2025–26 only and are not refreshed automatically. Confirm applicable rules against official guidance before use:

- [Income Tax Department — individual return guidance](https://www.incometax.gov.in/iec/foportal/help/individual/return-applicable-1)
- [Income Tax Department — ITR-2 guidance](https://www.incometax.gov.in/iec/foportal/help/all-topics/e-filing-services/file-itr-2-online)

The planning calculators use the rates, periods, caps, and assumptions supplied by the user. They do not guarantee returns, eligibility, provider terms, or tax outcomes.

## Known limitations

This is not a complete tax return calculator or finance SaaS platform.

- Only FY 2025–26 is supported; there are no tax-rule datasets for other financial years.
- Income is entered as annual category totals. Multiple employers/properties/transactions and source-document ingestion are not supported.
- Loss set-off/carry-forward, agricultural income, detailed exemptions and house-property rules, many capital-gains categories and special rates, AMT, interest/late fees, and filing eligibility are not fully modeled.
- Deduction caps and foreign-tax-credit treatment are simplified. Rebate and surcharge marginal relief, DTAA decisions, RNOR sourcing exceptions, and statutory eligibility checks are not determined.
- Section 44ADA, HRA, home-loan tax benefits, and salary/advance-tax helpers are indicative; they do not verify statutory conditions, deadlines, installment rules, or all applicable tax treatment.
- Finance planning tools are not linked to a persistent user ledger or account history. There is no expense transaction management, portfolio synchronization, notification delivery, AI financial-advisor integration, admin tax-rule publisher, password reset, or email verification.
- Browser drafts are saved in local storage and are not encrypted. Authentication endpoints do not make the planning data account-bound. The PDF endpoint generates a report on demand and does not save it.
- Do not deploy with sensitive data or rely on results for filing or financial decisions without independent security, legal, and qualified tax-professional review.

## Downloadable package

The companion Windows ZIP is named `tax-calc-system-finance-planning-extended.zip` (approximately 382 MB). GitHub repository files have a 100 MB per-file limit, so the ZIP must be distributed as a GitHub Release asset or another binary-download artifact rather than committed as an ordinary repository file.

SHA-256: `2EC5CCC91A06F001F4489D2A4E86161E1CA9592F992321860839A537C128BD84`
