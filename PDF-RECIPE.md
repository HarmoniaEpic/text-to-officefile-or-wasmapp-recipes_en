# **PDF Auto-Generation Page Recipe v2.0**
**The Complete Guide to One-Click Download Page Generation by AI**

---

## **📌 About This Recipe**

A complete instruction manual for generative AI to automatically create a **single HTML file** that can generate and download PDF files in the browser based on user instructions.

### **Features of v2.0**
- 🎯 Clear implementation rules by priority
- 🔍 Complete error handling procedures and examples
- 💡 Easy to learn with step-by-step implementation examples
- 📑 5+ types of business document templates
- 🔒 Security features (password protection, etc.) support

---

## **🚨 Implementation Rules (By Priority)**

### **🔴 Level 1: Absolutely Prohibited (No Changes/Deletions Allowed)**

The following elements **must not be changed at all**:

#### **1.1 reportlab Library Installation Process**
```javascript
await pyodide.loadPackage("micropip");
await pyodide.runPythonAsync(`
    import micropip
    await micropip.install('reportlab')
`);
```

**Why absolutely necessary:**
- reportlab is an external library for PDF file generation
- Not included by default in browser Pyodide environment
- Must be installed at runtime using micropip
- **Error that always occurs if deleted**: `ModuleNotFoundError: No module named 'reportlab'`

#### **1.2 Error Overlay HTML/CSS/JavaScript**
- Error overlay HTML structure (`<div id="error-overlay">`)
- Error handling JavaScript code
- CSS class names (`.error-overlay`, `.error-card`, etc.)

**Reason**: Important functionality to ensure usability during errors

#### **1.3 Pyodide Loading Process**
```html
<script src="https://cdn.jsdelivr.net/pyodide/v0.25.0/full/pyodide.js"></script>
```

**Reason**: Essential library that serves as the foundation for Python execution environment

### **🟡 Level 2: Required Rules (Must Follow)**

#### **2.1 Python Code Indentation Rules**
```python
# ✅ Correct (starts from left edge)
from reportlab.pdfgen import canvas
import base64

# ❌ Wrong (with indentation)
    from reportlab.pdfgen import canvas  # IndentationError occurs
    import base64
```

**Reason**: Adding indentation to Python code influenced by HTML indentation causes errors

#### **2.2 Required Import Statements**
```python
from reportlab.lib.pagesizes import A4
from io import BytesIO
import base64
import js  # Essential for JavaScript integration
```

**Error if omitted**: `NameError: name 'js' is not defined`

#### **2.3 Base64 Data Transfer**
Must execute at the end of Python code:
```python
js.pdf_base64_data = pdf_base64
```

**Reason**: Essential process for JavaScript to receive PDF data

### **🟢 Level 3: Customizable Elements**

The following can be freely changed:
- Page title (`<title>` tag and `<h1>` tag)
- PDF content in Python code
- PDF layout and design
- Download filename
- Page size and margins

---

## **📝 HTML Template (Complete Version)**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- Customizable: Page Title -->
    <title>PDF Document Download</title>

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
        
        /* Error Overlay (Do Not Modify) */
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
            PDF Document
        </h1>

        <p id="status-message" class="text-gray-600 mb-6 h-12 flex items-center justify-center">
            Preparing file...
        </p>

        <div id="loading-spinner" class="spinner mx-auto mb-6"></div>

        <button id="download-button" class="w-full bg-blue-600 text-white font-bold py-3 px-6 rounded-lg shadow-md hover:bg-blue-700 transition duration-300 transform hover:scale-105" disabled>
            Download File
        </button>
    </div>

    <!-- Customizable: Python Code (Follow the Rules) -->
    <script type="text/python" id="python-code">
from reportlab.lib.pagesizes import A4
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer
from reportlab.lib.styles import getSampleStyleSheet
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.cidfonts import UnicodeCIDFont
from io import BytesIO
import base64
import js

# Create BytesIO object
pdf_buffer = BytesIO()

# Create PDF document
doc = SimpleDocTemplate(pdf_buffer, pagesize=A4)

# Register font for international characters
pdfmetrics.registerFont(UnicodeCIDFont('HeiseiKakuGo-W5'))

# Get and set styles
styles = getSampleStyleSheet()
styles['Normal'].fontName = 'HeiseiKakuGo-W5'
styles['Title'].fontName = 'HeiseiKakuGo-W5'

# Content
story = []
title = Paragraph("PDF Generated Automatically by AI", styles['Title'])
story.append(title)
story.append(Spacer(1, 12))

text = Paragraph("This PDF document was created dynamically.", styles['Normal'])
story.append(text)

# Build PDF
doc.build(story)

# Base64 encode
pdf_buffer.seek(0)
pdf_bytes = pdf_buffer.read()
pdf_base64 = base64.b64encode(pdf_bytes).decode('utf-8')

# Pass to JavaScript (Required)
js.pdf_base64_data = pdf_base64

print("PDF file has been generated in memory.")
    </script>

    <!-- Error Overlay (Do Not Modify) -->
    <div id="error-overlay" class="error-overlay" role="dialog" aria-modal="true" aria-labelledby="error-title">
        <div class="error-card">
            <div class="error-title" id="error-title">An error occurred: Please paste the following error message into the AI chat area to request a fix</div>
            <div class="error-subtitle">Copy the full error and throw it to AI as is. It will identify the cause and suggest a patch.</div>
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

        // Error UI event listeners
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

                // Install reportlab (Do Not Delete)
                statusMessage.textContent = 'Installing libraries...';
                await pyodide.loadPackage("micropip");
                await pyodide.runPythonAsync(`
                    import micropip
                    await micropip.install('reportlab')
                `);

                statusMessage.textContent = 'Generating file...';
                const pythonCode = document.getElementById('python-code').textContent;
                window.pdf_base64_data = null; 
                await pyodide.runPythonAsync(pythonCode);
                const base64Data = window.pdf_base64_data;

                if (!base64Data) {
                    throw new Error("Python code did not produce any data.");
                }

                statusMessage.textContent = 'Ready!';
                loadingSpinner.style.display = 'none';
                downloadButton.disabled = false;
                downloadButton.addEventListener('click', () => downloadFile(base64Data, 'document.pdf'));

            } catch (error) {
                console.error("An error occurred:", error);
                
                try {
                    const details = (error && (error.stack || error.message || String(error))) || 'Unknown error';
                    errorText.textContent = details.trim();
                } catch(_) {
                    errorText.textContent = 'Unknown error';
                }
                
                overlay.style.display = 'flex';
                statusMessage.textContent = 'Process interrupted.';
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
                const blob = new Blob([byteArray], { type: 'application/pdf' });

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
ModuleNotFoundError: No module named 'reportlab'
```
**Cause**: reportlab installation process has been deleted
**Solution**: Restore `await micropip.install('reportlab')`

#### **2. IndentationError**
```
IndentationError: unexpected indent
```
**Cause**: Python code does not start from the left edge
**Solution**: Remove all indentation from Python code

#### **3. NameError**
```
NameError: name 'js' is not defined
```
**Cause**: `import js` is missing
**Solution**: Add `import js` to required import statements

#### **4. TypeError: onPage**
```
TypeError: build() got an unexpected keyword argument 'onPage'
```
**Cause**: onPage argument incorrectly passed to doc.build()
**Solution**: Specify onPage when creating SimpleDocTemplate

#### **5. Font Error**
```
Font '...' not found
```
**Cause**: Font not properly registered
**Solution**: Use CID fonts (HeiseiKakuGo-W5, etc.) for Asian languages (Chinese, Japanese, Korean)

### **Error Handling Procedure (4 Steps)**

1. **Auto Display**: Error overlay displays in fullscreen
   - High visibility design that cannot be missed
   - Full display of error details (stack trace)

2. **One-Click Copy**: Click "Copy Full Error" button
   - Automatically copies to clipboard
   - "Copied" confirmation display

3. **Paste to AI**: Paste directly into chat area
   - AI automatically analyzes error content
   - Identifies cause

4. **Resolution**: Use the fix provided by AI
   - Explanation of specific fixes
   - Provision of corrected code

---

## **💡 Implementation Examples**

### **Level 1: Minimal Implementation**
```python
from reportlab.lib.pagesizes import A4
from reportlab.platypus import SimpleDocTemplate, Paragraph
from reportlab.lib.styles import getSampleStyleSheet
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.cidfonts import UnicodeCIDFont
from io import BytesIO
import base64
import js

pdf_buffer = BytesIO()
doc = SimpleDocTemplate(pdf_buffer, pagesize=A4)

# Font for international characters
pdfmetrics.registerFont(UnicodeCIDFont('HeiseiKakuGo-W5'))
styles = getSampleStyleSheet()
styles['Normal'].fontName = 'HeiseiKakuGo-W5'

# Simple document
story = []
story.append(Paragraph("Simple PDF Document", styles['Normal']))

doc.build(story)

pdf_buffer.seek(0)
pdf_base64 = base64.b64encode(pdf_buffer.read()).decode('utf-8')
js.pdf_base64_data = pdf_base64
```

### **Level 2: Basic Formatted Document**
```python
from reportlab.lib.pagesizes import A4
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib import colors
from reportlab.lib.units import mm
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.cidfonts import UnicodeCIDFont
from io import BytesIO
import base64
import js

pdf_buffer = BytesIO()
doc = SimpleDocTemplate(
    pdf_buffer,
    pagesize=A4,
    rightMargin=20*mm,
    leftMargin=20*mm,
    topMargin=20*mm,
    bottomMargin=20*mm
)

# Font settings
pdfmetrics.registerFont(UnicodeCIDFont('HeiseiKakuGo-W5'))
pdfmetrics.registerFont(UnicodeCIDFont('HeiseiMin-W3'))

styles = getSampleStyleSheet()
styles['Normal'].fontName = 'HeiseiKakuGo-W5'
styles['Title'].fontName = 'HeiseiMin-W3'
styles['Title'].fontSize = 20
styles['Heading1'].fontName = 'HeiseiKakuGo-W5'
styles['Heading1'].fontSize = 16

story = []

# Title
story.append(Paragraph("Business Report", styles['Title']))
story.append(Spacer(1, 20))

# Section 1
story.append(Paragraph("1. Overview", styles['Heading1']))
story.append(Spacer(1, 10))
story.append(Paragraph("I report on this month's business as follows.", styles['Normal']))
story.append(Spacer(1, 10))

# Bullet points
bullet_items = [
    "• Completion of Project A",
    "• Acquisition of 3 new clients",
    "• Achievement of sales target (120%)"
]
for item in bullet_items:
    story.append(Paragraph(item, styles['Normal']))
    story.append(Spacer(1, 5))

story.append(Spacer(1, 15))

# Section 2
story.append(Paragraph("2. Detailed Data", styles['Heading1']))
story.append(Spacer(1, 10))

# Table
data = [
    ['Item', 'Plan', 'Actual', 'Achievement Rate'],
    ['Sales', '$1M', '$1.2M', '120%'],
    ['Clients', '10 companies', '13 companies', '130%'],
    ['Cases', '15 cases', '18 cases', '120%'],
]

table = Table(data, colWidths=[40*mm, 35*mm, 35*mm, 35*mm])
table.setStyle(TableStyle([
    ('BACKGROUND', (0, 0), (-1, 0), colors.grey),
    ('TEXTCOLOR', (0, 0), (-1, 0), colors.whitesmoke),
    ('ALIGN', (0, 0), (-1, -1), 'CENTER'),
    ('FONTNAME', (0, 0), (-1, -1), 'HeiseiKakuGo-W5'),
    ('FONTSIZE', (0, 0), (-1, 0), 12),
    ('BOTTOMPADDING', (0, 0), (-1, 0), 12),
    ('BACKGROUND', (0, 1), (-1, -1), colors.beige),
    ('GRID', (0, 0), (-1, -1), 1, colors.black),
]))

story.append(table)

doc.build(story)

pdf_buffer.seek(0)
pdf_base64 = base64.b64encode(pdf_buffer.read()).decode('utf-8')
js.pdf_base64_data = pdf_base64
```

### **Level 3: Business Document (Invoice)**
```python
from reportlab.lib.pagesizes import A4
from reportlab.platypus import SimpleDocTemplate, Table, TableStyle, Paragraph, Spacer
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib import colors
from reportlab.lib.units import mm
from reportlab.lib.enums import TA_RIGHT, TA_CENTER
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.cidfonts import UnicodeCIDFont
from io import BytesIO
import base64
import js
from datetime import date

pdf_buffer = BytesIO()
doc = SimpleDocTemplate(
    pdf_buffer,
    pagesize=A4,
    rightMargin=20*mm,
    leftMargin=20*mm,
    topMargin=20*mm,
    bottomMargin=20*mm
)

# Font
pdfmetrics.registerFont(UnicodeCIDFont('HeiseiKakuGo-W5'))

# Style settings
styles = getSampleStyleSheet()
styles['Normal'].fontName = 'HeiseiKakuGo-W5'
styles['Title'].fontName = 'HeiseiKakuGo-W5'
styles['Title'].fontSize = 24
styles['Title'].alignment = TA_CENTER

# Right align style
right_style = ParagraphStyle(
    'RightAlign',
    parent=styles['Normal'],
    fontName='HeiseiKakuGo-W5',
    alignment=TA_RIGHT,
)

story = []

# Title
title = Paragraph("INVOICE", styles['Title'])
story.append(title)
story.append(Spacer(1, 20))

# Invoice number and date
invoice_info = Table([
    ['Invoice No: INV-2025-001', f'Issue Date: {date.today().strftime("%B %d, %Y")}']
], colWidths=[90*mm, 80*mm])
invoice_info.setStyle(TableStyle([
    ('FONTNAME', (0, 0), (-1, -1), 'HeiseiKakuGo-W5'),
    ('ALIGN', (0, 0), (0, 0), 'LEFT'),
    ('ALIGN', (1, 0), (1, 0), 'RIGHT'),
]))
story.append(invoice_info)
story.append(Spacer(1, 20))

# To
story.append(Paragraph("To: ABC Corporation", styles['Normal']))
story.append(Paragraph("Accounting Department", styles['Normal']))
story.append(Spacer(1, 20))

# Invoice amount (emphasized)
total_style = ParagraphStyle(
    'TotalAmount',
    parent=styles['Normal'],
    fontName='HeiseiKakuGo-W5',
    fontSize=18,
    textColor=colors.red,
)
story.append(Paragraph("Invoice Amount: $3,410.00 (Tax Included)", total_style))
story.append(Spacer(1, 20))

# Invoice details
data = [
    ['Item', 'Quantity', 'Unit Price', 'Amount'],
    ['Consulting Services', '20 hours', '$100.00', '$2,000.00'],
    ['System Development', '1 package', '$1,000.00', '$1,000.00'],
    ['Maintenance Support', '1 month', '$100.00', '$100.00'],
    ['', '', 'Subtotal', '$3,100.00'],
    ['', '', 'Tax (10%)', '$310.00'],
    ['', '', 'Total', '$3,410.00'],
]

table = Table(data, colWidths=[70*mm, 30*mm, 35*mm, 35*mm])
table.setStyle(TableStyle([
    # Header
    ('BACKGROUND', (0, 0), (-1, 0), colors.HexColor('#2C3E50')),
    ('TEXTCOLOR', (0, 0), (-1, 0), colors.whitesmoke),
    ('FONTNAME', (0, 0), (-1, -1), 'HeiseiKakuGo-W5'),
    ('FONTSIZE', (0, 0), (-1, 0), 11),
    ('ALIGN', (0, 0), (-1, -1), 'CENTER'),
    
    # Data rows
    ('BACKGROUND', (0, 1), (-1, -4), colors.HexColor('#ECF0F1')),
    ('GRID', (0, 0), (-1, -4), 1, colors.black),
    
    # Subtotal/Tax/Total
    ('BACKGROUND', (2, -3), (-1, -1), colors.HexColor('#BDC3C7')),
    ('FONTSIZE', (2, -1), (-1, -1), 12),
    ('TEXTCOLOR', (2, -1), (-1, -1), colors.HexColor('#C0392B')),
    
    # Right align
    ('ALIGN', (1, 1), (-1, -1), 'RIGHT'),
]))

story.append(table)
story.append(Spacer(1, 30))

# Bank information
story.append(Paragraph("Bank Account", styles['Heading2']))
bank_info = """
First National Bank - Main Branch
Checking Account 1234567890
Sample Company Inc.
"""
story.append(Paragraph(bank_info, styles['Normal']))
story.append(Spacer(1, 20))

# Notes
story.append(Paragraph("Notes", styles['Heading2']))
story.append(Paragraph("• Payment Due: Net 30 days", styles['Normal']))
story.append(Paragraph("• Please contact us if you have any questions", styles['Normal']))

# Company information (footer-like content)
story.append(Spacer(1, 30))
company_info = """
Sample Corporation
123 Main Street, Suite 500
New York, NY 10001
TEL: +1-212-555-0100 / FAX: +1-212-555-0101
Email: info@sample.com
"""
footer_style = ParagraphStyle(
    'Footer',
    parent=styles['Normal'],
    fontName='HeiseiKakuGo-W5',
    fontSize=9,
    alignment=TA_CENTER,
    textColor=colors.grey,
)
story.append(Paragraph(company_info, footer_style))

doc.build(story)

pdf_buffer.seek(0)
pdf_base64 = base64.b64encode(pdf_buffer.read()).decode('utf-8')
js.pdf_base64_data = pdf_base64
```

### **Level 4: Advanced Document (Multi-page with Headers/Footers)**
```python
from reportlab.lib.pagesizes import A4
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle, PageBreak
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib import colors
from reportlab.lib.units import mm, cm
from reportlab.lib.enums import TA_CENTER, TA_RIGHT, TA_JUSTIFY
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.cidfonts import UnicodeCIDFont
from io import BytesIO
import base64
import js
from datetime import date

# Header/Footer functions
def header_footer(canvas, doc):
    canvas.saveState()
    
    # Header
    canvas.setFont('HeiseiKakuGo-W5', 9)
    canvas.drawString(30*mm, A4[1] - 20*mm, "Annual Report 2025")
    canvas.drawRightString(A4[0] - 30*mm, A4[1] - 20*mm, 
                          date.today().strftime("%B %d, %Y"))
    
    # Header line
    canvas.setStrokeColor(colors.grey)
    canvas.setLineWidth(0.5)
    canvas.line(30*mm, A4[1] - 25*mm, A4[0] - 30*mm, A4[1] - 25*mm)
    
    # Footer
    canvas.drawString(30*mm, 20*mm, "Sample Corporation")
    canvas.drawCentredString(A4[0] / 2, 20*mm, f"- {doc.page} -")
    canvas.drawRightString(A4[0] - 30*mm, 20*mm, "Confidential")
    
    # Footer line
    canvas.line(30*mm, 25*mm, A4[0] - 30*mm, 25*mm)
    
    canvas.restoreState()

# Create document
pdf_buffer = BytesIO()
doc = SimpleDocTemplate(
    pdf_buffer,
    pagesize=A4,
    rightMargin=30*mm,
    leftMargin=30*mm,
    topMargin=40*mm,
    bottomMargin=35*mm,
    title="Annual Report",
    author="Sample Corporation",
    subject="2025 Performance Report",
    creator="Auto-generation System",
)

# Font
pdfmetrics.registerFont(UnicodeCIDFont('HeiseiKakuGo-W5'))
pdfmetrics.registerFont(UnicodeCIDFont('HeiseiMin-W3'))

# Styles
styles = getSampleStyleSheet()
styles['Normal'].fontName = 'HeiseiKakuGo-W5'
styles['Normal'].fontSize = 10
styles['Normal'].leading = 14

styles['Title'].fontName = 'HeiseiMin-W3'
styles['Title'].fontSize = 24
styles['Title'].leading = 28
styles['Title'].alignment = TA_CENTER

styles['Heading1'].fontName = 'HeiseiKakuGo-W5'
styles['Heading1'].fontSize = 16
styles['Heading1'].leading = 20
styles['Heading1'].spaceAfter = 10

styles['Heading2'].fontName = 'HeiseiKakuGo-W5'
styles['Heading2'].fontSize = 14
styles['Heading2'].leading = 18

# Body text style (justified)
body_style = ParagraphStyle(
    'BodyText',
    parent=styles['Normal'],
    fontName='HeiseiKakuGo-W5',
    fontSize=10,
    leading=16,
    alignment=TA_JUSTIFY,
    spaceAfter=10,
)

story = []

# Cover page
story.append(Spacer(1, 100))
story.append(Paragraph("Fiscal Year 2025", styles['Title']))
story.append(Spacer(1, 20))
story.append(Paragraph("Annual Report", styles['Title']))
story.append(Spacer(1, 50))
story.append(Paragraph("Sample Corporation", styles['Title']))
story.append(PageBreak())

# Table of contents
story.append(Paragraph("Table of Contents", styles['Title']))
story.append(Spacer(1, 20))

toc_data = [
    ['1. CEO Message', '3'],
    ['2. Business Overview', '4'],
    ['3. Financial Highlights', '5'],
    ['4. Business Performance', '6'],
    ['5. Future Outlook', '7'],
]

toc_table = Table(toc_data, colWidths=[140*mm, 20*mm])
toc_table.setStyle(TableStyle([
    ('FONTNAME', (0, 0), (-1, -1), 'HeiseiKakuGo-W5'),
    ('FONTSIZE', (0, 0), (-1, -1), 12),
    ('ALIGN', (1, 0), (1, -1), 'RIGHT'),
    ('LINEBELOW', (0, 0), (-1, -1), 0.5, colors.grey),
    ('BOTTOMPADDING', (0, 0), (-1, -1), 8),
]))
story.append(toc_table)
story.append(PageBreak())

# 1. CEO Message
story.append(Paragraph("1. CEO Message", styles['Heading1']))
story.append(Spacer(1, 10))
message_text = """
To Our Shareholders and Investors,

Fiscal year 2025 has been a major turning point for our company. Having fully recovered from the impact of COVID-19,
we were able to achieve record-breaking performance through the promotion of digital transformation.

Sales increased by 20% year-over-year to $1.2 billion, operating profit increased by 25% to $150 million,
and we achieved growth in all business segments. We deeply appreciate your support in making this possible.

We will continue to aim for sustainable growth and promote ESG management.
We ask for your continued support.

President and CEO
John Smith
"""
for paragraph in message_text.strip().split('\n\n'):
    story.append(Paragraph(paragraph, body_style))
story.append(PageBreak())

# 2. Business Overview
story.append(Paragraph("2. Business Overview", styles['Heading1']))
story.append(Spacer(1, 10))

story.append(Paragraph("2.1 Main Business", styles['Heading2']))
story.append(Spacer(1, 5))

business_data = [
    ['Business Division', 'Sales Composition', 'Main Products/Services'],
    ['IT Solutions', '45%', 'System Development, Cloud Services'],
    ['Consulting', '30%', 'Management Consulting, DX Support'],
    ['Products', '25%', 'Package Software, SaaS'],
]

business_table = Table(business_data, colWidths=[50*mm, 35*mm, 70*mm])
business_table.setStyle(TableStyle([
    ('BACKGROUND', (0, 0), (-1, 0), colors.HexColor('#34495E')),
    ('TEXTCOLOR', (0, 0), (-1, 0), colors.white),
    ('FONTNAME', (0, 0), (-1, -1), 'HeiseiKakuGo-W5'),
    ('FONTSIZE', (0, 0), (-1, -1), 10),
    ('ALIGN', (0, 0), (-1, -1), 'CENTER'),
    ('GRID', (0, 0), (-1, -1), 1, colors.grey),
    ('BACKGROUND', (0, 1), (-1, -1), colors.HexColor('#ECF0F1')),
]))
story.append(business_table)
story.append(Spacer(1, 15))

story.append(Paragraph("2.2 Global Expansion", styles['Heading2']))
story.append(Spacer(1, 5))
global_text = """
We currently operate in three regions: Asia, North America, and Europe.
Overseas sales ratio has reached 35%, with particularly notable growth in the Asian region.
We will accelerate expansion into Southeast Asia and aim for a 50% overseas sales ratio by 2030.
"""
story.append(Paragraph(global_text, body_style))
story.append(PageBreak())

# 3. Financial Highlights
story.append(Paragraph("3. Financial Highlights", styles['Heading1']))
story.append(Spacer(1, 10))

financial_data = [
    ['Item', 'FY2023', 'FY2024', 'FY2025', 'YoY Change'],
    ['Sales ($M)', '900', '1,000', '1,200', '+20%'],
    ['Operating Profit ($M)', '100', '120', '150', '+25%'],
    ['Net Profit ($M)', '70', '85', '110', '+29%'],
    ['ROE (%)', '12.5', '14.2', '16.8', '+2.6pt'],
    ['Dividend ($)', '3.00', '3.50', '4.00', '+14%'],
]

financial_table = Table(financial_data, colWidths=[40*mm, 28*mm, 28*mm, 28*mm, 25*mm])
financial_table.setStyle(TableStyle([
    ('BACKGROUND', (0, 0), (-1, 0), colors.HexColor('#2C3E50')),
    ('TEXTCOLOR', (0, 0), (-1, 0), colors.white),
    ('FONTNAME', (0, 0), (-1, -1), 'HeiseiKakuGo-W5'),
    ('FONTSIZE', (0, 0), (-1, -1), 9),
    ('ALIGN', (1, 0), (-1, -1), 'RIGHT'),
    ('ALIGN', (0, 0), (0, -1), 'LEFT'),
    ('GRID', (0, 0), (-1, -1), 1, colors.grey),
    ('BACKGROUND', (0, 1), (-1, -1), colors.white),
    ('ROWBACKGROUNDS', (0, 1), (-1, -1), [colors.white, colors.HexColor('#F8F9FA')]),
]))
story.append(financial_table)

# Set headers/footers with SimpleDocTemplate
doc.build(story, onFirstPage=header_footer, onLaterPages=header_footer)

pdf_buffer.seek(0)
pdf_base64 = base64.b64encode(pdf_buffer.read()).decode('utf-8')
js.pdf_base64_data = pdf_base64
```

---

## **📑 Business Document Templates**

### **Receipt**
```python
# Receipt template
from reportlab.lib.units import mm
from reportlab.lib.colors import HexColor

# Title
story.append(Paragraph("RECEIPT", styles['Title']))
story.append(Spacer(1, 20))

# To and amount
story.append(Paragraph("To: ABC Corporation", styles['Normal']))
story.append(Spacer(1, 10))

amount_style = ParagraphStyle(
    'Amount',
    fontName='HeiseiKakuGo-W5',
    fontSize=20,
    alignment=TA_CENTER,
)
story.append(Paragraph("$1,000.00", amount_style))
story.append(Spacer(1, 10))
story.append(Paragraph("(One Thousand Dollars)", styles['Normal']))

# Description
story.append(Spacer(1, 15))
story.append(Paragraph("For: Consulting Services", styles['Normal']))

# Date and issuer
story.append(Spacer(1, 20))
story.append(Paragraph(f"Issue Date: {date.today().strftime('%B %d, %Y')}", styles['Normal']))

# Signature box
signature_data = [['Authorized Signature']]
signature_table = Table(signature_data, colWidths=[60*mm], rowHeights=[15*mm])
signature_table.setStyle(TableStyle([
    ('BOX', (0, 0), (-1, -1), 1, colors.black),
    ('ALIGN', (0, 0), (-1, -1), 'CENTER'),
    ('VALIGN', (0, 0), (-1, -1), 'BOTTOM'),
    ('FONTNAME', (0, 0), (-1, -1), 'HeiseiKakuGo-W5'),
    ('FONTSIZE', (0, 0), (-1, -1), 8),
]))
story.append(signature_table)
```

### **Certificate/Award**
```python
# Certificate template
from reportlab.lib.enums import TA_CENTER

cert_title_style = ParagraphStyle(
    'CertTitle',
    fontName='HeiseiMin-W3',
    fontSize=28,
    alignment=TA_CENTER,
    spaceAfter=30
)

cert_body_style = ParagraphStyle(
    'CertBody',
    fontName='HeiseiKakuGo-W5',
    fontSize=14,
    alignment=TA_CENTER,
    leading=24,
)

story.append(Spacer(1, 50))
story.append(Paragraph("Certificate of Completion", cert_title_style))
story.append(Spacer(1, 40))

story.append(Paragraph("Name: John Smith", cert_body_style))
story.append(Spacer(1, 30))

cert_text = """
This certifies that you have
successfully completed the
"Project Management Training"
conducted by our company
with excellent results.
"""
for line in cert_text.strip().split('\n'):
    story.append(Paragraph(line, cert_body_style))
    story.append(Spacer(1, 10))

story.append(Spacer(1, 40))
story.append(Paragraph(date.today().strftime("%B %d, %Y"), cert_body_style))
story.append(Spacer(1, 20))
story.append(Paragraph("Sample Corporation", cert_body_style))
story.append(Paragraph("CEO Signature", cert_body_style))
```

### **Contract**
```python
# Contract template
contract_style = ParagraphStyle(
    'Contract',
    fontName='HeiseiKakuGo-W5',
    fontSize=10,
    leading=16,
    alignment=TA_JUSTIFY,
)

story.append(Paragraph("Service Agreement", styles['Title']))
story.append(Spacer(1, 20))

contract_text = """
ABC Corporation (hereinafter "Party A") and XYZ Corporation (hereinafter "Party B")
hereby enter into this Service Agreement (hereinafter "this Agreement") as follows.

Article 1 (Purpose)
Party A commissions Party B with the services described in the attached document (hereinafter "the Services"),
and Party B accepts such commission.

Article 2 (Service Details)
The details of the Services shall be as specified in the separate specifications.

Article 3 (Contract Period)
The validity period of this Agreement shall be from April 1, 2025, to March 31, 2026.

Article 4 (Compensation)
Party A shall pay Party B a monthly fee of $10,000 (excluding applicable taxes) for the Services.

Article 5 (Confidentiality)
Party A and Party B shall not disclose any confidential information of the other party
obtained in connection with this Agreement to third parties without prior written consent.
"""

for paragraph in contract_text.strip().split('\n\n'):
    story.append(Paragraph(paragraph, contract_style))
    story.append(Spacer(1, 10))

# Signature area
story.append(Spacer(1, 30))
signature_data = [
    ['Party A', '', 'Party B', ''],
    ['Address:', '_______________', 'Address:', '_______________'],
    ['Company:', '_______________', 'Company:', '_______________'],
    ['Representative:', '_______________', 'Representative:', '_______________'],
    ['Signature', '', 'Signature', ''],
]

signature_table = Table(signature_data, colWidths=[20*mm, 60*mm, 20*mm, 60*mm])
signature_table.setStyle(TableStyle([
    ('FONTNAME', (0, 0), (-1, -1), 'HeiseiKakuGo-W5'),
    ('FONTSIZE', (0, 0), (-1, -1), 10),
    ('LINEBELOW', (1, 1), (1, 3), 0.5, colors.black),
    ('LINEBELOW', (3, 1), (3, 3), 0.5, colors.black),
]))
story.append(signature_table)
```

### **Quotation**
```python
# Quotation template
story.append(Paragraph("QUOTATION", styles['Title']))
story.append(Spacer(1, 20))

story.append(Paragraph("To: ABC Corporation", styles['Normal']))
story.append(Spacer(1, 15))

estimate_summary = Table([
    ['Quoted Amount', '$11,000.00 (Tax Included)'],
    ['Subject', 'Website Development Package'],
    ['Delivery', '2 months after contract'],
    ['Valid Until', '30 days from issue date'],
], colWidths=[40*mm, 100*mm])

estimate_summary.setStyle(TableStyle([
    ('FONTNAME', (0, 0), (-1, -1), 'HeiseiKakuGo-W5'),
    ('FONTSIZE', (0, 0), (0, 0), 12),
    ('FONTSIZE', (1, 0), (1, 0), 14),
    ('TEXTCOLOR', (1, 0), (1, 0), colors.red),
    ('BACKGROUND', (0, 0), (0, -1), colors.HexColor('#E8E8E8')),
    ('ALIGN', (0, 0), (0, -1), 'RIGHT'),
    ('BOX', (0, 0), (-1, -1), 1, colors.black),
    ('GRID', (0, 0), (-1, -1), 0.5, colors.grey),
]))

story.append(estimate_summary)
story.append(Spacer(1, 20))

# Details
story.append(Paragraph("Details", styles['Heading2']))
story.append(Spacer(1, 10))

detail_data = [
    ['Item', 'Quantity', 'Unit Price', 'Amount'],
    ['Design Fee', '1 package', '$3,000.00', '$3,000.00'],
    ['Coding Fee', '10 pages', '$300.00', '$3,000.00'],
    ['System Development', '1 package', '$2,000.00', '$2,000.00'],
    ['Direction Fee', '2 months', '$500.00', '$1,000.00'],
    ['', '', 'Subtotal', '$9,000.00'],
    ['', '', 'Tax (10%)', '$1,000.00'],
    ['', '', 'Total', '$10,000.00'],
]

detail_table = Table(detail_data, colWidths=[60*mm, 30*mm, 35*mm, 35*mm])
# Style settings (same as invoice)
```

---

## **🔒 PDF-specific Advanced Features**

### **Watermark**
```python
def add_watermark(canvas, doc):
    """Add watermark to each page"""
    canvas.saveState()
    
    # Watermark text settings
    canvas.setFont("HeiseiKakuGo-W5", 60)
    canvas.setFillColorRGB(0.8, 0.8, 0.8, alpha=0.3)  # Light gray, 30% transparency
    
    # Rotate 45 degrees
    canvas.translate(A4[0]/2, A4[1]/2)
    canvas.rotate(45)
    
    # Center placement
    canvas.drawCentredString(0, 0, "CONFIDENTIAL")
    
    canvas.restoreState()

# Use with SimpleDocTemplate
doc = SimpleDocTemplate(
    pdf_buffer,
    pagesize=A4,
    onFirstPage=add_watermark,
    onLaterPages=add_watermark
)
```

### **QR Code Generation**
```python
# Using qrcode library (requires installation)
import qrcode
from io import BytesIO
from reportlab.platypus import Image

# Generate QR code
qr = qrcode.QRCode(
    version=1,
    error_correction=qrcode.constants.ERROR_CORRECT_L,
    box_size=10,
    border=4,
)
qr.add_data('https://example.com/invoice/12345')
qr.make(fit=True)

# Save as image
img = qr.make_image(fill_color="black", back_color="white")
img_buffer = BytesIO()
img.save(img_buffer, format='PNG')
img_buffer.seek(0)

# Add to PDF
qr_image = Image(img_buffer, width=30*mm, height=30*mm)
story.append(qr_image)
```

### **Password Protection**
```python
from reportlab.lib import pdfencrypt

# Encryption settings
encrypt = pdfencrypt.StandardEncryption(
    userPassword="user123",      # View password
    ownerPassword="owner456",    # Administrator password
    canPrint=1,                  # Allow printing
    canModify=0,                 # Prohibit editing
    canCopy=0,                   # Prohibit copying
    canAnnotate=0                # Prohibit annotations
)

# Apply encryption when creating document
doc = SimpleDocTemplate(
    pdf_buffer,
    pagesize=A4,
    encrypt=encrypt
)
```

### **Metadata Settings**
```python
# PDF property settings
doc = SimpleDocTemplate(
    pdf_buffer,
    pagesize=A4,
    title="Invoice No.2025-001",
    author="Sample Corporation",
    subject="January 2025 Invoice",
    creator="Auto-generation System v2.0",
    producer="ReportLab",
    keywords="Invoice,2025,January",
    creationDate=date.today()
)
```

### **Custom Page Sizes**
```python
from reportlab.lib.pagesizes import landscape, portrait

# A4 landscape
doc = SimpleDocTemplate(pdf_buffer, pagesize=landscape(A4))

# Custom size (business card size)
BUSINESS_CARD = (91*mm, 55*mm)
doc = SimpleDocTemplate(pdf_buffer, pagesize=BUSINESS_CARD)

# Letter size
LETTER = (8.5*25.4*mm, 11*25.4*mm)
doc = SimpleDocTemplate(pdf_buffer, pagesize=LETTER)
```

---

## **🌍 International Support Guide**

### **Available CID Fonts**
```python
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.cidfonts import UnicodeCIDFont

# Gothic
pdfmetrics.registerFont(UnicodeCIDFont('HeiseiKakuGo-W5'))

# Mincho
pdfmetrics.registerFont(UnicodeCIDFont('HeiseiMin-W3'))

# Mincho (Bold)
pdfmetrics.registerFont(UnicodeCIDFont('HeiseiMin-W7'))
```

### **Vertical Writing Support**
```python
# Vertical writing style (experimental feature)
from reportlab.platypus import Paragraph
from reportlab.lib.styles import ParagraphStyle

vertical_style = ParagraphStyle(
    'Vertical',
    fontName='HeiseiMin-W3',
    fontSize=12,
    wordWrap='CJK',  # Line breaking for CJK characters
)

# Note: Full vertical writing has limitations in ReportLab
# Consider using with other libraries if necessary
```

### **Date Formatting**
```python
from datetime import date

def format_date_us(dt):
    """Format date in US style"""
    return dt.strftime("%B %d, %Y")  # e.g., "January 17, 2025"

def format_date_eu(dt):
    """Format date in European style"""
    return dt.strftime("%d %B %Y")  # e.g., "17 January 2025"

def format_date_iso(dt):
    """Format date in ISO style"""
    return dt.strftime("%Y-%m-%d")  # e.g., "2025-01-17"

# Usage example
today = date.today()
us_date = format_date_us(today)
story.append(Paragraph(f"Issue Date: {us_date}", styles['Normal']))
```

---

## **✅ Implementation Checklist**

### **Required Check Items**
- [ ] reportlab installation process exists
- [ ] Python code starts from the left edge
- [ ] `import js` is included
- [ ] Assignment to `js.pdf_base64_data` exists
- [ ] Error overlay HTML is complete

### **International Support Check Items**
- [ ] CID fonts are registered for international characters
- [ ] fontName is set in styles
- [ ] Text displays correctly for target languages

### **Operation Check Items**
- [ ] Page loads normally
- [ ] Download button becomes enabled
- [ ] PDF file can be downloaded
- [ ] Overlay displays on error
- [ ] Error copy button functions

---

## **📊 ReportLab Reference**

### **Basic Elements**
| Class | Purpose | Example |
|-------|---------|---------|
| `SimpleDocTemplate` | Basic structure of PDF document | `doc = SimpleDocTemplate(buffer, pagesize=A4)` |
| `Paragraph` | Paragraph text | `Paragraph("text", style)` |
| `Table` | Table | `Table(data, colWidths=[...])` |
| `Spacer` | Blank space | `Spacer(1, 20)` |
| `PageBreak` | Page break | `PageBreak()` |
| `Image` | Image | `Image(buffer, width=100, height=100)` |

### **Page Sizes**
```python
from reportlab.lib.pagesizes import A4, A3, letter, legal, landscape

# Standard sizes
A4 = (210*mm, 297*mm)
A3 = (297*mm, 420*mm)
letter = (8.5*inch, 11*inch)
legal = (8.5*inch, 14*inch)

# Orientation
portrait(A4)   # Portrait (default)
landscape(A4)  # Landscape
```

### **Color Definitions**
```python
from reportlab.lib import colors

# Basic colors
colors.black, colors.white, colors.red, colors.blue, colors.green

# RGB color
colors.Color(0.5, 0.5, 0.5)  # Gray

# HEX color
colors.HexColor('#2C3E50')

# With transparency
colors.Color(0, 0, 0, alpha=0.5)
```

### **Units**
```python
from reportlab.lib.units import inch, cm, mm, pt

# Conversion
1*inch = 72*pt
1*cm = 28.35*pt
1*mm = 2.835*pt
```

---

## **🚀 Quick Start**

### **Fastest Implementation (3 Steps)**

1. **Copy HTML template**
2. **Change title** (2 places: `<title>` and `<h1>`)
3. **Define document content in Python code**

```python
# Minimal change example
doc = SimpleDocTemplate(pdf_buffer, pagesize=A4)
story = []
story.append(Paragraph("Your Document Title", styles['Title']))
story.append(Paragraph("Body text here", styles['Normal']))
doc.build(story)
# ... Rest is save process (as per template)
```

---

## **📝 Update History**

- **v2.0** (2025-01-17)
  - Reorganized implementation rules by priority
  - Added error UI functionality (fullscreen overlay)
  - Expanded implementation examples to 4 levels
  - Added 5 types of business document templates
  - Added PDF-specific advanced features (encryption, metadata, etc.)

- **v1.2** (2025-01-17)
  - Emphasized importance of external library installation
  - Added error handling methods

- **v1.1** (2025-09-12)
  - Enhanced header/footer functionality description

- **v1.0** (2025-01-17)
  - Initial release

---

## **📄 License**

MIT License - Free to use, modify, redistribute, and use commercially

---

## **🆘 Support**

### **What to do when problems occur**

1. **Copy error message**
   - Use the "Copy Full Error" button in the error overlay

2. **Paste and consult with AI**
   - Paste the copied content into the chat area
   - AI will analyze the cause and provide a fixed version

3. **Check with checklist**
   - Confirm that required items are not missing
   - Especially the reportlab installation process

4. **Reference implementation examples**
   - Start with Level 1 minimal implementation
   - Add features gradually

### **Frequently Asked Questions**

**Q: Characters are garbled**
A: Use CID fonts (HeiseiKakuGo-W5, etc.) and set fontName in all styles.

**Q: Are complex layouts possible?**
A: Multi-column layouts are possible using Frame and PageTemplate. However, flexibility like HTML is limited.

**Q: Can I edit existing PDFs?**
A: ReportLab is primarily for creating new documents. Consider using PyPDF2 etc. for editing existing PDFs.

**Q: Can I embed graphs?**
A: It's possible to embed graphs created with matplotlib as images (see Level 4 example).

---

**This completes Recipe v2.0. Detailed explanations and examples are included to ensure AI can properly understand and create appropriate PDF generation pages.**
