# **Math-RECIPE v2.0 (Enhanced Error Handling Version)**
**Mathematical Formula PNG Auto-Generation Page Recipe**

---

## **📌 About This Recipe**

A directive for generative AI to interpret user formula instructions and automatically create a **single HTML file** that can generate and download high-quality **mathematical formula PNG files** in the browser.

### **v2.0 Changes (Important)**
- **Error overlay feature implementation**: Migrated the high-performance error reporting feature from `PPTX-RECIPE`. When an error occurs, detailed information is displayed, and you can copy the content with one click to request AI fixes.
- 🐛 **Improved stability**: Removed unnecessary font cache rebuilding process that was causing errors in previous versions.

### **Main Features from v1.9**
- 🚀 **Latest runtime environment**: Using Pyodide `v0.28.2` for fast initialization and performance.
- 🖋️ **Font selection**: Choose from multiple fonts including 'Computer Modern', 'STIX', 'DejaVu Sans'.
- 🎨 **Full customization**: Freely configure text color, background color, transparency, etc.

---

## **🎯 AI Implementation Steps**

### **Step 1: Interpreting User Input**

AI should process formulas following these steps:

1.  **Determine input format**
    ```
    - TeX format (contains \ or $) → Use as is
    - Natural language → Convert to TeX format
    - Other formula notations → Convert to TeX format
    ```

2.  **TeX format conversion examples**
    ```
    Input: "x squared plus y squared equals 1"
    Conversion: x^2 + y^2 = 1
    ```

3.  **Embedding conversion results**
    ```html
    <!-- Insert conversion result in this part of HTML -->
    <textarea id="formula-input">TeX format formula here</textarea>
    ```

### **Step 2: HTML File Generation**

Embed the converted TeX format into the specified location in the HTML template.

---

## **🚨 Implementation Rules (v2.0)**

### **🔴 Absolute Prohibitions (Do Not Change or Delete)**

1.  **Loading Matplotlib**
    ```javascript
    await pyodide.loadPackage("matplotlib");
    ```

2.  **Loading Pyodide**
    ```html
    <script src="https://cdn.jsdelivr.net/pyodide/v0.28.2/full/pyodide.js"></script>
    ```
3.  **Error Overlay HTML/CSS/JS**
    - HTML structure for error display (`<div id="error-overlay">`)
    - CSS classes for error display (`.error-overlay`, `.error-card`, etc.)
    - JavaScript code for error handling including the `showError` function

### **🟡 Required Rules**

1.  **Explicit Python code execution**
    Inside the `main` function, **after** Pyodide initialization and library loading is complete, load and execute the entire Python code inside the `<script>` tag.
    ```javascript
    const pythonCode = document.getElementById('python-code').textContent;
    await pyodide.runPythonAsync(pythonCode);
    await runGeneration(true); // Initial execution
    ```

---

## **📝 HTML Template (v2.0 Enhanced Error Handling Version)**

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <title>Mathematical Formula PNG Generator v2.0</title>

    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; }
        button:disabled { cursor: not-allowed; opacity: 0.6; }
        .spinner { border: 4px solid rgba(0, 0, 0, 0.1); width: 36px; height: 36px; border-radius: 50%; border-left-color: #3b82f6; animation: spin 1s ease infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        .preview-area { min-height: 240px; background: linear-gradient(to bottom, #ffffff, #fafafa); border: 2px solid #e5e7eb; border-radius: 16px; padding: 24px; display: flex; align-items: center; justify-content: center; margin-bottom: 24px; box-shadow: inset 0 2px 4px rgba(0,0,0,0.06); overflow: auto; position: relative; }
        .preview-area.transparent-bg { background: repeating-conic-gradient(#f3f4f6 0% 25%, transparent 0% 50%) 50% / 20px 20px; }
        .settings-section { background: #f9fafb; border: 1px solid #e5e7eb; border-radius: 12px; padding: 16px; margin-top: 20px; }
        .settings-header { display: flex; align-items: center; justify-content: space-between; cursor: pointer; user-select: none; }
        
        /* Error overlay (migrated from PPTX-RECIPE) */
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
<body class="bg-gradient-to-br from-blue-50 via-indigo-50 to-purple-50 min-h-screen p-4">

    <div class="max-w-4xl mx-auto py-8">
        <div class="bg-white rounded-2xl shadow-xl p-8">
            <div class="text-center mb-8">
                <h1 class="text-3xl font-bold text-gray-800 mb-2">Mathematical Formula PNG Generator</h1>
                <p class="text-gray-600">Convert beautiful formulas to PNG format with free colors and fonts</p>
                <p class="text-xs text-gray-500 mt-2">v2.0 - Pyodide v0.28.2</p>
            </div>

            <div class="mb-6">
                <label class="block mb-2 text-sm font-semibold text-gray-700" for="formula-input">TeX format formula</label>
                <textarea id="formula-input" class="w-full p-3 border-2 border-gray-200 rounded-lg font-mono text-sm focus:border-blue-500 focus:ring-1 focus:ring-blue-500 transition" rows="3" placeholder="Example: \int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}"></textarea>
            </div>

            <div id="status-container" class="mb-6 h-12 flex items-center justify-center">
                <p id="status-message" class="text-gray-600">Preparing...</p>
            </div>

            <div class="preview-area" id="preview-area">
                <div class="preview-placeholder">
                    <div id="loading-spinner" class="spinner mx-auto mb-4"></div>
                    <p>Generating formula...</p>
                </div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                <button id="regenerate-button" class="w-full bg-indigo-600 text-white font-bold py-3 px-4 rounded-lg hover:bg-indigo-700 transition" disabled>⟳ Regenerate</button>
                <button id="download-button" class="w-full bg-blue-600 text-white font-bold py-3 px-4 rounded-lg hover:bg-blue-700 transition" disabled>⬇ Download PNG</button>
            </div>
            
            <details class="settings-section">
                <summary class="settings-header"><span class="font-semibold text-gray-800">Advanced Settings</span></summary>
                <div class="mt-4 space-y-6">
                    <div>
                        <h3 class="font-semibold text-gray-700 mb-2 border-b pb-2">🎨 Color Settings</h3>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-2">
                            <div>
                                <label class="block text-sm font-medium text-gray-600">Text Color</label>
                                <div class="flex items-center gap-2 mt-1">
                                    <input type="color" id="text-color-picker" value="#000000" class="h-8 w-8 p-0 border-0 rounded cursor-pointer">
                                    <input type="text" id="text-color" value="#000000" class="w-full px-2 py-1 border border-gray-300 rounded-md text-sm font-mono">
                                </div>
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-600">Background Color</label>
                                <div class="flex items-center gap-2 mt-1">
                                    <input type="color" id="bg-color-picker" value="#ffffff" class="h-8 w-8 p-0 border-0 rounded cursor-pointer">
                                    <input type="text" id="bg-color" value="#ffffff" class="w-full px-2 py-1 border border-gray-300 rounded-md text-sm font-mono">
                                </div>
                            </div>
                        </div>
                         <div class="flex items-center gap-2 mt-4">
                            <input type="checkbox" id="transparent-bg" class="h-4 w-4 rounded border-gray-300 text-blue-600 focus:ring-blue-500">
                            <label for="transparent-bg" class="text-sm text-gray-700">Make background transparent</label>
                        </div>
                    </div>
                     <div>
                        <h3 class="font-semibold text-gray-700 mb-2 border-b pb-2">⚙️ Basic Settings</h3>
                        <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mt-2">
                            <div>
                                <label for="font-size" class="block text-sm font-medium text-gray-600">Font Size</label>
                                <input type="number" id="font-size" value="24" class="w-full mt-1 px-2 py-1 border border-gray-300 rounded-md text-sm">
                            </div>
                            <div>
                                <label for="dpi" class="block text-sm font-medium text-gray-600">Resolution (DPI)</label>
                                <input type="number" id="dpi" value="200" class="w-full mt-1 px-2 py-1 border border-gray-300 rounded-md text-sm">
                            </div>
                             <div class="col-span-2">
                                <label for="font-type" class="block text-sm font-medium text-gray-600">Font Type</label>
                                <select id="font-type" class="w-full mt-1 px-2 py-1 border border-gray-300 rounded-md text-sm">
                                    <option value="cm" selected>Computer Modern (serif)</option>
                                    <option value="stix">STIX (serif)</option>
                                    <option value="stixsans">STIX Sans (sans-serif)</option>
                                    <option value="dejavusans">DejaVu Sans (sans-serif)</option>
                                    <option value="dejavuserif">DejaVu Serif (serif)</option>
                                </select>
                            </div>
                        </div>
                    </div>
                </div>
            </details>
        </div>
    </div>

    <!-- Error overlay (migrated from here) -->
    <div id="error-overlay" class="error-overlay" role="dialog" aria-modal="true" aria-labelledby="error-title">
        <div class="error-card">
            <div class="error-title" id="error-title">An error has occurred: Please paste the following error message in AI chat area to request a fix</div>
            <div class="error-subtitle">Copy the entire error message and send it directly to the AI. It will identify the cause and suggest a patch.</div>
            <div class="error-actions">
                <button id="copy-error" class="copy-btn" aria-label="Copy error message">Copy entire error</button>
                <button id="close-error" class="copy-btn" aria-label="Close this error display">Close</button>
            </div>
            <pre id="error-text" class="error-pre" tabindex="0" aria-live="polite"></pre>
        </div>
    </div>
    <!-- (migrated up to here) -->

    <script type="text/python" id="python-code">
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt
from io import BytesIO
import base64
import js

def hex_to_rgb(hex_color):
    hex_color = hex_color.lstrip('#')
    if len(hex_color) == 3: hex_color = ''.join([c*2 for c in hex_color])
    try: return tuple(int(hex_color[i:i+2], 16) / 255.0 for i in (0, 2, 4))
    except (ValueError, TypeError): return (0, 0, 0)

def create_math_png(formula, options):
    fontsize = options.get('fontsize', 24)
    dpi = options.get('dpi', 200)
    fontset = options.get('fontset', 'cm')
    text_color = options.get('text_color', '#000000')
    bg_color = options.get('bg_color', '#ffffff')
    transparent = options.get('transparent', False)

    if not formula.strip().startswith('$'):
        formula = '$' + formula + '$'
    
    plt.rcParams['mathtext.fontset'] = fontset
    plt.rcParams['font.size'] = fontsize
    
    fig_facecolor = 'none' if transparent else hex_to_rgb(bg_color)
    fig = plt.figure(figsize=(10, 2), facecolor=fig_facecolor)
    ax = fig.add_subplot(111)
    ax.set_facecolor(fig_facecolor)
    ax.axis('off')

    try:
        ax.text(0.5, 0.5, formula, ha='center', va='center', transform=ax.transAxes,
                fontsize=fontsize, color=hex_to_rgb(text_color))
    except Exception as e:
        ax.text(0.5, 0.5, f"Error: {str(e)}", ha='center', va='center', transform=ax.transAxes,
                fontsize=12, color='red')
    
    png_io = BytesIO()
    plt.savefig(png_io, format='png', dpi=dpi, bbox_inches='tight', pad_inches=0.2,
                transparent=transparent, facecolor=fig.get_facecolor())
    plt.close(fig)
    
    png_io.seek(0)
    return png_io.read()

def generate_formula():
    formula_input = js.document.getElementById('formula-input')
    formula = formula_input.value if formula_input.value else r"\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}"
    
    options = {
        'fontsize': int(js.document.getElementById('font-size').value),
        'dpi': int(js.document.getElementById('dpi').value),
        'fontset': js.document.getElementById('font-type').value,
        'text_color': js.document.getElementById('text-color').value,
        'bg_color': js.document.getElementById('bg-color').value,
        'transparent': js.document.getElementById('transparent-bg').checked
    }
    
    png_bytes = create_math_png(formula, options)
    png_base64 = base64.b64encode(png_bytes).decode('utf-8')
    
    js.png_base64_data = png_base64
    js.png_preview_src = f"data:image/png;base64,{png_base64}"
    
    return True
    </script>
    
    <script src="https://cdn.jsdelivr.net/pyodide/v0.28.2/full/pyodide.js"></script>
    <script type="module">
        let pyodideInstance = null;
        
        const statusMessage = document.getElementById('status-message');
        const downloadButton = document.getElementById('download-button');
        const regenerateButton = document.getElementById('regenerate-button');
        const previewArea = document.getElementById('preview-area');
        
        // ELEMENTS for error overlay (migrated from here)
        const overlay = document.getElementById('error-overlay');
        const errorText = document.getElementById('error-text');
        const copyBtn = document.getElementById('copy-error');
        const closeBtn = document.getElementById('close-error');

        // Helper function for error display
        function showError(error) {
            console.error("An error occurred:", error);
            try {
                const details = (error && (error.stack || error.message || String(error))) || 'Unknown error';
                errorText.textContent = details.trim();
            } catch(_) {
                errorText.textContent = 'Unknown error';
            }
            overlay.style.display = 'flex';
        }
        
        // Event listeners for error UI
        copyBtn?.addEventListener('click', async () => {
            try {
                await navigator.clipboard.writeText(errorText.textContent || '');
                copyBtn.textContent = 'Copied';
                setTimeout(() => (copyBtn.textContent = 'Copy entire error'), 1200);
            } catch (_) {
                // Fallback
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
        // (migrated up to here)


        async function main() {
            try {
                statusMessage.textContent = 'Initializing runtime environment...';
                const pyodide = await loadPyodide({ indexURL: "https://cdn.jsdelivr.net/pyodide/v0.28.2/full/" });
                pyodideInstance = pyodide;

                statusMessage.textContent = 'Loading libraries...';
                await pyodide.loadPackage("matplotlib");

                statusMessage.textContent = 'Preparing Python functions...';
                const pythonCode = document.getElementById('python-code').textContent;
                await pyodide.runPythonAsync(pythonCode);

                await runGeneration(true);

            } catch (error) {
                // Updated initialization error handling
                showError(error);
                statusMessage.textContent = 'Initialization failed.';
                previewArea.innerHTML = `<p class="text-red-500">An error occurred. Please check the error display for details.</p>`;
            }
        }
        
        async function runGeneration(isInitial = false) {
            if (!pyodideInstance) return;
            try {
                statusMessage.textContent = isInitial ? 'Generating formula...' : 'Regenerating formula...';
                previewArea.innerHTML = `<div class="preview-placeholder"><div class="spinner mx-auto mb-4"></div><p>${statusMessage.textContent}</p></div>`;
                regenerateButton.disabled = true;
                downloadButton.disabled = true;
                
                await pyodideInstance.globals.get('generate_formula')();
                
                const previewSrc = window.png_preview_src;
                if (previewSrc) {
                    previewArea.innerHTML = `<img src="${previewSrc}" alt="Generated formula" class="max-w-full h-auto block mx-auto">`;
                } else {
                    throw new Error("PNG generation returned no data.");
                }
                
                statusMessage.textContent = '✓ Generation complete';
            } catch (error) {
                // Updated generation error handling
                showError(error);
                statusMessage.textContent = 'Generation failed';
                previewArea.innerHTML = `<p class="text-red-500">Generation error: ${error.message}</p>`;
            } finally {
                regenerateButton.disabled = false;
                downloadButton.disabled = false;
            }
        }

        function downloadFile(base64Data, fileName) {
            const byteCharacters = atob(base64Data);
            const byteNumbers = new Array(byteCharacters.length);
            for (let i = 0; i < byteCharacters.length; i++) {
                byteNumbers[i] = byteCharacters.charCodeAt(i);
            }
            const byteArray = new Uint8Array(byteNumbers);
            const blob = new Blob([byteArray], { type: 'image/png' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = fileName;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        }
        
        // --- Event Listeners Setup ---
        regenerateButton.addEventListener('click', () => runGeneration(false));
        downloadButton.addEventListener('click', () => {
            if (window.png_base64_data) {
                downloadFile(window.png_base64_data, `formula-${Date.now()}.png`);
            }
        });
        const textColorPicker = document.getElementById('text-color-picker');
        const textColorInput = document.getElementById('text-color');
        const bgColorPicker = document.getElementById('bg-color-picker');
        const bgColorInput = document.getElementById('bg-color');
        textColorPicker.addEventListener('input', (e) => textColorInput.value = e.target.value);
        textColorInput.addEventListener('change', (e) => textColorPicker.value = e.target.value);
        bgColorPicker.addEventListener('input', (e) => bgColorInput.value = e.target.value);
        bgColorInput.addEventListener('change', (e) => bgColorPicker.value = e.target.value);

        const transparentBg = document.getElementById('transparent-bg');
        transparentBg.addEventListener('change', () => {
            previewArea.classList.toggle('transparent-bg', transparentBg.checked);
            bgColorPicker.disabled = transparentBg.checked;
            bgColorInput.disabled = transparentBg.checked;
        });

        window.onload = main;
    </script>
</body>
</html>
```

---

## **🤖 AI Implementation Guide**

### **Steps for AI to Use This Recipe**

1.  **Receive formula from user**
    Receive natural language instructions like "graph the integral of x" or direct TeX format input from the user.

2.  **Convert to TeX format**
    If the input is natural language, convert it to an appropriate TeX format string using AI capabilities.
    -   Input: `"x squared"` → Conversion: `"x^2"`
    -   Input: `"sigma from k=1 to n"` → Conversion: `\sum_{k=1}^{n}`

3.  **Embed in HTML template**
    Insert the converted TeX format string into the `<textarea>` element in the HTML template above.
    ```html
    <textarea id="formula-input" ... >Insert converted TeX format here</textarea>
    ```

4.  **Provide the completed HTML file**
    Provide the final completed single HTML file to the user.

---

## **📄 License**

MIT License - Free to use, modify, redistribute, and use commercially
