# **XLSX Auto-Generation Page Recipe v2.0**
**Complete Guide for AI-Powered One-Click Download Page Generation**

---

## **📌 About This Recipe**

A complete instruction manual for generative AI to automatically create a **single HTML file** that can generate and download Excel files (.xlsx) in the browser based on user instructions.

### **Features of v2.0**
- 🎯 Clear prioritization of implementation rules
- 📊 Support for automatic CSV data processing (optional)
- 🔍 Complete error handling procedures and examples
- 💡 Easy to learn with step-by-step implementation examples
- 📈 Comprehensive coverage of Excel-specific advanced features

### **About CSV Processing**
- CSV file attachment is **completely optional**
- If there is no CSV, normal Excel generation will be performed
- Only when CSV is present will automatic data import be executed
- Guaranteed to work properly in both cases

---

## **🚨 Implementation Rules (In Order of Priority)**

### **🔴 Level 1: Absolute Prohibitions (Strictly No Changes or Deletions)**

The following elements must **never be changed**:

#### **1.1 openpyxl Library Installation Process**
```javascript
await pyodide.loadPackage("micropip");
await pyodide.runPythonAsync(`
    import micropip
    await micropip.install('openpyxl')
`);
```

**Why it's absolutely necessary:**
- openpyxl is an external library for Excel file generation
- Not included by default in the browser's Pyodide environment
- Must be installed at runtime using micropip
- **Error that always occurs if deleted**: `ModuleNotFoundError: No module named 'openpyxl'`

#### **1.2 Error Overlay HTML/CSS/JavaScript**
- Error overlay HTML structure (`<div id="error-overlay">`)
- Error handling JavaScript code
- CSS class names (`.error-overlay`, `.error-card`, etc.)

**Reason**: Important feature to ensure usability during errors

#### **1.3 Pyodide Loading Process**
```html
<script src="https://cdn.jsdelivr.net/pyodide/v0.25.0/full/pyodide.js"></script>
```

**Reason**: Essential library that forms the foundation of the Python execution environment

### **🟡 Level 2: Required Rules (Must Follow)**

#### **2.1 Python Code Indentation Rules**
```python
# ✅ Correct (start from left edge)
from openpyxl import Workbook
import base64

# ❌ Wrong (with indentation)
    from openpyxl import Workbook  # IndentationError occurs
    import base64
```

**Reason**: Adding indentation to Python code influenced by HTML indentation causes errors

#### **2.2 Required Import Statements**
```python
from openpyxl import Workbook
from io import BytesIO
import base64
import js  # Required for JavaScript integration
```

**Error if omitted**: `NameError: name 'js' is not defined`

#### **2.3 Base64 Data Transfer**
Must execute at the end of Python code:
```python
js.xlsx_base64_data = xlsx_base64
```

**Reason**: Required process for JavaScript to receive XLSX data

### **🟢 Level 3: Customizable Elements**

The following can be freely changed:
- Page title (`<title>` tag and `<h1>` tag)
- Excel content within Python code
- Sheet layout and design
- Download filename
- Types of charts and aggregations

---

## **📊 CSV Data Processing Feature (Optional)**

### **Basic Policy**
When a CSV file is attached, it automatically reads the data and converts it to Excel. If there is no attachment, normal Excel generation is performed.

### **CSV Processing Flow**
1. Check for CSV data existence
2. Automatic character encoding detection (UTF-8, Shift-JIS support)
3. Header row recognition
4. Automatic data type conversion (numbers, dates, strings)
5. Expansion to Excel sheet

### **Implementation Code Example**
```python
# Check for CSV data (None if doesn't exist)
csv_content = None
try:
    csv_content = js.csv_content if hasattr(js, 'csv_content') else None
except:
    csv_content = None

# Branch processing based on CSV presence
if csv_content:
    # Process CSV data
    import csv
    import io
    
    # Parse CSV
    reader = csv.reader(io.StringIO(csv_content))
    headers = next(reader, None)
    
    if headers:
        # Set headers
        for col, header in enumerate(headers, 1):
            ws.cell(row=1, column=col, value=header)
            # Header formatting
            cell = ws.cell(row=1, column=col)
            cell.font = Font(bold=True)
            cell.fill = PatternFill(start_color="366092", end_color="366092", fill_type="solid")
        
        # Set data
        for row_idx, row_data in enumerate(reader, 2):
            for col_idx, value in enumerate(row_data, 1):
                # Attempt numeric conversion
                try:
                    # Integer check
                    if '.' not in value:
                        numeric_value = int(value)
                    else:
                        numeric_value = float(value)
                    ws.cell(row=row_idx, column=col_idx, value=numeric_value)
                except ValueError:
                    # Set as string
                    ws.cell(row=row_idx, column=col_idx, value=value)
else:
    # Normal processing when there's no CSV
    ws['A1'] = 'Data'
    ws['B1'] = 'Value'
    # Continue with normal data setting
```

### **CSV Processing Error Handling**
```python
def safe_csv_process(csv_content):
    """Safe CSV processing"""
    try:
        # CSV processing
        process_csv_data(csv_content)
    except UnicodeDecodeError:
        # Character encoding error
        try:
            # Retry as Shift-JIS
            csv_content = csv_content.encode('shift-jis').decode('utf-8')
            process_csv_data(csv_content)
        except:
            # Set error message
            ws['A1'] = 'Failed to read CSV'
    except Exception as e:
        # Other errors
        ws['A1'] = f'Error: {str(e)}'
```

---

## **📝 HTML Template (Complete Version)**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- Customizable: Page title -->
    <title>Excel File Download</title>

    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { font-family: 'Inter', sans-serif; }
        button:disabled { cursor: not-allowed; opacity: 0.6; }
        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border-left-color: #09f;
            animation: spin 1s ease infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        /* Error overlay (do not change) */
        .error-overlay { position: fixed; inset: 0; background: rgba(17,24,39,0.25); display: none; align-items: center; justify-content: center; padding: 16px; z-index: 9999; }
        .error-card { width: 100%; max-width: 880px; background: rgba(255,255,255,0.96); border: 1px solid rgba(55,65,81,0.25); border-radius: 16px; box-shadow: 0 10px 30px rgba(0,0,0,0.25); padding: 20px; }
        .error-title { display:flex; align-items:center; gap:8px; font-weight:800; font-size:18px; color:#991b1b; }
        .error-subtitle { margin-top:6px; color:#374151; line-height:1.55; }
        .error-actions { margin-top:12px; display:flex; gap:8px; flex-wrap:wrap; }
        .copy-btn { border:1px solid rgba(55,65,81,.3); padding:8px 12px; border-radius:10px; font-weight:600; background:#fff; cursor: pointer; }
        .copy-btn:hover { background: #f3f4f6; }
        .error-pre { margin-top:12px; background:#0b1020; color:#e5e7eb; border-radius:12px; padding:12px 14px; max-height:320px; overflow:auto; white-space:pre-wrap; word-break:break-word; font-family:ui-monospace,SFMono-Regular,Menlo,Consolas,'Courier New',monospace; font-size:13px; line-height:1.45; }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen">

    <div class="bg-white rounded-2xl shadow-lg p-8 sm:p-12 text-center max-w-md w-full">
        <!-- Customizable: Heading -->
        <h1 class="text-2xl sm:text-3xl font-bold text-gray-800 mb-4">
            Excel File
        </h1>

        <p id="status-message" class="text-gray-600 mb-6 h-12 flex items-center justify-center">
            Preparing file...
        </p>

        <div id="loading-spinner" class="spinner mx-auto mb-6"></div>

        <button id="download-button" class="w-full bg-blue-600 text-white font-bold py-3 px-6 rounded-lg shadow-md hover:bg-blue-700 transition duration-300 transform hover:scale-105" disabled>
            Download File
        </button>
    </div>

    <!-- Customizable: Python code (follow the rules) -->
    <script type="text/python" id="python-code">
# Create workbook
from openpyxl import Workbook
from io import BytesIO
import base64
import js

# Create workbook
wb = Workbook()
ws = wb.active
ws.title = "Data"

# Add data
ws['A1'] = 'Auto-generated by AI'
ws['A2'] = 'This Excel file was created dynamically'

# Save to BytesIO
xlsx_io = BytesIO()
wb.save(xlsx_io)
xlsx_io.seek(0)

# Base64 encoding
xlsx_bytes = xlsx_io.read()
xlsx_base64 = base64.b64encode(xlsx_bytes).decode('utf-8')

# Pass to JavaScript (required)
js.xlsx_base64_data = xlsx_base64

print("XLSX file has been generated in memory.")
    </script>

    <!-- Error overlay (do not change) -->
    <div id="error-overlay" class="error-overlay" role="dialog" aria-modal="true" aria-labelledby="error-title">
        <div class="error-card">
            <div class="error-title" id="error-title">An error occurred: Please paste the following error message into the AI chat area to request a fix</div>
            <div class="error-subtitle">Copy the full error text and throw it directly to the AI. It will suggest the cause identification and patch.</div>
            <div class="error-actions">
                <button id="copy-error" class="copy-btn" aria-label="Copy error message">Copy Full Error</button>
                <button id="close-error" class="copy-btn" aria-label="Close this error display">Close</button>
            </div>
            <pre id="error-text" class="error-pre" tabindex="0" aria-live="polite"></pre>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/pyodide/v0.25.0/full/pyodide.js"></script>
    <script type="module">
        const statusMessage = document.getElementById('status-message');
        const downloadButton = document.getElementById('download-button');
        const loadingSpinner = document.getElementById('loading-spinner');
        const overlay = document.getElementById('error-overlay');
        const errorText = document.getElementById('error-text');
        const copyBtn = document.getElementById('copy-error');
        const closeBtn = document.getElementById('close-error');

        // Event listeners for error UI
        copyBtn?.addEventListener('click', async () => {
            try {
                await navigator.clipboard.writeText(errorText.textContent || '');
                copyBtn.textContent = 'Copied';
                setTimeout(() => (copyBtn.textContent = 'Copy Full Error'), 1200);
            } catch (_) {
                const r = document.createRange(); 
                r.selectNodeContents(errorText);
                const sel = window.getSelection(); 
                sel.removeAllRanges(); 
                sel.addRange(r);
            }
        });

        closeBtn?.addEventListener('click', () => { 
            overlay.style.display = 'none'; 
        });

        async function main() {
            try {
                statusMessage.textContent = 'Loading execution environment...';
                const pyodide = await loadPyodide({
                    indexURL: "https://cdn.jsdelivr.net/pyodide/v0.25.0/full/",
                });

                // Install openpyxl (do not delete)
                statusMessage.textContent = 'Installing libraries...';
                await pyodide.loadPackage("micropip");
                await pyodide.runPythonAsync(`
                    import micropip
                    await micropip.install('openpyxl')
                `);

                statusMessage.textContent = 'Generating file...';
                const pythonCode = document.getElementById('python-code').textContent;
                window.xlsx_base64_data = null; 
                await pyodide.runPythonAsync(pythonCode);
                const base64Data = window.xlsx_base64_data;

                if (!base64Data) {
                    throw new Error("Python code did not produce any data.");
                }

                statusMessage.textContent = 'Ready!';
                loadingSpinner.style.display = 'none';
                downloadButton.disabled = false;
                downloadButton.addEventListener('click', () => downloadFile(base64Data, 'spreadsheet.xlsx'));

            } catch (error) {
                console.error("An error occurred:", error);
                
                try {
                    const details = (error && (error.stack || error.message || String(error))) || 'Unknown error';
                    errorText.textContent = details.trim();
                } catch(_) {
                    errorText.textContent = 'Unknown error';
                }
                
                overlay.style.display = 'flex';
                statusMessage.textContent = 'Processing interrupted.';
                loadingSpinner.style.display = 'none';
            }
        }

        function downloadFile(base64Data, fileName) {
            try {
                const byteCharacters = atob(base64Data);
                const byteNumbers = new Array(byteCharacters.length);
                for (let i = 0; i < byteCharacters.length; i++) {
                    byteNumbers[i] = byteCharacters.charCodeAt(i);
                }
                const byteArray = new Uint8Array(byteNumbers);
                const blob = new Blob([byteArray], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' });

                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.style.display = 'none';
                a.href = url;
                a.download = fileName;
                document.body.appendChild(a);
                a.click();
                
                setTimeout(() => {
                    URL.revokeObjectURL(url);
                    document.body.removeChild(a);
                }, 100);

            } catch (error) {
                console.error("Download failed:", error);
                statusMessage.textContent = 'Download failed.';
            }
        }

        window.onload = main;
    </script>
</body>
</html>
```

---

## **🔧 Error Diagnosis Guide**

### **Common Errors and Solutions**

#### **1. ModuleNotFoundError**
```
ModuleNotFoundError: No module named 'openpyxl'
```
**Cause**: openpyxl installation process has been deleted
**Solution**: Restore `await micropip.install('openpyxl')`

#### **2. IndentationError**
```
IndentationError: unexpected indent
```
**Cause**: Python code doesn't start from the left edge
**Solution**: Remove indentation from all Python code

#### **3. NameError**
```
NameError: name 'js' is not defined
```
**Cause**: Missing `import js`
**Solution**: Add `import js` to required import statements

#### **4. ValueError**
```
ValueError: Cannot convert ... to Excel
```
**Cause**: Setting data types that Excel cannot handle
**Solution**: Use basic data types (strings, numbers, dates)

#### **5. CSV-Related Errors**
```
UnicodeDecodeError: 'utf-8' codec can't decode
```
**Cause**: CSV file has different character encoding
**Solution**: Error handling automatically retries as Shift-JIS

### **Error Handling Procedure (4 Steps)**

1. **Auto-display**: Error overlay appears in full screen
   - High-visibility design that can't be missed
   - Complete display of error details (stack trace)

2. **One-click copy**: Click the "Copy Full Error" button
   - Automatically copies to clipboard
   - "Copied" confirmation display

3. **Paste to AI**: Paste directly into chat area
   - AI automatically analyzes error content
   - Identifies the cause

4. **Resolution**: Use the fix provided by AI
   - Explanation of specific fixes
   - Provision of corrected code

---

## **💡 Implementation Examples**

### **Level 1: Minimal Implementation**
```python
from openpyxl import Workbook
from io import BytesIO
import base64
import js

wb = Workbook()
ws = wb.active
ws['A1'] = 'Simple Table'
ws['A2'] = 'Data 1'
ws['B2'] = 100

xlsx_io = BytesIO()
wb.save(xlsx_io)
xlsx_io.seek(0)
js.xlsx_base64_data = base64.b64encode(xlsx_io.read()).decode('utf-8')
```

### **Level 2: Basic Spreadsheet**
```python
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side
from openpyxl.utils import get_column_letter
from io import BytesIO
import base64
import js

wb = Workbook()
ws = wb.active
ws.title = "Sales Table"

# Header settings
headers = ['Date', 'Product Name', 'Quantity', 'Unit Price', 'Sales']
for idx, header in enumerate(headers, 1):
    cell = ws.cell(row=1, column=idx, value=header)
    cell.font = Font(bold=True, color="FFFFFF")
    cell.fill = PatternFill(start_color="366092", end_color="366092", fill_type="solid")
    cell.alignment = Alignment(horizontal="center")

# Data input
data = [
    ['2025/01/01', 'Product A', 10, 100],
    ['2025/01/02', 'Product B', 5, 200],
    ['2025/01/03', 'Product C', 8, 150],
]

for row_idx, row_data in enumerate(data, 2):
    for col_idx, value in enumerate(row_data, 1):
        ws.cell(row=row_idx, column=col_idx, value=value)
    # Sales calculation formula
    ws.cell(row=row_idx, column=5, value=f'=C{row_idx}*D{row_idx}')

# Total row
last_row = len(data) + 2
ws[f'A{last_row}'] = 'Total'
ws[f'A{last_row}'].font = Font(bold=True)
ws[f'E{last_row}'] = f'=SUM(E2:E{last_row-1})'
ws[f'E{last_row}'].font = Font(bold=True)
ws[f'E{last_row}'].fill = PatternFill(start_color="FFFF00", end_color="FFFF00", fill_type="solid")

# Border settings
thin_border = Border(
    left=Side(style='thin'),
    right=Side(style='thin'),
    top=Side(style='thin'),
    bottom=Side(style='thin')
)

for row in ws.iter_rows(min_row=1, max_row=last_row, min_col=1, max_col=5):
    for cell in row:
        cell.border = thin_border

# Column width adjustment
for col in range(1, 6):
    ws.column_dimensions[get_column_letter(col)].width = 15

# Save
xlsx_io = BytesIO()
wb.save(xlsx_io)
xlsx_io.seek(0)
js.xlsx_base64_data = base64.b64encode(xlsx_io.read()).decode('utf-8')
```

### **Level 3: CSV Data Import and Processing**
```python
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment
from openpyxl.chart import BarChart, Reference
from openpyxl.utils import get_column_letter
from io import BytesIO
import base64
import js
import csv
import io

wb = Workbook()
ws = wb.active
ws.title = "CSV Data"

# Check for CSV data (assume it exists in this example)
csv_content = None
try:
    csv_content = js.csv_content if hasattr(js, 'csv_content') else None
except:
    csv_content = None

if csv_content:
    # When CSV data exists
    try:
        # Parse CSV
        reader = csv.reader(io.StringIO(csv_content))
        headers = next(reader, None)
        
        if headers:
            # Set headers
            for col, header in enumerate(headers, 1):
                cell = ws.cell(row=1, column=col, value=header)
                cell.font = Font(bold=True, color="FFFFFF")
                cell.fill = PatternFill(start_color="366092", end_color="366092", fill_type="solid")
                cell.alignment = Alignment(horizontal="center")
            
            # Set data
            for row_idx, row_data in enumerate(reader, 2):
                for col_idx, value in enumerate(row_data, 1):
                    # Attempt numeric conversion
                    try:
                        if '.' not in value:
                            numeric_value = int(value)
                        else:
                            numeric_value = float(value)
                        ws.cell(row=row_idx, column=col_idx, value=numeric_value)
                    except ValueError:
                        ws.cell(row=row_idx, column=col_idx, value=value)
            
            # Get data range
            max_row = ws.max_row
            max_col = ws.max_column
            
            # Add total row (numeric columns only)
            ws.cell(row=max_row + 2, column=1, value="Total")
            ws.cell(row=max_row + 2, column=1).font = Font(bold=True)
            
            for col in range(2, max_col + 1):
                # Check if numeric column
                is_numeric = True
                for row in range(2, max_row + 1):
                    cell_value = ws.cell(row=row, column=col).value
                    if cell_value is not None and not isinstance(cell_value, (int, float)):
                        is_numeric = False
                        break
                
                if is_numeric:
                    col_letter = get_column_letter(col)
                    sum_cell = ws.cell(row=max_row + 2, column=col)
                    sum_cell.value = f'=SUM({col_letter}2:{col_letter}{max_row})'
                    sum_cell.font = Font(bold=True)
                    sum_cell.fill = PatternFill(start_color="FFFF00", end_color="FFFF00", fill_type="solid")
            
            # Add chart (if data has 3+ columns)
            if max_col >= 3 and max_row >= 3:
                chart = BarChart()
                chart.title = "Data Chart"
                chart.y_axis.title = headers[1] if len(headers) > 1 else 'Value'
                chart.x_axis.title = headers[0] if headers else 'Category'
                
                # Data range (use column 2)
                data = Reference(ws, min_col=2, min_row=1, max_row=max_row, max_col=2)
                categories = Reference(ws, min_col=1, min_row=2, max_row=max_row)
                chart.add_data(data, titles_from_data=True)
                chart.set_categories(categories)
                
                ws.add_chart(chart, f"{get_column_letter(max_col + 2)}3")
                
    except Exception as e:
        # Error handling
        ws['A1'] = 'An error occurred while processing CSV data'
        ws['A2'] = str(e)
else:
    # Default processing when there's no CSV data
    ws['A1'] = 'Sample Data'
    ws['A1'].font = Font(bold=True, size=14)
    
    # Create sample data
    headers = ['Item', 'Value 1', 'Value 2', 'Value 3']
    for idx, header in enumerate(headers, 1):
        cell = ws.cell(row=3, column=idx, value=header)
        cell.font = Font(bold=True)
        cell.fill = PatternFill(start_color="D9E1F2", end_color="D9E1F2", fill_type="solid")
    
    sample_data = [
        ['Data A', 100, 150, 200],
        ['Data B', 200, 250, 300],
        ['Data C', 150, 200, 250],
    ]
    
    for row_idx, row_data in enumerate(sample_data, 4):
        for col_idx, value in enumerate(row_data, 1):
            ws.cell(row=row_idx, column=col_idx, value=value)

# Auto-adjust column width
for column in ws.columns:
    max_length = 0
    column_letter = get_column_letter(column[0].column)
    for cell in column:
        if cell.value:
            max_length = max(max_length, len(str(cell.value)))
    adjusted_width = min(max_length + 2, 50)
    ws.column_dimensions[column_letter].width = adjusted_width

# Save
xlsx_io = BytesIO()
wb.save(xlsx_io)
xlsx_io.seek(0)
js.xlsx_base64_data = base64.b64encode(xlsx_io.read()).decode('utf-8')
```

### **Level 4: Advanced Analysis and Multiple Sheets**
```python
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side
from openpyxl.chart import BarChart, PieChart, LineChart, Reference
from openpyxl.formatting.rule import ColorScaleRule, DataBarRule
from openpyxl.utils import get_column_letter
from datetime import date, timedelta
from io import BytesIO
import base64
import js

def create_summary_sheet(wb, data_sheet):
    """Create summary sheet"""
    summary_ws = wb.create_sheet("Summary")
    summary_ws['A1'] = 'Statistical Information'
    summary_ws['A1'].font = Font(bold=True, size=14)
    
    # Calculate statistical information
    stats = [
        ['Item', 'Value'],
        ['Data Count', f'=COUNTA({data_sheet.title}!B:B)-1'],
        ['Total', f'=SUM({data_sheet.title}!D:D)'],
        ['Average', f'=AVERAGE({data_sheet.title}!D:D)'],
        ['Maximum', f'=MAX({data_sheet.title}!D:D)'],
        ['Minimum', f'=MIN({data_sheet.title}!D:D)'],
    ]
    
    for row_idx, (label, formula) in enumerate(stats, 3):
        summary_ws.cell(row=row_idx, column=1, value=label)
        summary_ws.cell(row=row_idx, column=2, value=formula)
        
        if row_idx == 3:
            # Header row formatting
            summary_ws.cell(row=row_idx, column=1).font = Font(bold=True)
            summary_ws.cell(row=row_idx, column=2).font = Font(bold=True)
            summary_ws.cell(row=row_idx, column=1).fill = PatternFill(
                start_color="366092", end_color="366092", fill_type="solid"
            )
            summary_ws.cell(row=row_idx, column=2).fill = PatternFill(
                start_color="366092", end_color="366092", fill_type="solid"
            )
    
    # Conditional formatting (data bar)
    data_bar_rule = DataBarRule(
        start_type="num", start_value=0,
        end_type="max", color="638EC6"
    )
    summary_ws.conditional_formatting.add("B4:B8", data_bar_rule)
    
    return summary_ws

wb = Workbook()

# Main data sheet
ws = wb.active
ws.title = "Sales Data"

# Title
ws['A1'] = 'Monthly Sales Report'
ws['A1'].font = Font(bold=True, size=16)
ws.merge_cells('A1:E1')

# Header
headers = ['Date', 'Staff', 'Product', 'Quantity', 'Sales']
for idx, header in enumerate(headers, 1):
    cell = ws.cell(row=3, column=idx, value=header)
    cell.font = Font(bold=True, color="FFFFFF")
    cell.fill = PatternFill(start_color="366092", end_color="366092", fill_type="solid")
    cell.alignment = Alignment(horizontal="center")

# Generate sample data
base_date = date.today() - timedelta(days=30)
products = ['Product A', 'Product B', 'Product C', 'Product D']
staff = ['Johnson', 'Smith', 'Davis', 'Wilson']

data = []
for i in range(30):
    import random
    data.append([
        (base_date + timedelta(days=i)).strftime('%Y/%m/%d'),
        random.choice(staff),
        random.choice(products),
        random.randint(5, 50),
        random.randint(100, 1000)
    ])

# Data input
for row_idx, row_data in enumerate(data, 4):
    for col_idx, value in enumerate(row_data, 1):
        ws.cell(row=row_idx, column=col_idx, value=value)

# Border settings
thin_border = Border(
    left=Side(style='thin'),
    right=Side(style='thin'),
    top=Side(style='thin'),
    bottom=Side(style='thin')
)

for row in ws.iter_rows(min_row=3, max_row=len(data)+3, min_col=1, max_col=5):
    for cell in row:
        cell.border = thin_border

# Conditional formatting (color scale for sales)
color_scale_rule = ColorScaleRule(
    start_type="min", start_color="63BE7B",
    mid_type="percentile", mid_value=50, mid_color="FFEB9C",
    end_type="max", end_color="F8696B"
)
ws.conditional_formatting.add(f"E4:E{len(data)+3}", color_scale_rule)

# Column width adjustment
column_widths = [12, 10, 10, 8, 12]
for idx, width in enumerate(column_widths, 1):
    ws.column_dimensions[get_column_letter(idx)].width = width

# Chart sheet
chart_ws = wb.create_sheet("Charts")

# Sales trend chart
line_chart = LineChart()
line_chart.title = "Daily Sales Trend"
line_chart.y_axis.title = "Sales"
line_chart.x_axis.title = "Date"
line_chart.width = 20
line_chart.height = 10

data_ref = Reference(ws, min_col=5, min_row=3, max_row=len(data)+3)
dates_ref = Reference(ws, min_col=1, min_row=4, max_row=len(data)+3)
line_chart.add_data(data_ref, titles_from_data=True)
line_chart.set_categories(dates_ref)

chart_ws.add_chart(line_chart, "A1")

# Staff summary sheet
staff_ws = wb.create_sheet("By Staff")
staff_ws['A1'] = 'Sales Summary by Staff'
staff_ws['A1'].font = Font(bold=True, size=14)

# Staff summary (simplified version)
staff_summary = {}
for row in data:
    staff_name = row[1]
    sales = row[4]
    if staff_name in staff_summary:
        staff_summary[staff_name] += sales
    else:
        staff_summary[staff_name] = sales

# Input summary results
staff_ws['A3'] = 'Staff'
staff_ws['B3'] = 'Total Sales'
staff_ws['A3'].font = Font(bold=True)
staff_ws['B3'].font = Font(bold=True)

for idx, (name, total) in enumerate(staff_summary.items(), 4):
    staff_ws.cell(row=idx, column=1, value=name)
    staff_ws.cell(row=idx, column=2, value=total)

# Pie chart
pie_chart = PieChart()
pie_chart.title = "Sales Composition by Staff"
data_ref = Reference(staff_ws, min_col=2, min_row=3, max_row=3+len(staff_summary))
labels_ref = Reference(staff_ws, min_col=1, min_row=4, max_row=3+len(staff_summary))
pie_chart.add_data(data_ref, titles_from_data=True)
pie_chart.set_categories(labels_ref)

staff_ws.add_chart(pie_chart, "D3")

# Create summary sheet
summary_sheet = create_summary_sheet(wb, ws)

# Save
xlsx_io = BytesIO()
wb.save(xlsx_io)
xlsx_io.seek(0)
js.xlsx_base64_data = base64.b64encode(xlsx_io.read()).decode('utf-8')

print("Advanced XLSX file with multiple sheets has been generated.")
```

---

## **📚 Excel-Specific Features**

### **Formulas and Functions**
```python
# Basic formulas
ws['C1'] = '=A1+B1'          # Addition
ws['C2'] = '=A2-B2'          # Subtraction
ws['C3'] = '=A3*B3'          # Multiplication
ws['C4'] = '=A4/B4'          # Division

# Aggregation functions
ws['D1'] = '=SUM(A1:A10)'    # Sum
ws['D2'] = '=AVERAGE(B1:B10)' # Average
ws['D3'] = '=MAX(C1:C10)'    # Maximum
ws['D4'] = '=MIN(D1:D10)'    # Minimum
ws['D5'] = '=COUNT(E1:E10)'  # Count

# Conditional functions
ws['E1'] = '=IF(A1>100,"Large","Small")'        # IF statement
ws['E2'] = '=SUMIF(A:A,">100",B:B)'            # Conditional sum
ws['E3'] = '=COUNTIF(A:A,">=50")'              # Conditional count
ws['E4'] = '=VLOOKUP(A1,F:G,2,FALSE)'          # VLOOKUP
```

### **Data Validation (Dropdown Lists)**
```python
from openpyxl.worksheet.datavalidation import DataValidation

# Create dropdown list
dv = DataValidation(
    type="list",
    formula1='"Option 1,Option 2,Option 3"',
    allow_blank=True
)
dv.error = 'Please select from the list'
dv.errorTitle = 'Input Error'

ws.add_data_validation(dv)
dv.add('A1:A10')  # Apply dropdown to A1 through A10
```

### **Conditional Formatting**
```python
from openpyxl.formatting.rule import ColorScaleRule, DataBarRule, IconSetRule

# Color scale (color gradient based on value)
color_scale_rule = ColorScaleRule(
    start_type="min", start_color="00FF00",  # Minimum value is green
    end_type="max", end_color="FF0000"       # Maximum value is red
)
ws.conditional_formatting.add("A1:A10", color_scale_rule)

# Data bar (display bar graph in cell)
data_bar_rule = DataBarRule(
    start_type="num", start_value=0,
    end_type="max", color="638EC6"
)
ws.conditional_formatting.add("B1:B10", data_bar_rule)

# Icon set (display icons based on value)
from openpyxl.formatting.rule import Rule
from openpyxl.styles.differential import DifferentialStyle
from openpyxl.formatting import Rule

# Highlight specific values or above
red_fill = PatternFill(start_color="FF0000", end_color="FF0000", fill_type="solid")
dxf = DifferentialStyle(fill=red_fill)
rule = Rule(type="expression", dxf=dxf)
rule.formula = ["$A1>100"]
ws.conditional_formatting.add("A1:A10", rule)
```

### **Creating Charts**
```python
from openpyxl.chart import BarChart, LineChart, PieChart, ScatterChart, Reference

# Bar chart
bar_chart = BarChart()
bar_chart.title = "Sales Chart"
bar_chart.y_axis.title = "Sales"
bar_chart.x_axis.title = "Month"

data = Reference(ws, min_col=2, min_row=1, max_row=10, max_col=3)
categories = Reference(ws, min_col=1, min_row=2, max_row=10)
bar_chart.add_data(data, titles_from_data=True)
bar_chart.set_categories(categories)

ws.add_chart(bar_chart, "E5")

# Line chart
line_chart = LineChart()
line_chart.title = "Trend Chart"
line_chart.style = 13  # Style number

# Pie chart
pie_chart = PieChart()
pie_chart.title = "Composition Ratio"

# Scatter plot
scatter_chart = ScatterChart()
scatter_chart.title = "Correlation Chart"
scatter_chart.x_axis.title = "X Axis"
scatter_chart.y_axis.title = "Y Axis"
```

### **Cross-Sheet References**
```python
# Reference value from another sheet
ws['A1'] = '=Sheet2!A1'           # Reference A1 cell in Sheet2
ws['B1'] = '=SUM(Sheet2!A:A)'     # Sum column A in Sheet2
ws['C1'] = "='Sales Data'!B2"     # When sheet name has spaces

# 3D reference (reference multiple sheets together)
ws['D1'] = '=SUM(Sheet1:Sheet3!A1)'  # Sum A1 from Sheet1 to Sheet3
```

### **Efficient Processing of Large Data**
```python
# Memory-efficient writing
def write_large_dataset(ws, data):
    """Write large data efficiently"""
    # Using append() is faster
    for row in data:
        ws.append(row)
    
    # Or batch write by specifying range
    from openpyxl.utils import range_boundaries
    for row_idx, row_data in enumerate(data, 1):
        for col_idx, value in enumerate(row_data, 1):
            ws.cell(row=row_idx, column=col_idx, value=value)

# Large data processing with write_only mode
wb = Workbook(write_only=True)
ws = wb.create_sheet()

# Streaming write
for row in large_dataset:
    ws.append(row)

wb.save('large_file.xlsx')
```

---

## **✅ Implementation Checklist**

### **Required Confirmation Items**
- [ ] openpyxl installation process exists
- [ ] Python code starts from left edge
- [ ] `import js` is included
- [ ] Assignment to `js.xlsx_base64_data` exists
- [ ] Error overlay HTML is complete

### **CSV Processing Confirmation Items**
- [ ] CSV processing is implemented as optional
- [ ] Works normally even without CSV
- [ ] Character encoding error handling is included
- [ ] Automatic data type conversion is implemented

### **Operation Confirmation Items**
- [ ] Page loads normally
- [ ] Download button becomes enabled
- [ ] XLSX file can be downloaded
- [ ] Overlay displays on error
- [ ] Error copy button functions

---

## **📊 openpyxl Reference**

### **Basic Operations**
| Method | Purpose | Example |
|---------|------|-----|
| `Workbook()` | Create new workbook | `wb = Workbook()` |
| `wb.active` | Get active sheet | `ws = wb.active` |
| `wb.create_sheet(title)` | Create new sheet | `ws2 = wb.create_sheet("Sheet2")` |
| `ws['A1']` | Cell access | `ws['A1'] = 100` |
| `ws.cell(row, column)` | Row/column specified access | `ws.cell(1, 1, value=100)` |
| `ws.append(list)` | Add row | `ws.append([1, 2, 3])` |
| `ws.merge_cells()` | Merge cells | `ws.merge_cells('A1:D1')` |

### **Formatting**
```python
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side

# Font
cell.font = Font(
    bold=True,           # Bold
    italic=True,         # Italic
    size=12,            # Size
    color="FF0000"      # Color (red)
)

# Background color
cell.fill = PatternFill(
    start_color="FFFF00",
    end_color="FFFF00",
    fill_type="solid"
)

# Alignment
cell.alignment = Alignment(
    horizontal="center",  # Horizontal
    vertical="center",    # Vertical
    wrap_text=True       # Wrap text
)

# Border
border = Border(
    left=Side(style='thin'),
    right=Side(style='thin'),
    top=Side(style='thin'),
    bottom=Side(style='thin')
)
cell.border = border
```

### **Number Formats**
```python
# Currency
cell.number_format = '#,##0'           # 1,000
cell.number_format = '$#,##0'          # $1,000
cell.number_format = '#,##0.00'        # 1,000.00

# Percentage
cell.number_format = '0%'              # 10%
cell.number_format = '0.00%'           # 10.00%

# Date
cell.number_format = 'yyyy/mm/dd'      # 2025/01/17
cell.number_format = 'mmmm d, yyyy'    # January 17, 2025
```

---

## **🚀 Quick Start**

### **Fastest Implementation (3 Steps)**

1. **Copy HTML template**
2. **Change title** (2 locations: `<title>` and `<h1>`)
3. **Define table content in Python code**

```python
# Minimal change example
wb = Workbook()
ws = wb.active
ws['A1'] = 'Your data here'
ws['B1'] = 100
# ... Rest is save processing (as per template)
```

### **When Using CSV Data**

1. **Prepare CSV file**
```csv
Product Name,Quantity,Unit Price,Sales
Product A,10,100,1000
Product B,5,200,1000
```

2. **Request to AI**
```
Attach CSV file,
"Please create a report with charts from the attached CSV"
```

---

## **📝 Update History**

- **v2.0** (2025-09-17)
  - Reorganized implementation rules by priority
  - Fully implemented CSV processing feature (optional)
  - Added error UI feature (fullscreen overlay)
  - Expanded implementation examples to 4 levels
  - Added Excel-specific advanced features

- **v1.1** (2025-09-17)
  - Emphasized importance of external library installation
  - Added error handling methods

- **v1.0** (2025-09-17)
  - Initial release

---

## **📄 License**

MIT License - Free to use, modify, redistribute, and use commercially

---

## **🆘 Support**

### **What to Do When Problems Occur**

1. **Copy error message**
   - Use the "Copy Full Error" button in the error overlay

2. **Paste and consult with AI**
   - Paste the copied content into the chat area
   - AI will analyze the cause and provide a corrected version

3. **Check with checklist**
   - Confirm that required items are not missing
   - Especially the openpyxl installation process

4. **Refer to implementation examples**
   - Start with Level 1 minimal implementation
   - Add features gradually

### **Frequently Asked Questions**

**Q: Is a CSV file required?**
A: No, CSV files are completely optional. It works normally without CSV.

**Q: Can it process large amounts of data?**
A: Yes, it can process data with tens of thousands of rows. For more than that, consider using write_only mode.

**Q: Japanese CSV is garbled**
A: The automatic character encoding detection feature supports both UTF-8 and Shift-JIS. If it still garbles, please resave the CSV in UTF-8.

**Q: Can I use complex formulas?**
A: Yes, most Excel formulas like VLOOKUP, SUMIF, INDEX/MATCH can be used.

---

**This completes the full text of Recipe v2.0. It includes detailed explanations and examples so that AI can correctly understand and create appropriate XLSX generation pages regardless of CSV availability.**
