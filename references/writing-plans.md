# Writing Plans — Detailed Guide

## Plan Quality Checklist

A good plan answers:
- **What** are we building? (feature description)
- **Why** are we building it? (user need / business goal)
- **How** will we build it? (architecture, approach)
- **What files** will change? (exact paths)
- **How** will we verify it works? (tests, manual checks)
- **What could go wrong?** (risks, edge cases)

## Task Granularity

**Too big:** "Implement user authentication"
**Right size:** "Add POST /api/login endpoint that validates email/password against users table and returns JWT"

Each task should:
- Be completable in 15-30 minutes
- Touch 1-3 files maximum
- Have clear acceptance criteria
- Be independently testable

## Example Plan

```markdown
# Plan: Add CSV Export to Dashboard

**Goal:** Users can click "Export" and download their data as CSV.
**Effort:** 4 tasks, ~2 hours

## Tasks

### 1. Create CSV generation utility
- **Files:** `src/utils/csv.js`
- **What:** Function `toCSV(headers, rows)` that returns CSV string with proper escaping
- **Verify:** Unit test with special characters (commas, quotes, newlines)

### 2. Add export API endpoint
- **Files:** `src/api/export.js`, `src/api/routes.js`
- **What:** GET /api/export?format=csv returns CSV with Content-Disposition header
- **Verify:** curl test returns valid CSV file

### 3. Add export button to dashboard UI
- **Files:** `src/components/Dashboard.jsx`
- **What:** "Export CSV" button that calls /api/export and triggers download
- **Verify:** Click button → file downloads in browser

### 4. Add loading state + error handling
- **Files:** `src/components/Dashboard.jsx`
- **What:** Show spinner during export, toast on error
- **Verify:** Slow network simulation shows spinner

## Risks
- Large datasets may timeout → add pagination or streaming
- Special characters in data may break CSV → test with Unicode
```
