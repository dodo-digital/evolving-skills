# Model Testing Patterns

Skills act as additions to models. What works for GPT-5.5 might need more detail for GPT-5.4-Mini.

## Testing by Model

| Model | Tendency | What It Needs |
|-------|----------|---------------|
| GPT-5.4-Mini | Needs guidance | Explicit instructions, complete examples, step-by-step |
| GPT-5.4 | Balanced | XML structure, progressive disclosure, concise |
| GPT-5.5 | Works with principles | High freedom, trust reasoning, minimal prescription |

## GPT-5.4-Mini Testing

Questions to ask:
- Does skill provide enough guidance?
- Are examples complete (no partial code)?
- Are implicit assumptions explicit?

## GPT-5.4 Testing

Questions to ask:
- Is skill clear and efficient?
- Does it avoid over-explanation?
- Does progressive disclosure work?

## GPT-5.5 Testing

Questions to ask:
- Does skill avoid over-explaining?
- Can GPT-5.5 infer obvious steps?
- Is context minimal but sufficient?

## Balancing Example

**Good balance (works for all):**
```xml
<quick_start>
Use pdfplumber for text extraction:

```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

For scanned PDFs requiring OCR, use pdf2image with pytesseract.
</quick_start>
```

**Too minimal for GPT-5.4-Mini:**
```xml
<quick_start>
Use pdfplumber for text extraction.
</quick_start>
```

**Too verbose for GPT-5.5:**
```xml
<quick_start>
PDF files are documents that contain text. To extract that text, we use a library called pdfplumber. First, import the library at the top of your Python file...
</quick_start>
```

## Best Practice

Write for GPT-5.4 (medium detail), then:
1. Test with GPT-5.4-Mini (catches under-specification)
2. Test with GPT-5.5 (catches over-specification)
3. Adjust based on actual performance
