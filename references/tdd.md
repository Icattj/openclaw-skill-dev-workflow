# Test-Driven Development — Framework Patterns

## Node.js (Jest/Vitest)

```javascript
// RED: Write the failing test
test('calculateTotal returns sum with tax', () => {
  expect(calculateTotal([10, 20], 0.1)).toBe(33);
});

// GREEN: Minimal implementation
function calculateTotal(items, taxRate) {
  const sum = items.reduce((a, b) => a + b, 0);
  return sum + sum * taxRate;
}

// REFACTOR: Clean up if needed
```

## Python (pytest)

```python
# RED
def test_parse_invoice_extracts_total():
    result = parse_invoice("tests/fixtures/invoice1.pdf")
    assert result["total"] == 150000

# GREEN
def parse_invoice(path):
    # minimal implementation
    return {"total": 150000}

# REFACTOR → extract to proper parser class
```

## Bash (Test with assertions)

```bash
# RED: Expected output
echo "Testing CSV export..."
RESULT=$(node scripts/export.js --format csv)
echo "$RESULT" | grep -q "name,email" || { echo "FAIL: Missing headers"; exit 1; }
echo "PASS"
```

## What to Test

**Test behavior, not implementation:**
- ✅ "When I submit an empty form, I see an error message"
- ❌ "The validateForm function calls setError with 'required'"

**Test boundaries:**
- Empty input, null, undefined
- Maximum size, minimum size
- Special characters, Unicode
- Concurrent access (if applicable)

**Test the happy path AND the sad path:**
- ✅ Valid input → expected output
- ✅ Invalid input → proper error
- ✅ Edge case → graceful handling
