# **PPTX Auto-Generation Page Recipe v4.0**
**Complete Guide for AI-Powered One-Click Download Page Generation**

---

## **📌 About This Recipe**

A complete instruction manual for generative AI to automatically create a **single HTML file** that can generate and download PowerPoint files (.pptx) in the browser based on user instructions.

### **Features of v4.0**
- 🎯 Clear prioritization of implementation rules
- 🖼️ Image placeholders are **optional features** (used only with explicit instructions)
- 📝 Rich content including detailed explanations
- 🔍 Complete error handling procedures and examples
- 📊 Provides practical placement guides

### **Important Changes (v3.x -> v4.0)**
- **Default behavior change**: Image placeholders are not automatically placed
- **Explicit instructions required**: Clear instructions like "place images" are needed if images are required
- **Simplicity focus**: Default is clean text-based presentations

---

## **🚨 Implementation Rules (By Priority)**

### **🔴 Level 1: Absolute Prohibitions (Strictly No Changes/Deletions)**

The following elements must **never be changed**:

#### **1.1 python-pptx Library Installation Process**
```javascript
await pyodide.loadPackage("micropip");
await pyodide.runPythonAsync(`
    import micropip
    await micropip.install('python-pptx')
`);
```

**Why absolutely necessary:**
- python-pptx is an external library for PowerPoint file generation
- Not included by default in the browser's Pyodide environment
- Must be installed at runtime using micropip
- **Error that always occurs if deleted**: `ModuleNotFoundError: No module named 'pptx'`

#### **1.2 Error Overlay HTML/CSS/JavaScript**
- Error overlay HTML structure (`<div id="error-overlay">`)
- Error handling JavaScript code
- CSS class names (`.error-overlay`, `.error-card`, etc.)

**Reason**: Critical feature for ensuring usability during errors

#### **1.3 Pyodide Loading Process**
```html
<script src="https://cdn.jsdelivr.net/pyodide/v0.25.0/full/pyodide.js"></script>
```

**Reason**: Essential library that forms the foundation of the Python execution environment

### **🟡 Level 2: Required Rules (Must Follow)**

#### **2.1 Python Code Indentation Rules**
```python
# ✅ Correct (starts at left edge)
from pptx import Presentation
import base64

# ❌ Wrong (with indentation)
    from pptx import Presentation  # IndentationError occurs
    import base64
```

**Reason**: Adding indentation to Python code due to HTML indentation causes errors

#### **2.2 Required Import Statements**
```python
from pptx import Presentation
from io import BytesIO
import base64
import js  # Required for JavaScript integration
```

**Error if omitted**: `NameError: name 'js' is not defined`

#### **2.3 Base64 Data Transfer**
Must execute at the end of Python code:
```python
js.pptx_base64_data = pptx_base64
```

**Reason**: Required process for JavaScript to receive PPTX data

### **🟢 Level 3: Customizable Elements**

The following can be freely modified:
- Page title (`<title>` tag and `<h1>` tag)
- Presentation content within Python code
- Slide design and layout
- Download filename
- Placeholder placement and content (if used)

---

## **🖼️ Image Placeholder Feature (Optional)**

### **Basic Policy**
**Image placeholders are completely optional**
- Unless explicitly instructed, image placeholders are not placed
- Create placeholders only when users mention images
- Even if image files are attached, always process as placeholders (no direct embedding)
- Default is clean text-based presentations

### **Why Use Placeholder Method**
- **HTML file weight reduction**: Only a few KB (vs. several MB to tens of MB with embedded images)
- **Context window conservation**: Doesn't exceed AI processing limits
- **Flexibility**: Images can be freely changed later
- **Shareability**: Easy to reuse among multiple people
- **Copyright**: Avoids image rights issues

### **Conditions for Using Placeholders**
Place placeholders when instructions like these are given:

1. **Explicit image requests**
   - "Add images" "Place photos" "Add logo"
   - "Insert diagrams" "Place graphs" "Add illustrations"
   
2. **Image file attachments**
   - When image files are attached, create placeholders with those filenames
   - Use attached images as reference information (no direct embedding)

3. **Clear mentions of visual elements**
   - Clear instructions like "Add visuals" "Include diagrams"

### **When Not to Use Placeholders**
- General instructions like just "make a presentation"
- When there's no mention of images
- When there are instructions like "text only" or "keep it simple"
- When text information alone is sufficient

### **Information to Include in Placeholders**
When using placeholders, include the following information:
1. **Recommended filename or description** (e.g., `logo.png`, `Main image`)
2. **Placement position** (e.g., "bottom right", "center", "left side")
3. **Recommended size** (e.g., `800x600px`, `4x3 inches`)
4. **Purpose** (e.g., "Company logo", "Main visual", "Background image")

### **Recommended Placement Patterns (When Used)**
| Slide Type | Image Type | Recommended Position | Recommended Size |
|------------|------------|---------------------|------------------|
| Title | Logo | Bottom right/left | 2x1 inches |
| Content | Main image | Right side | 4x3 inches |
| Data | Graph/Chart | Center | 6x4 inches |
| Summary | Icon | Next to bullets | 1x1 inches |

---

## **📝 HTML Template (Complete Version)**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- Customizable: Page title -->
    <title>Download Presentation</title>

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
        
        /* Error overlay (No changes allowed) */
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
            Presentation
        </h1>

        <p id="status-message" class="text-gray-600 mb-6 h-12 flex items-center justify-center">
            Preparing file...
        </p>

        <div id="loading-spinner" class="spinner mx-auto mb-6"></div>

        <button id="download-button" class="w-full bg-blue-600 text-white font-bold py-3 px-6 rounded-lg shadow-md hover:bg-blue-700 transition duration-300 transform hover:scale-105" disabled>
            Download File
        </button>
    </div>

    <!-- Customizable: Python code (following rules) -->
    <script type="text/python" id="python-code">
from pptx import Presentation
from io import BytesIO
import base64
import js

# Create presentation
prs = Presentation()

# Add title slide
slide_layout = prs.slide_layouts[0]
slide = prs.slides.add_slide(slide_layout)
title = slide.shapes.title
subtitle = slide.placeholders[1]

title.text = "Auto-generated by AI"
subtitle.text = "This page was created dynamically"

# Save to BytesIO
pptx_io = BytesIO()
prs.save(pptx_io)
pptx_io.seek(0)

# Base64 encode
pptx_bytes = pptx_io.read()
pptx_base64 = base64.b64encode(pptx_bytes).decode('utf-8')

# Pass to JavaScript (Required)
js.pptx_base64_data = pptx_base64

print("PPTX file has been generated in memory.")
    </script>

    <!-- Error overlay (No changes allowed) -->
    <div id="error-overlay" class="error-overlay" role="dialog" aria-modal="true" aria-labelledby="error-title">
        <div class="error-card">
            <div class="error-title" id="error-title">An error occurred: Please paste the error message below to AI chat area to request a fix</div>
            <div class="error-subtitle">Copy the full error text and paste it directly to AI. It will identify the cause and suggest a patch.</div>
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

                // Install python-pptx (deletion prohibited)
                statusMessage.textContent = 'Installing libraries...';
                await pyodide.loadPackage("micropip");
                await pyodide.runPythonAsync(`
                    import micropip
                    await micropip.install('python-pptx')
                `);

                statusMessage.textContent = 'Generating file...';
                const pythonCode = document.getElementById('python-code').textContent;
                window.pptx_base64_data = null; 
                await pyodide.runPythonAsync(pythonCode);
                const base64Data = window.pptx_base64_data;

                if (!base64Data) {
                    throw new Error("Python code did not produce any data.");
                }

                statusMessage.textContent = 'Ready!';
                loadingSpinner.style.display = 'none';
                downloadButton.disabled = false;
                downloadButton.addEventListener('click', () => downloadFile(base64Data, 'presentation.pptx'));

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
                const blob = new Blob([byteArray], { type: 'application/vnd.openxmlformats-officedocument.presentationml.presentation' });

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
ModuleNotFoundError: No module named 'pptx'
```
**Cause**: python-pptx installation process has been deleted
**Solution**: Restore `await micropip.install('python-pptx')`

#### **2. IndentationError**
```
IndentationError: unexpected indent
```
**Cause**: Python code doesn't start from the left edge
**Solution**: Remove all indentation from Python code

#### **3. NameError**
```
NameError: name 'js' is not defined
```
**Cause**: Missing `import js`
**Solution**: Add `import js` to required import statements

#### **4. AttributeError**
```
AttributeError: 'NoneType' object has no attribute 'text'
```
**Cause**: The placeholder doesn't exist in the slide layout
**Solution**: Check layout index or use `add_textbox()`

### **Error Handling Process (4 Steps)**

1. **Auto-display**: Error overlay displays in full screen
   - High visibility design that can't be missed
   - Complete display of error details (stack trace)

2. **One-click copy**: Click "Copy Full Error" button
   - Auto-copy to clipboard
   - "Copied" confirmation display

3. **Paste to AI**: Paste directly into chat area
   - AI automatically analyzes error content
   - Identifies cause

4. **Resolution**: Use the fixed version provided by AI
   - Explanation of specific fixes
   - Provision of fixed code

---

## **💡 Implementation Examples**

### **Level 1: Minimal Implementation (No Images)**
```python
# Default behavior: No image placeholders
from pptx import Presentation
from io import BytesIO
import base64
import js

prs = Presentation()

# Simple title slide
slide = prs.slides.add_slide(prs.slide_layouts[0])
slide.shapes.title.text = "Simple Presentation"
slide.placeholders[1].text = "Text-only composition"

# Content slide
slide = prs.slides.add_slide(prs.slide_layouts[1])
slide.shapes.title.text = "Content"
slide.placeholders[1].text = "• Point 1\n• Point 2\n• Point 3"

# Save process
pptx_io = BytesIO()
prs.save(pptx_io)
pptx_io.seek(0)
js.pptx_base64_data = base64.b64encode(pptx_io.read()).decode('utf-8')
```

### **Level 2: Basic Multi-Slide Structure (No Images)**
```python
from pptx import Presentation
from pptx.util import Inches, Pt
from io import BytesIO
import base64
import js

prs = Presentation()

# Slide 1: Title
slide = prs.slides.add_slide(prs.slide_layouts[0])
slide.shapes.title.text = "FY2025 Business Plan"
slide.placeholders[1].text = "Sample Corporation"

# Slide 2: Agenda
slide = prs.slides.add_slide(prs.slide_layouts[1])
slide.shapes.title.text = "Agenda"
content = slide.placeholders[1]
content.text = "1. Current Analysis\n2. Goal Setting\n3. Strategy Overview\n4. Execution Plan\n5. Summary"

# Slide 3: Current Analysis
slide = prs.slides.add_slide(prs.slide_layouts[1])
slide.shapes.title.text = "Current Analysis"
content = slide.placeholders[1]
content.text = "• Market growth rate: 15% annually\n• Our market share: 12%\n• Competition: Intensifying\n• Strengths: Technology and support system"

# Slide 4: Goal Setting
slide = prs.slides.add_slide(prs.slide_layouts[1])
slide.shapes.title.text = "FY2025 Goals"
content = slide.placeholders[1]
content.text = "• Sales: 120% of previous year\n• New customers: 100 companies\n• Customer satisfaction: 95%\n• Market share: Expand to 15%"

# Slide 5: Summary
slide = prs.slides.add_slide(prs.slide_layouts[1])
slide.shapes.title.text = "Summary"
slide.placeholders[1].text = "We will work as one company to achieve our goals"

# Save and convert
pptx_io = BytesIO()
prs.save(pptx_io)
pptx_io.seek(0)
js.pptx_base64_data = base64.b64encode(pptx_io.read()).decode('utf-8')
```

### **Level 3: Option - With Image Placeholders (When Explicitly Instructed)**
```python
# ※ Use only when user explicitly requests images
from pptx import Presentation
from pptx.util import Inches, Pt
from pptx.enum.text import PP_ALIGN
from pptx.dml.color import RGBColor
from io import BytesIO
import base64
import js

def add_image_placeholder(slide, position, size, description):
    """Add image placeholder (optional feature)"""
    left, top = position
    width, height = size
    
    textbox = slide.shapes.add_textbox(left, top, width, height)
    text_frame = textbox.text_frame
    text_frame.text = description
    
    # Visual border
    textbox.line.color.rgb = RGBColor(150, 150, 150)
    textbox.line.width = Pt(1)
    
    # Background color
    textbox.fill.solid()
    textbox.fill.fore_color.rgb = RGBColor(250, 250, 250)
    
    return textbox

prs = Presentation()

# Title slide
slide = prs.slides.add_slide(prs.slide_layouts[0])
slide.shapes.title.text = "Annual Report 2025"
slide.placeholders[1].text = "Sample Corporation"

# Logo placeholder (when user explicitly requests)
# Example: When instructed "Please place a logo"
add_image_placeholder(
    slide,
    position=(Inches(7.5), Inches(4.5)),
    size=(Inches(2), Inches(1)),
    description="[Image Recommended]\nlogo.png\nCompany Logo\n200x100px"
)

# Content slide
slide = prs.slides.add_slide(prs.slide_layouts[1])
slide.shapes.title.text = "Product Introduction"
slide.placeholders[1].text = "• High performance\n• Easy to use\n• Cost-effective"

# Main image placeholder (when user requests images)
# Example: When instructed "Please add product images"
add_image_placeholder(
    slide,
    position=(Inches(5), Inches(1.5)),
    size=(Inches(4), Inches(3)),
    description="[Image Recommended]\nproduct_main.jpg\nMain Product Image\n800x600px\n\nPlease place an image\nthat clearly shows\nthe product overview"
)

# Save
pptx_io = BytesIO()
prs.save(pptx_io)
pptx_io.seek(0)
js.pptx_base64_data = base64.b64encode(pptx_io.read()).decode('utf-8')
```

### **Level 4: Advanced Implementation with Dynamic Detection**
```python
from pptx import Presentation
from pptx.util import Inches, Pt
from pptx.dml.color import RGBColor
from io import BytesIO
import base64
import js

def should_add_image_placeholder(user_request):
    """Determine if user's request includes image instructions"""
    
    # Image-related keywords
    image_keywords = [
        'image', 'photo', 'picture', 'logo', 'figure',
        'graph', 'chart', 'illustration', 'visual',
        'graphic', 'diagram', 'icon', 'artwork'
    ]
    
    # Explicit instruction patterns
    explicit_patterns = [
        'add', 'place', 'insert', 'include', 'put', 'attach',
        'embed', 'incorporate', 'space', 'location'
    ]
    
    # Default is False (no images)
    request_lower = user_request.lower() if user_request else ""
    
    for keyword in image_keywords:
        if keyword in request_lower:
            for pattern in explicit_patterns:
                if pattern in request_lower:
                    return True
    
    return False

def add_image_placeholder_if_needed(slide, position, size, description, user_wants_image):
    """Conditionally add placeholder"""
    if user_wants_image:
        left, top = position
        width, height = size
        
        textbox = slide.shapes.add_textbox(left, top, width, height)
        text_frame = textbox.text_frame
        text_frame.text = description
        
        textbox.line.color.rgb = RGBColor(150, 150, 150)
        textbox.line.width = Pt(1)
        textbox.fill.solid()
        textbox.fill.fore_color.rgb = RGBColor(250, 250, 250)
        
        return textbox
    return None

# Assume user request (actually determined by AI)
user_request = "Please create a presentation"  # In this case, no images
# user_request = "Please create a presentation with logo placement"  # In this case, with images

# Determine need for images
add_images = should_add_image_placeholder(user_request)

prs = Presentation()

# Slide creation
slide_configs = [
    {"type": "title", "title": "Presentation", "subtitle": "Sample Corporation"},
    {"type": "content", "title": "Overview", "content": "Key points"},
    {"type": "content", "title": "Details", "content": "Detailed explanation"}
]

for config in slide_configs:
    if config["type"] == "title":
        slide = prs.slides.add_slide(prs.slide_layouts[0])
        slide.shapes.title.text = config["title"]
        slide.placeholders[1].text = config["subtitle"]
        
        # Conditionally add logo placeholder
        add_image_placeholder_if_needed(
            slide,
            position=(Inches(7), Inches(4.5)),
            size=(Inches(2), Inches(1)),
            description="[Logo]\nlogo.png\n200x100px",
            user_wants_image=add_images
        )
        
    elif config["type"] == "content":
        slide = prs.slides.add_slide(prs.slide_layouts[1])
        slide.shapes.title.text = config["title"]
        if config["content"]:
            slide.placeholders[1].text = config["content"]

# Save
pptx_io = BytesIO()
prs.save(pptx_io)
pptx_io.seek(0)
js.pptx_base64_data = base64.b64encode(pptx_io.read()).decode('utf-8')
```

---

## **✅ Implementation Checklist**

### **Required Confirmation Items**
- [ ] python-pptx installation process exists
- [ ] Python code starts from the left edge
- [ ] `import js` is included
- [ ] Assignment to `js.pptx_base64_data` exists
- [ ] Error overlay HTML is complete

### **Placeholder Confirmation Items**
- [ ] Don't place image placeholders by default
- [ ] Not directly embedding images (required)
- [ ] Place placeholders only when user explicitly requests images
- [ ] Process as placeholder even when image files are attached
- [ ] Placeholders include necessary information (when used)

### **Operation Confirmation Items**
- [ ] Page loads normally
- [ ] Download button becomes enabled
- [ ] PPTX file can be downloaded
- [ ] Overlay displays on error
- [ ] Error copy button functions

---

## **📊 python-pptx Reference**

### **Slide Layouts**
| Index | Layout Name | Use | Placeholders |
|-------|-------------|-----|-------------|
| 0 | Title Slide | Cover | title, subtitle |
| 1 | Title and Content | Standard | title, content |
| 2 | Section Header | Chapter break | title, content |
| 3 | Two Content | Comparison | title, contentx2 |
| 4 | Comparison | Contrast display | title, contentx2 |
| 5 | Blank | Free placement | None |
| 6 | Content and Caption | With explanation | title, content, caption |

### **Frequently Used Methods**
```python
# Create presentation
prs = Presentation()

# Add slide
slide = prs.slides.add_slide(prs.slide_layouts[n])

# Set text
slide.shapes.title.text = "Title"
slide.placeholders[1].text = "Content"

# Add text box
textbox = slide.shapes.add_textbox(left, top, width, height)
textbox.text_frame.text = "Text"

# Font settings
from pptx.util import Pt
paragraph = textbox.text_frame.paragraphs[0]
paragraph.font.size = Pt(24)
paragraph.font.bold = True

# Color settings
from pptx.dml.color import RGBColor
paragraph.font.color.rgb = RGBColor(255, 0, 0)

# Alignment settings
from pptx.enum.text import PP_ALIGN
paragraph.alignment = PP_ALIGN.CENTER

# Border settings
textbox.line.color.rgb = RGBColor(0, 0, 0)
textbox.line.width = Pt(1)

# Background color settings
textbox.fill.solid()
textbox.fill.fore_color.rgb = RGBColor(255, 255, 255)
```

### **Handling Units**
```python
from pptx.util import Inches, Pt, Cm

# Inch units
left = Inches(1)  # 1 inch

# Point units (font size, etc.)
size = Pt(24)  # 24 points

# Centimeter units
width = Cm(10)  # 10cm
```

---

## **🚀 Quick Start**

### **Fastest Implementation (3 Steps)**

1. **Copy HTML template**
2. **Change title** (2 places: `<title>` and `<h1>`)
3. **Define slide content in Python code**

```python
# Minimal change example (simple presentation without images)
prs = Presentation()
slide = prs.slides.add_slide(prs.slide_layouts[0])
slide.shapes.title.text = "Your title here"
slide.placeholders[1].text = "Your subtitle here"
# ... Rest is save process (as per template)
```

### **When Images Are Needed**

Only when user explicitly requests images:

1. **Place placeholder**
```python
# When instructed "Please place images"
textbox = slide.shapes.add_textbox(
    Inches(5), Inches(2),  # Position
    Inches(4), Inches(3)   # Size
)
textbox.text_frame.text = "[Image]\nPlace image here"
```

2. **User adds images in PowerPoint**
   - Open downloaded PPTX file
   - Delete placeholder
   - Insert actual image

---

## **📝 Update History**

- **v4.0** (2025-09-17)
  - Changed image placeholders to **optional feature**
  - Changed specification to not place placeholders by default
  - Create placeholders only with explicit instructions
  - Always process as placeholders even when image files are attached (no direct embedding)
  - Adopted same design philosophy as CSV processing in XLSX-RECIPE.md

- **v3.4** (2025-09-17)
  - Clarified image processing policy (all placeholders)
  - Revived detailed explanations from v3.1.5
  - Completed error handling procedures
  - Added recommended placement patterns

- **v3.3** (2025-09-17)
  - Reorganized implementation rules by priority
  - Added error message examples
  - Logically optimized structure

- **v3.1.5** (2025-09-17)
  - Enhanced error UI functionality
  - Implemented one-click copy feature

- **v3.1** (2025-09-17)
  - Clarified required elements
  - Strengthened error prevention

---

## **📄 License**

MIT License - Free to use, modify, redistribute, and use commercially

---

## **🆘 Support**

### **Troubleshooting When Issues Occur**

1. **Copy error message**
   - Use "Copy Full Error" button in error overlay

2. **Paste and consult with AI**
   - Paste copied content into chat area
   - AI will analyze cause and provide fixed version

3. **Check with checklist**
   - Confirm no required items are missing
   - Especially python-pptx installation process

4. **Reference implementation examples**
   - Start with Level 1 minimal implementation
   - Add features gradually

### **Frequently Asked Questions**

**Q: How do I add images?**
A: If you need images, explicitly instruct with "Please place images". Image placeholders are not placed by default.

**Q: I want to automatically create image spaces like the previous version**
A: If you instruct "Please include image placeholders in each slide", it will work similar to v3.x.

**Q: Image files were attached but not embedded**
A: By specification, images are always processed as placeholders. This keeps file size light and allows free image changes later.

**Q: I want to change the appearance of placeholders**
A: You can freely customize with text box `line` and `fill` properties.

**Q: I want to create special layouts**
A: Use Layout 5 (Blank) and place freely with `add_textbox()`.

---

## **v3.x -> v4.0 Migration Guide**

### **Main Changes**
- Image placeholders are no longer placed by default
- Explicit instructions like "place images" are required if images are needed

### **About Compatibility**
- For same behavior as before: Instruct "include image placeholders"
- For simple documents: No special instructions needed (new default)

### **Benefits**
- Cleaner and simpler presentations by default
- Image spaces added only when needed
- Always lightweight file size

---

**This completes the full text of Recipe v4.0. Detailed explanations and examples are included to ensure AI correctly understands and creates appropriate PPTX generation pages aligned with user intent.**
