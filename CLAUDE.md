# Expense Tracker App

## What this app is
A personal expense tracker web app for a single user based in Kozhikode, Kerala, India. All amounts are in INR (₹). The app runs in the browser as a single HTML file. Data is stored in Google Sheets via a Google Apps Script Web App URL.

## Core purpose
1. Track family expenses that will be reimbursed by the user's father
2. Track personal expenses with AI-powered categorisation
3. Share family expense list to WhatsApp in one tap when settlement is due

## Two expense types
- **Family** — amount + description + date only. No category. Goes into reimbursement log.
- **Personal** — amount + description + date + AI auto-category. Goes into personal spending log.

## Personal expense categories (specific, not broad)
Essentials: Groceries, Fuel, Utilities, Medical, Education
Lifestyle: Dining Out, Shopping, Entertainment, Subscriptions
Household: Home Supplies, Repairs & Maintenance
Other: Charity / Zakat, Miscellaneous

## AI categorisation
- Uses Claude API (claude-sonnet-4-20250514) to categorise personal expenses
- Input: description typed by user
- Output: one category from the fixed list above
- User can override the AI suggestion before saving

## Settlement flow (Family tab)
- All unsettled family expenses show with running total
- "Share with Father" button generates a WhatsApp message with itemised list + total
- After sharing, user manually marks as "Settled" via confirmation modal
- Settled expenses are archived in Google Sheets — removed from active view
- Next settlement cycle starts fresh

## Home screen
Two clear sections visible at a glance:
- Family: running total of unsettled expenses
- Personal: this month's total spend + breakdown by category

## Data storage — Google Sheets via Apps Script
- All data is stored in a Google Sheet with two tabs: Expenses and Settlements
- The app communicates with the sheet via a Google Apps Script Web App URL
- This URL is entered by the user in the Settings screen and saved in localStorage
- The Claude API key is also entered in Settings and saved in localStorage
- On app load, fetch all expenses from the sheet
- On save, post new expense to the sheet
- On settle, update settled status in the sheet

## API calls to Apps Script
All calls are POST requests to the Apps Script Web App URL with JSON body:

Add expense:
{ action: "addExpense", id, date, type, amount, description, category, settled: "unsettled" }

Get all expenses:
{ action: "getExpenses" }

Settle expenses:
{ action: "settleExpenses", ids: [...], total }

## Sheet structure
Expenses tab columns: id | date | type | amount | description | category | settled | createdAt
Settlements tab columns: settledAt | total | expenseIds

## Design principles
- Mobile-first (used primarily on phone)
- Fast to add an expense — minimum taps
- Clean, no clutter
- Deep green primary colour (#1B4332)
- Font: Inter or system-ui
- Smooth transitions between screens