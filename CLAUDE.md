# Office Document Auto‑Generation System – Project Memory

## Project Overview
This project is a system that uses AI to automatically generate Office documents (Word, Excel, PowerPoint) and PDFs. By providing a recipe file (.md) to the AI, it generates an HTML page from which the file can be downloaded with one click.

## Critical Constraints (Must Follow)
### 🔴 Do **Not** Delete or Modify the Following
1. **Library installation**
   ```javascript
   await micropip.install('python-docx')  // for Word
   await micropip.install('openpyxl')     // for Excel
   await micropip.install('python-pptx')  // for PowerPoint
   await micropip.install('reportlab')    // for PDF
   ```
   Removing any of these will cause a ModuleNotFoundError.

2. **Python indentation rules**
   - Python code inside the `<script type="text/python">` tag **must start at the leftmost column**
   - Do not add indentation influenced by the surrounding HTML

3. **Passing Base64 data to JavaScript**
   - The final line **must** assign `base64` to `js.xxx_base64_data`
   - Word: `js.docx_base64_data`
   - Excel: `js.xlsx_base64_data`
   - PowerPoint: `js.pptx_base64_data`
   - PDF: `js.pdf_base64_data`

## Recipe Files and Their Use
| File type | Recipe file | Library | Use cases |
|---|---|---|---|
| Word (.docx) | DOCX-RECIPE.md | python-docx | Reports, contracts, manuals |
| Excel (.xlsx) | XLSX-RECIPE.md | openpyxl | Sales tables, budgeting, data analysis |
| PowerPoint (.pptx) | PPTX-RECIPE.md | python-pptx | Presentations, proposals |
| PDF (.pdf) | PDF-RECIPE.md | reportlab | Invoices, certificates, print materials |

## Common Workflows
### When asked to generate a new document
1. Confirm the file format the user wants
2. Check that the corresponding recipe file is attached
3. Generate the HTML according to the instructions in the recipe file
4. **Always** meet the **must‑have requirements** (library installation, indentation, Base64 hand‑off)

### When generating Excel from CSV data
1. Check that both `XLSX-RECIPE.md` and the CSV file are attached
2. Read the CSV data and convert it to a DataFrame
3. Generate the Excel file with openpyxl
4. Add charts and conditional formatting as needed

## Template for Code Generation
### Basic structure (common to all formats)
```python
# 1. Required imports (start at column 0)
from [Library] import [Class]
from io import BytesIO
import base64
import js

# 2. Build the document
[Create document object]
[Add content]

# 3. Mandatory finalization
buffer = BytesIO()
[save_method](buffer)
buffer.seek(0)

bytes_data = buffer.read()
base64_data = base64.b64encode(bytes_data).decode('utf-8')

# 4. Hand off to JavaScript (required)
js.[format]_base64_data = base64_data
```

## Common Errors and How to Fix
| Error | Cause | Fix |
|---|---|---|
| ModuleNotFoundError | Library installation was removed | Restore the `micropip.install` calls |
| IndentationError | Unnecessary indentation | Start all Python code at the leftmost column |
| NameError: 'js' | Forgot `import js` | Check that required imports are present |
| No data produced | Forgot Base64 assignment | Add the `js.xxx_base64_data = base64_data` line |

## Debug Commands
- Inspect in‑memory files: `/memory`
- For error details: instruct the user to check the browser console

## Project‑Specific Notes
### Image Placeholder Feature
- Recipes v2.0 and later support image placeholders
- Do not embed images; instead, specify placement and recommended size
- Include necessary info in placeholders (file name, purpose, size)

### Performance Considerations
- First load: 2–5 seconds (Pyodide + libraries)
- Max document size: ~50 MB (browser limitations)
- For large datasets, split processing

## Frequently Used Code Snippets
### Add an Excel chart
```python
from openpyxl.chart import BarChart, Reference
chart = BarChart()
chart.title = "Title"
data = Reference(ws, min_col=2, min_row=1, max_row=5, max_col=3)
chart.add_data(data, titles_from_data=True)
ws.add_chart(chart, "E5")
```

### Add a Word table
```python
table = doc.add_table(rows=2, cols=3)
table.style = 'Light Grid Accent 1'
table.cell(0, 0).text = 'Header'
```

### Add a PowerPoint slide
```python
slide_layout = prs.slide_layouts[0]  # 0=Title, 1=Title+Content
slide = prs.slides.add_slide(slide_layout)
slide.shapes.title.text = "Title"
```

### PDF: Japanese font settings
```python
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.cidfonts import UnicodeCIDFont
pdfmetrics.registerFont(UnicodeCIDFont('HeiseiKakuGo-W5'))
styles['Normal'].fontName = 'HeiseiKakuGo-W5'
```

## How to Create a New File‑Format Recipe

### 📝 Basic steps to author a recipe
#### 1. Research the target format
- Confirm the MIME type of the target format
- Identify the required Python libraries
- Confirm the libraries are available on Pyodide ([Pyodide packages](https://pyodide.org/en/stable/usage/packages-in-pyodide.html))

#### 2. Required recipe file structure (sections)
```markdown
# **[Format Name] Auto‑Generation Page – Recipe v1.0**

## **🔴 MUST READ FIRST – Mandatory Requirements 🔴**
### **1. External library installation (never omit)**
### **2. Python indentation rules (strictly follow to avoid errors)**

## **📋 Implementation instructions for the AI**
### **What to replace**
### **Rules for writing Python code**

## **📄 HTML skeleton template**

## **⚠️ Common errors and fixes**

## **💡 Examples**

## **📚 Key features reference for [Library Name]**

## **✅ Final checklist**

## **📝 Changelog**

## **📄 License**
```

#### 3. Build the HTML template
Base it on an existing recipe’s HTML template and update:
- Page title (two locations)
- Library installation section
- Python code section
- File name and MIME type
- Base64 variable name (`js.xxx_base64_data`)

#### 4. Mandatory implementation pattern
```python
# Common pattern (required in all recipes)
from io import BytesIO
import base64
import js

# 1. Library‑specific processing
[File generation steps]

# 2. Save to BytesIO
buffer = BytesIO()
[save_method]
buffer.seek(0)

# 3. Base64 encoding
bytes_data = buffer.read()
base64_data = base64.b64encode(bytes_data).decode('utf-8')

# 4. Hand off to JavaScript (required)
js.[format]_base64_data = base64_data

print("[Format] file has been generated in memory.")
```

### ⚠️ Important notes when authoring recipes
#### Mark “do‑not‑delete” areas
```html
<!-- 
  ⚠️⚠️⚠️ WARNING: The Pyodide and [Library Name] install block below must NOT be deleted ⚠️⚠️⚠️
  Deleting this block makes file generation impossible.
-->
```
- Emphasize visually with 🔴🔴🔴
- Add a warning in an HTML comment

#### Prevent indentation errors
```html
<!-- 
  AI will overwrite here: code for [Library Name]
  Important: Start Python code at the leftmost column (no indentation)
-->
<script type="text/python" id="python-code">
from [Library] import [Class]  # ← start at column 0
import base64  # ← no indentation
```

#### Set the correct MIME type
| File format | MIME type |
|---|---|
| CSV | text/csv |
| JSON | application/json |
| XML | application/xml |
| HTML | text/html |
| Markdown | text/markdown |
| YAML | application/x-yaml |
| SQLite | application/x-sqlite3 |

#### Implement error handling
```javascript
if (!base64Data) {
    throw new Error("Python code did not produce any data.");
}
```

### 🧪 Testing a recipe
1. **Basic functionality**
   - Verify with minimal content
   - Confirm the download starts correctly
   - Ensure the generated file opens correctly

2. **Error cases**
   - Verify errors when library installs are removed
   - Confirm indentation errors are detected
   - Confirm error when Base64 variable is missing

3. **Compatibility**
   - Major browsers (Chrome, Firefox, Edge, Safari)
   - Check with the target applications that open the generated files

### 📋 Checklist for adding a new recipe
- [ ] Library is available on Pyodide
- [ ] MIME type is correctly set
- [ ] All required sections are present
- [ ] “Do‑not‑delete” markers are placed appropriately
- [ ] Indentation rules are clearly explained
- [ ] Base64 variable name is unique and clear
- [ ] Examples are practical and easy to follow
- [ ] Error handling guidance is comprehensive
- [ ] Version number starts at v1.0
- [ ] MIT license notice is included

### 🔄 Updating an existing recipe
1. **Versioning**
   - Minor: v1.0 → v1.1 (bug fixes, small improvements)
   - Major: v1.x → v2.0 (significant feature additions)

2. **Record the changelog**
   ```markdown
   ## **📝 Changelog**
   - **v1.1** (date): Summary of changes
     - Specific change 1
     - Specific change 2
   - **v1.0** (date): Initial release
   ```

3. **Maintain backward compatibility**
   - Do not remove existing mandatory elements
   - Add new features as additions
   - Avoid breaking changes

### 💡 Best Practices for Writing Recipes
1. **Keep it simple**
   - Focus on the minimum mandatory requirements
   - Provide advanced features in the Examples section

2. **Use visual emphasis**
   - Use 🔴 to highlight critical items
   - Use ⚠️ to call out pitfalls
   - Use 📋 icons to clarify sections

3. **Provide practical examples**
   - Business use cases
   - Personal‑use scenarios
   - Examples with increasing complexity

4. **Make error messages informative**
   - Explain the cause clearly
   - Provide concrete steps to resolve
   - Prevent common mistakes proactively

## Authoring‑Time Checklist
- [ ] The recipe file is attached
- [ ] Library installation steps are present
- [ ] Python code starts at column 0
- [ ] Base64 hand‑off to JavaScript is present
- [ ] File name and MIME type are appropriate
- [ ] Error handling is implemented
