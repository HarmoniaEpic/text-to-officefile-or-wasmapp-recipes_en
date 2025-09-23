# **ASMSCRIPT-OPTIMIZER-RECIPE v1.4.3**
**AssemblyScript WebAssembly Compiler-Integrated Single HTML Application Generation Recipe**

---

## **📌 About This Recipe**

An instruction manual for Generative AI to interpret user requirements and automatically generate a **high-performance single HTML file** that compiles and executes AssemblyScript code to WebAssembly within the browser. This recipe is designed to be independent of specific AI environments and can be utilized by any generative AI system.

---

### **Core Features**
- 🚀 **Auto-initialization**: Automatically starts compilation and execution on page load
- 🔧 **15+ optimization options**: Detailed compilation control
- 📦 **Modular architecture**: Supports diverse use cases
- ⚡ **CDN-based**: Browser-only, no external tools required
- 📟 **Command-line preview**: Real-time display of CLI commands (v1.4.1 new feature)
- ⚠️ **Option change notification**: Visual feedback when recompilation is needed (v1.4.2 new feature)

---

## **🎯 AI Implementation Steps**

### **Step 0: Verify User Requirements (Mandatory)**

The AI must first perform the following checks and **always stop work** to ask the user for confirmation if requirements are unclear:

#### **Requirement Clarity Checklist**
1. **Are specific processing requirements specified?**
   - ❌ Unclear example: "Make an app with AssemblyScript"
   - ✅ Clear example: "Make an app that calculates prime numbers with AssemblyScript"

2. **Are the purpose and goals clear?**
   - ❌ Unclear example: "Want to optimize"
   - ✅ Clear example: "Want to speed up grayscale conversion of images"

3. **Can input/output specifications be inferred?**
   - ❌ Unclear example: "Process data"
   - ✅ Clear example: "Load CSV file and display graph"

#### **Work Suspension and Inquiry Template**

```markdown
❓ **Please tell me what you want to implement with AssemblyScript**

I apologize. Could you please tell me specifically what kind of processing you want to implement?
With the following information, I can generate an optimal WebAssembly application:

**1. What do you want to create?** (Select from examples or describe freely)
□ Computational processing (numerical calculations, statistics, algorithm execution, etc.)
□ Graphics (Canvas drawing, animation, games, etc.)
□ Data processing (filtering, transformation, analysis, etc.)
□ Interactive tools (editors, simulators, etc.)
□ Other: [Please describe specifically]

**2. Specific processing details**
Examples:
- "Want to quickly calculate 1000 prime numbers"
- "Want to create a ball physics simulation"
- "Want to apply filters to images"
- "Want to process audio in real-time"

**3. Expected optimization direction** (Optional)
□ Prioritize execution speed
□ Minimize file size
□ Reduce memory usage
□ Balanced approach

Please let me know your specific requirements.
```

### **Step 0.5: Dynamic AssemblyScript Version Retrieval**

Before generating HTML, the AI should retrieve the latest stable version using the following procedure:

#### **Dynamic Version Retrieval Implementation**

```javascript
// Version management system
const VersionManager = {
    // Cache mechanism
    cache: {
        get() {
            try {
                const cached = sessionStorage.getItem('asc_version_cache');
                if (cached) {
                    const data = JSON.parse(cached);
                    // Cache valid for 1 hour
                    if (Date.now() - data.timestamp < 3600000) {
                        return data.version;
                    }
                }
            } catch (e) {}
            return null;
        },
        set(version) {
            try {
                sessionStorage.setItem('asc_version_cache', JSON.stringify({
                    version,
                    timestamp: Date.now()
                }));
            } catch (e) {}
        }
    },
    
    async getLatestVersion() {
        // Check cache
        const cached = this.cache.get();
        if (cached) {
            console.log(`[Version] Using cached: ${cached}`);
            return cached;
        }
        
        // jsDelivr-priority retrieval strategy
        const strategies = [
            { name: 'jsDelivr API', fn: () => this.fetchFromJsDelivr() },
            { name: 'jsDelivr Package', fn: () => this.fetchJsDelivrPackageJson() },
            { name: 'Unpkg', fn: () => this.fetchFromUnpkg() }
        ];
        
        for (const strategy of strategies) {
            try {
                console.log(`[Version] Trying ${strategy.name}...`);
                const version = await Promise.race([
                    strategy.fn(),
                    this.timeout(3000)
                ]);
                
                if (version && this.isValidStableVersion(version)) {
                    console.log(`[Version] Success: ${version}`);
                    this.cache.set(version);
                    return version;
                }
            } catch (e) {
                console.warn(`[Version] ${strategy.name} failed`);
            }
        }
        
        // Fallback
        return "0.28.8";
    },
    
    async fetchFromJsDelivr() {
        const response = await fetch('https://data.jsdelivr.com/v1/package/npm/assemblyscript');
        const data = await response.json();
        const stable = data.versions.filter(v => this.isValidStableVersion(v));
        return stable[0];
    },
    
    async fetchJsDelivrPackageJson() {
        const response = await fetch('https://cdn.jsdelivr.net/npm/assemblyscript@latest/package.json');
        const data = await response.json();
        return data.version;
    },
    
    async fetchFromUnpkg() {
        const response = await fetch('https://unpkg.com/assemblyscript@latest/package.json');
        const data = await response.json();
        return data.version;
    },
    
    isValidStableVersion(version) {
        const semverRegex = /^(\d+)\.(\d+)\.(\d+)$/;
        if (!version.match(semverRegex)) return false;
        
        const preRelease = ['alpha', 'beta', 'rc', 'dev', '-'];
        return !preRelease.some(indicator => 
            version.toLowerCase().includes(indicator)
        );
    },
    
    timeout(ms) {
        return new Promise((_, reject) => 
            setTimeout(() => reject(new Error('Timeout')), ms)
        );
    }
};
```

### **Step 1: Analyze User Requirements**

The AI processes user requirements through the following steps:

1. **Identify Use Case**
   ```
   - Visual (Canvas/WebGL usage) → RenderModule
   - Computational (numerical output) → ComputeModule
   - Data processing (transformation/analysis) → DataModule
   - Interactive (user input) → InteractiveModule
   ```

2. **Generate/Select AssemblyScript Code**
   ```typescript
   // Example: For physics simulation
   export function update(dt: f32): void {
     // Physics calculation logic
   }
   ```

3. **Determine Appropriate Module Type**
   - Required WebAssembly export functions
   - Import requirements (memory, environment variables, etc.)
   - Execution frequency (animation loop or on-demand)

### **Step 2: Generate AppModule**

Based on the selected use case, generate the AppModule section:

```javascript
const AppModule = {
    name: "Use Case Name",
    sourceCode: `AssemblyScript code`,
    defaultOptions: { /* Optimal compilation options */ },
    execute: (exports) => { /* Execution logic */ },
    render: (data) => { /* Display update */ }
};
```

### **Step 3: Assemble HTML File**

1. Load core structure template
2. Integrate dynamic version retrieval functionality
3. Embed AppModule
4. Add UI customization sections as needed
5. Output as single HTML file

---

## **🚨 Implementation Rules (v1.4.2 UX Improved)**

### **🔴 Absolutely Prohibited**

1. **Prohibition of Direct Import Usage**
   ```javascript
   // ❌ Absolutely prohibited (will not work)
   import("https://cdn.jsdelivr.net/npm/assemblyscript@latest/dist/asc/index.js");
   
   // ✅ Always use official-compatible web.js method
   loadScriptTag("https://cdn.jsdelivr.net/npm/assemblyscript@0.28.8/dist/web.js");
   await import("assemblyscript/asc");
   ```

2. **Prohibition of @latest Tag Usage**
   ```javascript
   // ❌ Absolutely prohibited
   const url = "assemblyscript@latest/dist/web.js";
   
   // ✅ Always specify concrete version
   const version = await VersionManager.getLatestVersion(); // Dynamic retrieval
   const url = `assemblyscript@${version}/dist/web.js`;
   ```

3. **Prohibition of Single CDN Dependency**
   ```javascript
   // ❌ Single CDN only is prohibited
   const url = "https://cdn.jsdelivr.net/...";
   
   // ✅ Always use 3-CDN fallback
   const cdnProviders = ['jsDelivr', 'UNPKG', 'esm.sh'];
   ```

### **🟡 Mandatory Rules (v1.4.3)**

1. **Use Official-Compatible web.js Method**
   - Dynamically insert web.js script tag
   - Wait for import map configuration
   - Import "assemblyscript/asc"

2. **3-Stage CDN Fallback**
   - 1st priority: jsDelivr (fast, officially recommended)
   - 2nd priority: UNPKG (stable, npm mirror)
   - 3rd priority: esm.sh (experimental, last resort)

3. **Timeout Protection**
   - 10-second limit for each CDN attempt
   - Script tag cleanup

4. **Import Map Wait Process**
   - Confirm global variables
   - Maximum 2-second wait time

5. **Metadata Recording**
   - CDN provider used
   - Load time
   - Number of attempts
   - Version source

6. **Command-Line Preview Display**
   - Place at top of WebAssembly tab
   - Real-time updates
   - Reflect all options

7. **Visual Feedback for Option Changes** (v1.4.2 New)
   - Highlight recompile button
   - Keep menu open during preset changes
   - Separate UI operations from compilation processing

### **🟢 Recommendations**

1. **Debug Information Output**
   - Log each CDN attempt
   - Success/failure details
   - Performance measurements

2. **Clear Error Messages**
   - Which CDN failed
   - Network issue or version issue

3. **User Feedback**
   - Progress bar updates
   - Display currently attempting CDN
   - Notifications on option changes

---

## **📟 Command-Line Preview Feature (v1.4.3 Compatible)**

### **Overview**

Displays the CLI command corresponding to current option settings in real-time at the top of the WebAssembly tab. v1.4.2 removes side effects and improves performance.

### **Feature Specifications**

#### **Display Position**
- Top of WebAssembly tab (above Execution Control section)
- Always visible

#### **Display Content**
Example:
```bash
asc main.ts -o main.wasm --runtime minimal --optimize --optimizeLevel 3
```

#### **Update Timing**
- On option change (checkboxes, select boxes)
- On preset application
- On initialization complete

#### **v1.4.2 Improvements**
- Side-effect-free option collection
- Visual highlighting of recompile button
- Stable UX with menu staying open

### **Implementation Details (v1.4.3)**

```javascript
// ============================================
// Command-Line Preview Feature (v1.4.2 Fixed)
// ============================================
function updateCmdPreview() {
    // Collect options without side effects (don't change OptionsManager state)
    const options = {};
    
    // Checkboxes
    ['optimize', 'exportRuntime', 'noAssert', 'debug', 'sourceMap',
     'measure', 'validate', 'importMemory', 'sharedMemory',
     'exportTable', 'explicitStart', 'lowMemoryLimit'].forEach(opt => {
        const el = document.getElementById(`opt-${opt}`);
        if (el && el.checked) options[opt] = true;
    });
    
    // Select boxes
    ['optimizeLevel', 'shrinkLevel', 'runtime'].forEach(opt => {
        const el = document.getElementById(`opt-${opt}`);
        if (el && el.value) options[opt] = el.value;
    });
    
    const cmd = generateCmdLine(options);
    const preview = document.getElementById('cmd-preview');
    if (preview) {
        preview.textContent = cmd;
    }
    
    // v1.4.2: Record option changes and highlight button
    markOptionsChanged();
}

// v1.4.2: Visual feedback on option changes
function markOptionsChanged() {
    if (!optionsChanged) {
        optionsChanged = true;
        const btn = document.getElementById('recompileBtn');
        if (btn) {
            btn.classList.add('options-changed');
            // Change icon
            btn.innerHTML = '⚠️ <span data-i18n="button.recompile">Re-compile</span>';
        }
    }
}

function generateCmdLine(options) {
    const parts = ['asc', 'main.ts', '-o', 'main.wasm'];
    
    Object.entries(options).forEach(([key, value]) => {
        if (typeof value === 'boolean' && value) {
            parts.push(`--${key}`);
        } else if (typeof value !== 'boolean' && value !== '') {
            parts.push(`--${key}`, value.toString());
        }
    });
    
    return parts.join(' ');
}
```

---

## **📝 Memory Management and Data Processing in AssemblyScript (v1.4.0)**

### **Basic Memory Access Methods**

AssemblyScript provides memory access methods at two levels:

#### **1. High-Level Approach (Recommended)**

Managed memory access using TypedArray:

```typescript
// Using TypedArray (type-safe and intuitive)
let buffer = new Uint8ClampedArray(1024);
buffer[0] = 255;  // Automatic bounds checking

// Performance-optimized version
for (let i = 0; i < buffer.length; i++) {
    unchecked(buffer[i] = 128);  // Skip bounds checking
}
```

#### **2. Low-Level Approach (Fast)**

Direct memory access using built-in functions:

```typescript
// load/store built-in functions
store<u8>(ptr, value);         // Write 1 byte
let value = load<u8>(ptr);     // Read 1 byte

// Multi-byte operations
store<i32>(ptr, 0x12345678);   // Write 4 bytes
let int32 = load<i32>(ptr);    // Read 4 bytes
```

### **Memory Layout and Object Management**

#### **Managed Object Header Structure**

```
Offset     | Size  | Content
-----------|-------|------------------
-20        | usize | Memory manager info
-16        | usize | GC info 1
-12        | usize | GC info 2
-8         | u32   | Class ID (rtId)
-4         | u32   | Data size (rtSize)
0          |       | Payload start position
```

#### **Standard Data Type Memory Layout**

| Type | Class ID | Characteristics |
|------|----------|-----------------|
| ArrayBuffer | 1 | Raw byte array |
| String | 2 | UTF-16 string (same as JavaScript) |
| TypedArray | Dynamic | buffer + dataStart + byteLength |
| StaticArray | - | Fixed size, direct data storage |

### **Practical Memory Management Patterns**

#### **Pattern 1: Global Buffer (Reusable)**

```typescript
// Avoid frequent reallocation with global buffer
let globalBuffer: Uint8ClampedArray | null = null;

export function initBuffer(size: i32): void {
    globalBuffer = new Uint8ClampedArray(size);
}

export function getBufferPtr(): usize {
    return globalBuffer ? globalBuffer!.dataStart : 0;
}
```

#### **Pattern 2: Utilizing StaticArray (Fixed Size)**

```typescript
// StaticArray has no indirection, fast access
let lookupTable = new StaticArray<f32>(256);

// Initialization
for (let i = 0; i < 256; i++) {
    unchecked(lookupTable[i] = <f32>i / 255.0);
}
```

#### **Pattern 3: Optimization with unchecked**

```typescript
// Use only when safety is guaranteed
export function processArray(arr: Int32Array): i32 {
    let sum = 0;
    // Perform length check outside
    let len = arr.length;
    for (let i = 0; i < len; i++) {
        sum += unchecked(arr[i]);  // Skip bounds checking
    }
    return sum;
}
```

### **Importance of Type Conversion**

AssemblyScript does not allow implicit type conversion, so explicit casting is mandatory:

```typescript
// ❌ Error: Implicit conversion from f64 to f32
let value: f32 = 3.14;

// ✅ Correct: Explicit cast (f32 literal)
let value: f32 = <f32>3.14;
```

### **Data Exchange with Host**

Strategies for exchanging data between JavaScript and WebAssembly:

| Data Type | Strategy | Description |
|-----------|----------|-------------|
| Number | Pass by value | Automatic conversion |
| TypedArray | Copy | Copy via memory |
| String | Copy | Transfer as UTF-16 |
| Object (simple) | Copy | Copy field by field |
| Object (complex) | Reference | Pass pointer only |

### **Memory Management Best Practices**

1. **Prioritize TypedArray** - Type-safe managed environment
2. **Use load/store only when necessary** - When low-level control is needed
3. **Apply unchecked after measurement** - Verify operation first, then optimize
4. **Use StaticArray for fixed-size data** - Avoid indirection
5. **Reuse with global buffer** - Reduce GC load

---

### 🟠 Important Notes on Type Conversion

AssemblyScript has a strict type system that **does not allow any implicit type conversion**. In particular, since the default floating-point type corresponding to JavaScript's `number` is `f64`, conscious type conversion is essential when using `f32` for performance reasons.

#### 1. Basic Rule: Recommend Explicit Type Casting

**Current AssemblyScript does not support `f` suffix notation like `1.23f`**. The safest and most reliable way to convert from `f64` to `f32` is with explicit type casting using **`<f32>value`**.

| Situation | Solution | Notation (Example) |
| :--- | :--- | :--- |
| **Numeric literals** | **Explicit casting** recommended. | `const gravity: f32 = <f32>9.8;` |
| **Variable assignment** | Type inference may work, but casting is safer. | `let value: f32 = <f32>someF64Value;` |
| **`Math` functions** | Return value is always `f64`, so **casting is mandatory**. | `const sinValue: f32 = <f32>Math.sin(angle);` |
| **Operations** | Unify both sides of operator to `f32`. | `const result: f32 = val1 * <f32>0.5;` |

#### 2. Common Errors and Immediate Solutions

* **`Type 'f64' is not assignable to type 'f32'`**
    → Add **`<f32>` cast** before the `f64` value. (Example: `<f32>Math.PI`)

* **`Operator '*' cannot be applied to types 'f32' and 'f64'`**
    → Add **`<f32>` cast** to the `f64` side value to unify both sides to `f32`.

#### 3. Type Selection Guidelines (No Changes)

**Use f32 (Performance Priority)**
* Canvas/WebGL drawing
* Game physics
* Real-time processing
* When memory constraints exist

**Use f64 (Precision Priority)**
* Scientific/financial calculations
* When cumulative error is problematic
* JavaScript compatibility priority

#### 4. Pre-Code Generation Checklist

* [ ] Are `<f32>` casts applied to values that could be `f64` (numeric literals, `Math.*` function returns, etc.) where `f32` is expected?
* [ ] Are operations between different floating-point types being performed?
* [ ] Are types explicitly specified in constant definitions? (`const G: f32 = <f32>9.8`)

**Important**: The default for numeric literals in AssemblyScript is `f64`. When treating as `f32`, type-conscious coding is always necessary.

---

## **📄 Official Recommended Method Compatible Implementation (v1.3.1)**

### **Implementation Flowchart**

```
Start loading AssemblyScript compiler
    ↓
Dynamic version retrieval
    ├─ Check cache → Use if exists
    └─ New retrieval
        ├─ jsDelivr API
        ├─ jsDelivr package.json
        └─ UNPKG package.json
    ↓
Start CDN selection loop
    ↓
1. Try jsDelivr
    ├─ Load web.js → Set import map → Import asc
    ├─ Success → Start using
    └─ Failure → Next CDN
    ↓
2. Try UNPKG
    ├─ Load web.js → Set import map → Import asc
    ├─ Success → Start using
    └─ Failure → Next CDN
    ↓
3. Try esm.sh
    ├─ Load web.js → Set import map → Import asc
    ├─ Success → Start using
    └─ Failure → Display error
```

### **CDN Provider Configuration (v1.4.3)**

```javascript
const CDN_PROVIDERS = [
    {
        name: 'jsDelivr',
        baseUrl: 'https://cdn.jsdelivr.net/npm',
        characteristics: 'Fast, globally optimized, officially recommended by AssemblyScript'
    },
    {
        name: 'UNPKG',
        baseUrl: 'https://unpkg.com',
        characteristics: 'Official npm mirror, integrity guaranteed, stability focused'
    },
    {
        name: 'esm.sh',
        baseUrl: 'https://esm.sh',
        characteristics: 'Modern ESM delivery, experimental, final fallback'
    }
];
```

### **Detailed Loading Process Implementation**

```javascript
// CDN fallback-compatible loader (v1.4.3 3-CDN version)
async function loadCompilerWithCDNFallback() {
    const version = await VersionManager.getLatestVersion();
    
    for (let i = 0; i < CDN_PROVIDERS.length; i++) {
        const cdn = CDN_PROVIDERS[i];
        const startTime = performance.now();
        
        try {
            updateProgress(30 + (i * 23), `Loading compiler (${cdn.name})...`);
            
            // Build web.js URL
            const webJsUrl = `${cdn.baseUrl}/assemblyscript@${version}/dist/web.js`;
            console.log(`[Loader] Attempting ${cdn.name}: ${webJsUrl}`);
            
            // Load web.js
            await loadScriptTag(webJsUrl);
            
            // Wait for import map configuration
            await waitForImportMap();
            
            // Import assemblyscript/asc
            const asc = await import("assemblyscript/asc");
            const compiler = asc.default || asc;
            
            // Validate compiler
            await validateCompiler(compiler);
            
            // Update metadata
            const loadTime = Math.round(performance.now() - startTime);
            COMPILER_METADATA.cdnProvider = cdn.name;
            COMPILER_METADATA.loadTime = loadTime;
            COMPILER_METADATA.attempts = i + 1;
            COMPILER_METADATA.versionSource = 'Dynamic fetch';
            
            console.log(`[Loader] Success with ${cdn.name} in ${loadTime}ms`);
            updateProgress(100, 'Compiler ready');
            
            return compiler;
            
        } catch (error) {
            console.error(`[Loader] ${cdn.name} failed:`, error.message);
            
            // Clean up script tags
            cleanupFailedScripts();
        }
    }
    
    throw new Error(`All CDN providers failed for AssemblyScript ${version}`);
}

// Script tag loading function
function loadScriptTag(url) {
    return new Promise((resolve, reject) => {
        // Check existing AssemblyScript
        if (window.ASSEMBLYSCRIPT_VERSION) {
            console.log('[Loader] AssemblyScript already loaded');
            resolve();
            return;
        }
        
        const script = document.createElement('script');
        script.src = url;
        script.dataset.assemblyScript = 'loading';
        
        // Timeout setting (10 seconds)
        const timeout = setTimeout(() => {
            script.remove();
            reject(new Error(`Timeout loading: ${url}`));
        }, 10000);
        
        script.onload = () => {
            clearTimeout(timeout);
            script.dataset.assemblyScript = 'loaded';
            resolve();
        };
        
        script.onerror = () => {
            clearTimeout(timeout);
            script.remove();
            reject(new Error(`Failed to load: ${url}`));
        };
        
        document.head.appendChild(script);
    });
}

// Import map wait function
async function waitForImportMap() {
    const maxAttempts = 20;
    const waitTime = 100;
    
    for (let i = 0; i < maxAttempts; i++) {
        if (window.ASSEMBLYSCRIPT_VERSION && window.ASSEMBLYSCRIPT_IMPORTMAP) {
            console.log('[Loader] Import map ready');
            return;
        }
        await new Promise(resolve => setTimeout(resolve, waitTime));
    }
    
    throw new Error('Import map setup timeout');
}

// Clean up failed scripts
function cleanupFailedScripts() {
    const scripts = document.querySelectorAll('script[data-assembly-script]');
    scripts.forEach(script => {
        if (script.dataset.assemblyScript !== 'loaded') {
            script.remove();
        }
    });
    
    // Also clear global variables
    delete window.ASSEMBLYSCRIPT_VERSION;
    delete window.ASSEMBLYSCRIPT_IMPORTMAP;
}
```

---

## **🔧 Compilation Option Specifications**

### **Basic Options**

| Option | Default | Description |
|--------|---------|-------------|
| `--optimize` | false | Enable Binaryen optimization |
| `--optimizeLevel` | 3 | Optimization level (0-3) |
| `--shrinkLevel` | 0 | Size reduction level (0-2) |
| `--runtime` | minimal | Runtime selection (stub/minimal/incremental) |
| `--debug` | false | Generate debug information |
| `--sourceMap` | false | Generate source map |
| `--noAssert` | false | Disable assertions |

### **Advanced Options**

| Option | Description | Use Case |
|--------|-------------|----------|
| `--importMemory` | Import WebAssembly memory | When using shared memory |
| `--sharedMemory` | Use SharedArrayBuffer | Multi-threaded processing |
| `--exportRuntime` | Export runtime API | Advanced memory management |
| `--explicitStart` | Explicit _start function call | Initialization control |
| `--lowMemoryLimit` | Optimization for low memory environments | Embedded browsers |

### **Preset Definitions**

```javascript
const PRESETS = {
    simple: {
        optimize: false,
        runtime: 'minimal'
    },
    debug: {
        optimize: false,
        optimizeLevel: '0',
        debug: true,
        sourceMap: true,
        measure: true
    },
    release: {
        optimize: true,
        optimizeLevel: '3',
        shrinkLevel: '1',
        noAssert: true
    },
    minimal: {
        optimize: true,
        optimizeLevel: '3',
        shrinkLevel: '2',
        runtime: 'stub',
        noAssert: true
    }
};
```

---

## **📦 Module Architecture**

### **Layer 1: Core Infrastructure (v1.4.2 UX Improved)**

```javascript
// ============================================
// Version management and metadata
// ============================================
const COMPILER_METADATA = {
    version: null,          // Dynamic retrieval
    versionSource: null,    // Source (Dynamic/Fallback)
    fetchedFrom: "NPM Registry via CDN",
    generatedAt: new Date().toISOString(),
    cdnProvider: null,      // Successful CDN provider
    loadTime: null,         // Load time (ms)
    attempts: 0,            // Number of attempts
    
    async initialize() {
        const startTime = performance.now();
        try {
            this.version = await VersionManager.getLatestVersion();
            this.versionSource = 'Dynamic fetch';
            console.log(`[Version] Fetched: v${this.version}`);
        } catch (error) {
            this.version = "0.28.8";
            this.versionSource = 'Hardcoded fallback';
            console.warn('[Version] Using fallback version');
        }
        const fetchTime = Math.round(performance.now() - startTime);
        console.log(`[Version] Time: ${fetchTime}ms`);
    }
};

// ============================================
// CDN Provider Definitions (v1.4.3 3-CDN version)
// ============================================
const CDN_PROVIDERS = [
    {
        name: 'jsDelivr',
        baseUrl: 'https://cdn.jsdelivr.net/npm',
        timeout: 10000,
        priority: 1
    },
    {
        name: 'UNPKG',
        baseUrl: 'https://unpkg.com',
        timeout: 10000,
        priority: 2
    },
    {
        name: 'esm.sh',
        baseUrl: 'https://esm.sh',
        timeout: 10000,
        priority: 3
    }
];

// ============================================
// Version Management System (v1.3.1 New)
// ============================================
const VersionManager = {
    // See Step 0.5 above for implementation
};

// ============================================
// Compiler Loader (Official Compatible Unified)
// ============================================
const CompilerLoader = {
    compiler: null,
    
    async load() {
        // Dynamically retrieve version
        await COMPILER_METADATA.initialize();
        
        // Load with CDN fallback
        return await loadCompilerWithCDNFallback();
    }
};

// ============================================
// Compiler Core
// ============================================
const CompilerCore = {
    compiler: null,
    
    init(ascInstance) {
        this.compiler = ascInstance;
        return true;
    },
    
    async compile(sourceCode, options) {
        if (!this.compiler) {
            throw new Error('Compiler not initialized');
        }
        
        const compileOptions = this.buildOptions(options);
        let wasmBinary = null;
        let compileStats = '';
        
        const { error, stdout, stderr } = await this.compiler.main(compileOptions, {
            readFile: (filename) => {
                if (filename === "main.ts") return sourceCode;
                return null;
            },
            writeFile: (filename, contents) => {
                if (filename === "main.wasm") {
                    wasmBinary = contents;
                }
            },
            listFiles: () => ["main.ts"]
        });
        
        if (error) throw new Error(stderr || error.toString());
        if (!wasmBinary) throw new Error('No WebAssembly binary generated');
        
        if (stdout) compileStats = stdout;
        
        return { wasmBinary, stats: compileStats };
    },
    
    buildOptions(userOptions) {
        const flags = ["main.ts", "-o", "main.wasm"];
        
        Object.entries(userOptions).forEach(([key, value]) => {
            if (typeof value === 'boolean' && value) {
                flags.push(`--${key}`);
            } else if (typeof value !== 'boolean' && value !== '') {
                flags.push(`--${key}`, value.toString());
            }
        });
        
        return flags;
    }
};

// ============================================
// Options Management System
// ============================================
const OptionsManager = {
    current: {},
    
    init() {
        this.current = { runtime: 'minimal' };
        this.loadFromStorage();
        this.updateUI();
        updateCmdPreview();  // v1.4.1: Update preview on initialization
    },
    
    apply(preset) {
        if (PRESETS[preset]) {
            // First, reset all UI options to their default state.
            document.querySelectorAll('[id^="opt-"]').forEach(el => {
                if (el.type === 'checkbox') {
                    el.checked = false;
                } else if (el.tagName === 'SELECT') {
                    const defaultOption = el.querySelector('option[selected]');
                    el.value = defaultOption ? defaultOption.value : (el.options[0] ? el.options[0].value : '');
                }
            });
   
            // Now, apply the new preset.
            this.current = { ...PRESETS[preset] };
            this.updateUI();
            this.saveToStorage();
        }
    },
    
    collectFromUI() {
        const options = {};
        
        // Boolean options
        ['optimize', 'exportRuntime', 'noAssert', 'debug', 'sourceMap',
         'measure', 'validate', 'importMemory', 'sharedMemory',
         'exportTable', 'explicitStart', 'lowMemoryLimit'].forEach(opt => {
            const el = document.getElementById(`opt-${opt}`);
            if (el && el.checked) options[opt] = true;
        });
        
        // Select options
        ['optimizeLevel', 'shrinkLevel', 'runtime'].forEach(opt => {
            const el = document.getElementById(`opt-${opt}`);
            if (el && el.value) options[opt] = el.value;
        });
        
        this.current = options;
        this.saveToStorage();
        
        // v1.4.2: Don't call updateCmdPreview inside collectFromUI
        // (Called only once during compile, so unnecessary)
        
        return options;
    },
    
    updateUI() {
        Object.keys(this.current).forEach(key => {
            const el = document.getElementById(`opt-${key}`);
            if (el) {
                if (el.type === 'checkbox') {
                    el.checked = this.current[key] === true;
                } else {
                    el.value = this.current[key] || '';
                }
            }
        });
    },
    
    saveToStorage() {
        try {
            localStorage.setItem('asmOptions', JSON.stringify(this.current));
        } catch(e) {}
    },
    
    loadFromStorage() {
        try {
            const saved = localStorage.getItem('asmOptions');
            if (saved) this.current = JSON.parse(saved);
        } catch(e) {}
    }
};

// ============================================
// Command-Line Preview Feature (v1.4.2 Fixed)
// ============================================
function updateCmdPreview() {
    // Collect options without side effects (don't change OptionsManager state)
    const options = {};
    
    // Checkboxes
    ['optimize', 'exportRuntime', 'noAssert', 'debug', 'sourceMap',
     'measure', 'validate', 'importMemory', 'sharedMemory',
     'exportTable', 'explicitStart', 'lowMemoryLimit'].forEach(opt => {
        const el = document.getElementById(`opt-${opt}`);
        if (el && el.checked) options[opt] = true;
    });
    
    // Select boxes
    ['optimizeLevel', 'shrinkLevel', 'runtime'].forEach(opt => {
        const el = document.getElementById(`opt-${opt}`);
        if (el && el.value) options[opt] = el.value;
    });
    
    const cmd = generateCmdLine(options);
    const preview = document.getElementById('cmd-preview');
    if (preview) {
        preview.textContent = cmd;
    }
    
    // v1.4.2: Record option changes and highlight button
    markOptionsChanged();
}

// v1.4.2: Visual feedback on option changes
function markOptionsChanged() {
    if (!optionsChanged) {
        optionsChanged = true;
        const btn = document.getElementById('recompileBtn');
        if (btn) {
            btn.classList.add('options-changed');
            // Change icon
            btn.innerHTML = '⚠️ <span data-i18n="button.recompile">Re-compile</span>';
        }
    }
}

function generateCmdLine(options) {
    const parts = ['asc', 'main.ts', '-o', 'main.wasm'];
    
    Object.entries(options).forEach(([key, value]) => {
        if (typeof value === 'boolean' && value) {
            parts.push(`--${key}`);
        } else if (typeof value !== 'boolean' && value !== '') {
            parts.push(`--${key}`, value.toString());
        }
    });
    
    return parts.join(' ');
}

// ============================================
// WebAssembly Execution Management
// ============================================
const WasmRunner = {
    instance: null,
    
    async instantiate(wasmBinary, imports = {}) {
        const defaultImports = {
            env: {
                abort: (msg, file, line, column) => {
                    console.error(`Abort: ${msg} at ${file}:${line}:${column}`);
                },
                trace: (msg, n, ...args) => {
                    console.log(`Trace: ${msg}`, ...args);
                },
                seed: () => Date.now(),
                memory: new WebAssembly.Memory({ initial: 1 })
            },
            Math: Math,
            Date: Date
        };
        
        const mergedImports = this.mergeImports(defaultImports, imports);
        
        let buffer;
        if (wasmBinary instanceof ArrayBuffer) {
            buffer = wasmBinary;
        } else if (wasmBinary instanceof Uint8Array) {
            buffer = wasmBinary.buffer.slice(wasmBinary.byteOffset, 
                                             wasmBinary.byteOffset + wasmBinary.byteLength);
        } else {
            throw new Error('Unexpected binary format');
        }
        
        const wasmModule = await WebAssembly.compile(buffer);
        this.instance = await WebAssembly.instantiate(wasmModule, mergedImports);
        
        return this.instance.exports;
    },
    
    mergeImports(defaults, custom) {
        const merged = { ...defaults };
        Object.keys(custom).forEach(key => {
            if (typeof custom[key] === 'object' && !Array.isArray(custom[key])) {
                merged[key] = { ...defaults[key], ...custom[key] };
            } else {
                merged[key] = custom[key];
            }
        });
        return merged;
    }
};

// ============================================
// Preset Definitions
// ============================================
const PRESETS = {
    simple: {
        optimize: false,
        runtime: 'minimal'
    },
    debug: {
        optimize: false,
        optimizeLevel: '0',
        debug: true,
        sourceMap: true,
        measure: true
    },
    release: {
        optimize: true,
        optimizeLevel: '3',
        shrinkLevel: '1',
        noAssert: true
    },
    minimal: {
        optimize: true,
        optimizeLevel: '3',
        shrinkLevel: '2',
        runtime: 'stub',
        noAssert: true
    }
};
```

### **Layer 2: Application Module (Customization Area)**

```javascript
// Placeholder for AI-generated part
const AppModule = {
    // Module identification
    name: "UserDefinedApp",
    description: "User-defined application",
    
    // AssemblyScript source code
    sourceCode: `
        // AI generates based on user requirements
        export function main(): void {
            // Application logic
        }
    `,
    
    // Recommended compilation options
    defaultOptions: {
        optimize: false,
        runtime: 'minimal'
    },
    
    // WebAssembly import definitions
    getImports() {
        return {
            // Custom imports as needed
        };
    },
    
    // Initialization process
    async init(wasmExports) {
        // Initialization logic
    },
    
    // Main loop execution
    execute(wasmExports, deltaTime) {
        // Per-frame processing
    },
    
    // UI update
    render(data) {
        // Draw/display update
    },
    
    // Cleanup
    cleanup() {
        // Resource release
    }
};
```

---

## **📝 HTML Template (v1.4.2)**

The complete HTML template code is provided in a separate file:

### **📄 ASMSCRIPT-OPTIMIZER-RECIPE-HTMLTEMPLATE.md**

This file includes:
- Complete HTML structure (approx. 1500 lines)
- Default demo (counter app)
- All UI components
- CDN fallback mechanism
- Dynamic version retrieval system
- Command-line preview feature
- Visual feedback on option changes (v1.4.2 new feature)

When implementing, use the HTML template file code as is,
and replace the AppModule section according to your use case.

### When "ASMSCRIPT-OPTIMIZER-RECIPE-HTMLTEMPLATE.md" Cannot Be Referenced Due to User Forgetting to Attach Template File

1. **Stop work** and prompt the user to attach the "ASMSCRIPT-OPTIMIZER-RECIPE-HTMLTEMPLATE.md" file.
2. Resume work after confirming attachment of "ASMSCRIPT-OPTIMIZER-RECIPE-HTMLTEMPLATE.md" file.

---

## **🤖 Universal AI Implementation Guide**

### **Importance of Step 0 Execution**

When Generative AI generates HTML, **always execute Step 0 first**:

1. **Verify clarity of user requirements**
2. **Stop work and inquire if unclear**
3. **Start implementation after receiving clear answers**
4. **Automatic generation of default app is strictly prohibited**

### **Dynamic Version Retrieval Implementation**

When Generative AI generates HTML, retrieve the latest version using these steps:

1. **Integrate VersionManager**
2. **Prioritize jsDelivr API**
3. **Use 0.28.8 as fallback**
4. **Utilize cache mechanism**

### **Adherence to Official Recommended Method Compatibility**

When Generative AI generates HTML, strictly follow these implementation policies:

1. **Never use direct import**
2. **Always load via web.js**
3. **CDNs are jsDelivr, UNPKG, esm.sh** (in priority order)
4. **Wait for import map configuration**

### **UX Improvements (v1.4.2)**

When Generative AI generates HTML, always implement these UX improvements:

1. **Visually highlight button on option changes**
2. **Don't auto-compile on preset application**
3. **Menu opens/closes only by user operation**
4. **Remove side effects from updateCmdPreview()**

### **Implementation Flowchart**

```
0. Receive user request
   ↓
1. Verify requirement clarity (Step 0)
   ├─ Unclear → Inquire with user → Wait for response
   └─ Clear → Proceed
   ↓
2. Dynamic version retrieval
   ↓
3. Determine use case
   ↓
4. Generate AssemblyScript code
   ↓
5. Generate AppModule
   ↓
6. Use HTML template v1.4.2
   ↓
7. Output as single HTML file
```

### **Use Case Implementation Examples**

#### **1. Physics Simulation (Ball)**

```javascript
const AppModule = {
    name: "Ball Physics Demo",
    description: "Ball motion simulation with physics",
    
    sourceCode: `
export class Ball {
    x: f32 = <f32>300.0;
    y: f32 = <f32>200.0;
    vx: f32 = <f32>3.0;
    vy: f32 = <f32>2.0;
    radius: f32 = <f32>15.0;
}

let ball = new Ball();

export function update(): void {
    ball.x += ball.vx;
    ball.y += ball.vy;
    
    if (ball.x - ball.radius <= 0 || ball.x + ball.radius >= 600) {
        ball.vx = -ball.vx;
    }
    if (ball.y - ball.radius <= 0 || ball.y + ball.radius >= 400) {
        ball.vy = -ball.vy;
    }
}

export function getX(): f32 { return ball.x; }
export function getY(): f32 { return ball.y; }
export function getRadius(): f32 { return ball.radius; }
    `.trim(),
    
    async init(wasmExports) {
        const viewport = document.getElementById('app-viewport');
        viewport.innerHTML = '<canvas id="canvas" width="600" height="400" style="border: 1px solid #e5e7eb; border-radius: 8px;"></canvas>';
    },
    
    execute(wasmExports) {
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        
        function animate() {
            wasmExports.update();
            
            ctx.fillStyle = '#f8f9fa';
            ctx.fillRect(0, 0, 600, 400);
            
            const x = wasmExports.getX();
            const y = wasmExports.getY();
            const r = wasmExports.getRadius();
            
            ctx.fillStyle = '#667eea';
            ctx.beginPath();
            ctx.arc(x, y, r, 0, Math.PI * 2);
            ctx.fill();
            
            requestAnimationFrame(animate);
        }
        animate();
    }
};
```

#### **2. Image Filter Processing (v1.4.0 Working Version)**

```javascript
const AppModule = {
    name: "Image Filter Processor",
    description: "High-speed image filter processing",
    
    sourceCode: `
// Global image buffer (reusable)
let imageBuffer: Uint8ClampedArray | null = null;

// Initialize or reallocate buffer
export function initBuffer(size: i32): void {
    imageBuffer = new Uint8ClampedArray(size);
}

// Get buffer pointer (for data copy from JavaScript)
export function getBufferPtr(): usize {
    if (!imageBuffer) return 0;
    return imageBuffer!.dataStart;
}

// Grayscale conversion (TypedArray-based)
export function applyGrayscale(): void {
    if (!imageBuffer) return;
    
    let data = imageBuffer!;
    let length = data.length;
    
    for (let i = 0; i < length; i += 4) {
        let r = unchecked(data[i]);
        let g = unchecked(data[i + 1]);
        let b = unchecked(data[i + 2]);
        
        // ITU-R BT.601 standard grayscale calculation
        let gray = <u8>(
            <f32>r * <f32>0.299 + 
            <f32>g * <f32>0.587 + 
            <f32>b * <f32>0.114
        );
        
        unchecked(data[i] = gray);
        unchecked(data[i + 1] = gray);
        unchecked(data[i + 2] = gray);
        // Don't change Alpha value (i + 3)
    }
}

// Sepia tone effect
export function applySepia(): void {
    if (!imageBuffer) return;
    
    let data = imageBuffer!;
    let length = data.length;
    
    for (let i = 0; i < length; i += 4) {
        let r = unchecked(data[i]);
        let g = unchecked(data[i + 1]);
        let b = unchecked(data[i + 2]);
        
        let tr = <f32>r * <f32>0.393 + <f32>g * <f32>0.769 + <f32>b * <f32>0.189;
        let tg = <f32>r * <f32>0.349 + <f32>g * <f32>0.686 + <f32>b * <f32>0.168;
        let tb = <f32>r * <f32>0.272 + <f32>g * <f32>0.534 + <f32>b * <f32>0.131;
        
        unchecked(data[i] = <u8>Math.min(255, tr));
        unchecked(data[i + 1] = <u8>Math.min(255, tg));
        unchecked(data[i + 2] = <u8>Math.min(255, tb));
    }
}

// Invert effect
export function applyInvert(): void {
    if (!imageBuffer) return;
    
    let data = imageBuffer!;
    let length = data.length;
    
    for (let i = 0; i < length; i += 4) {
        unchecked(data[i] = 255 - data[i]);
        unchecked(data[i + 1] = 255 - data[i + 1]);
        unchecked(data[i + 2] = 255 - data[i + 2]);
    }
}

// Brightness adjustment
export function adjustBrightness(factor: f32): void {
    if (!imageBuffer) return;
    
    let data = imageBuffer!;
    let length = data.length;
    
    for (let i = 0; i < length; i += 4) {
        let r = <f32>unchecked(data[i]) * factor;
        let g = <f32>unchecked(data[i + 1]) * factor;
        let b = <f32>unchecked(data[i + 2]) * factor;
        
        unchecked(data[i] = <u8>Math.max(0, Math.min(255, r)));
        unchecked(data[i + 1] = <u8>Math.max(0, Math.min(255, g)));
        unchecked(data[i + 2] = <u8>Math.max(0, Math.min(255, b)));
    }
}

// Memory statistics (for debugging)
export function getMemoryStats(): i32 {
    return memory.size(); // Return number of pages
}

// Get buffer size
export function getBufferSize(): i32 {
    return imageBuffer ? imageBuffer!.length : 0;
}
    `.trim(),
    
    defaultOptions: {
        optimize: true,
        optimizeLevel: '3',
        shrinkLevel: '1',
        noAssert: true,
        runtime: 'minimal',
        importMemory: false,  // Use internal memory
        exportRuntime: false  // Runtime functions not needed
    },
    
    // Filter application processing in JavaScript
    applyFilter(wasmExports, filterType, imageData) {
        const dataLength = imageData.data.length;
        
        // Initialize buffer
        wasmExports.initBuffer(dataLength);
        const bufferPtr = wasmExports.getBufferPtr();
        
        if (bufferPtr === 0) {
            throw new Error('Failed to initialize buffer');
        }
        
        // Copy data to WebAssembly memory
        const wasmMemory = new Uint8ClampedArray(wasmExports.memory.buffer);
        for (let i = 0; i < dataLength; i++) {
            wasmMemory[bufferPtr + i] = imageData.data[i];
        }
        
        // Apply filter
        switch(filterType) {
            case 'grayscale':
                wasmExports.applyGrayscale();
                break;
            case 'sepia':
                wasmExports.applySepia();
                break;
            case 'invert':
                wasmExports.applyInvert();
                break;
            case 'bright':
                wasmExports.adjustBrightness(1.3);
                break;
            case 'dark':
                wasmExports.adjustBrightness(0.7);
                break;
        }
        
        // Get processed data
        const updatedWasmMemory = new Uint8ClampedArray(wasmExports.memory.buffer);
        const updatedBufferPtr = wasmExports.getBufferPtr();
        
        for (let i = 0; i < dataLength; i++) {
            imageData.data[i] = updatedWasmMemory[updatedBufferPtr + i];
        }
        
        return imageData;
    }
};
```

---

## **📚 Appendix**

### **AssemblyScript Type Correspondence Table**

| AssemblyScript | JavaScript | Use |
|----------------|------------|-----|
| i32 | number | Integer operations |
| f32 | number | Single precision float |
| f64 | number | Double precision float |
| i64 | BigInt | Large integers |
| bool | boolean | Boolean values |

### **Performance Metrics**

- Initial compilation: < 5 seconds
- Recompilation: < 3 seconds
- WASM size: 1KB-50KB (option dependent)
- Execution speed: 2-10x JavaScript (depends on processing)

### **Troubleshooting**

| Problem | Cause | Solution |
|---------|-------|----------|
| "import map not found" | web.js load failure | CDN fallback handles automatically |
| Compiler load failure | CDN outage/network | Automatically retries with 3 CDNs |
| Timeout | Slow network | Automatically switches to next CDN |
| Version mismatch | Breaking changes | Dynamic version retrieval handles |
| Low performance | No optimization | Enable --optimize option |
| Large WASM size | Debug information | Disable --debug, increase --shrinkLevel |
| Filter not applied | Memory management error | Use TypedArray and unchecked |
| Type conversion error | Implicit type conversion | Add explicit <f32> cast |
| Preview not updating | updateCmdPreview side effects | Fixed in v1.4.2 |

### **Debugging Tips**

```javascript
// Enable debug mode in local storage
localStorage.setItem('debug', 'true');

// Compiler info panel will be displayed (v1.3.1 enhanced)
// - AssemblyScript version
// - Version source (Dynamic/Fallback)
// - CDN provider used (color-coded display)
// - Load time
// - Number of attempts

// Check detailed logs in console
// [Version] tag for version retrieval status
// [Loader] tag for loading progress
// [Init] tag for initialization status
// [Progress] tag for progress status
// [Compile] tag to check export functions
// [Filter] tag for image processing details
```

### **CDN Provider Details**

| CDN | URL | Features | Recommended Use |
|-----|-----|----------|-----------------|
| **jsDelivr** | cdn.jsdelivr.net | Fastest, global CDN, officially recommended | First choice |
| **UNPKG** | unpkg.com | Official npm mirror, integrity guaranteed | Second choice |
| **esm.sh** | esm.sh | Modern ESM delivery, experimental | Final fallback |

---

## **📄 License**

MIT License - Free to use, modify, redistribute, commercial use

---

## **📄 Update History**

### **v1.4.3** (2025-09) - CDN Fallback Enhancement
**Main Changes:**
- ✅ **Added esm.sh**: Placed as final fallback as 3rd CDN
- ✅ **Improved reliability**: Better connection success rate with 3-stage fallback
- ✅ **Minimal changes**: Only array addition while maintaining existing logic
- ✅ **Initialization time**: Worst case 30 seconds (3 CDNs × 10 second timeout)

**Technical Improvements:**
- Added esm.sh as priority:3
- Adjusted progress bar calculation for 3 CDNs
- Revived esm.sh excluded in v1.3.1 after judging it useful

### **v1.4.2** (2025-09) - UX Improvements and Bug Fixes
**Fixes:**
- ✅ Removed side effects when updating command preview (performance improvement)
- ✅ Removed auto-compile on preset application
- ✅ Fixed menu not closing on recompile
- ✅ Visually highlight recompile button on option changes
- ✅ Changed menu open/close to complete user control

**Technical Improvements:**
- Fixed updateCmdPreview() to not change state
- Change tracking with optionsChanged flag
- Separation of concerns between UI operations and compilation processing
- Reduced frequency of OptionsManager.collectFromUI() calls

### **v1.4.1** (2025-09) - Command-Line Preview Feature Added
**Main Changes:**
- ✅ **Command-line preview feature**: Added at top of WebAssembly tab
- ✅ **Real-time updates**: Instantly reflects option changes
- ✅ **All options supported**: Reflects all options settable in UI
- ✅ **Preset integration**: Auto-updates on preset application
- ✅ **Simple implementation**: Minimal implementation without copy feature

**Technical Improvements:**
- Centralized management with updateCmdPreview() function
- Flexible command generation with generateCmdLine()
- Immediate response with onchange attribute
- Selectable text with user-select: text

### **v1.4.0** (2025-09) - Memory Management and Sample Fixes
**Main Changes:**
- ✅ **Fixed image filter sample**: Updated to working TypedArray-based version
- ✅ **Added memory management explanation**: Detailed explanation of standard memory access methods
- ✅ **Improved type conversion**: Explicit casting for all numeric literals
- ✅ **Enhanced debugging**: Verification of export functions and memory state
- ✅ **Added best practices**: unchecked optimization and global buffer
- ✅ **HTML template separation**: Improved maintainability with separate file

**Technical Improvements:**
- Type-safe memory access with TypedArray
- Bounds check skipping with unchecked()
- Reduced GC load with global buffer
- Proper use of load/store built-in functions

### **v1.3.1** (2025-09) - Improved Stability and Simplification
**Main Changes:**
- ✅ **Added dynamic version retrieval**: Latest version retrieval prioritizing jsDelivr API
- ✅ **CDN optimization**: Switched to 2-provider system excluding esm.sh
- ✅ **Cache mechanism**: Speed improvement with session storage
- ✅ **Reduced initialization time**: Worst case 30s → 20s
- ✅ **Enhanced debug info**: Visualization of version source

### **v1.3.0** (2025-09) - Complete Migration to Official Recommended Method Compatibility
**Main Changes:**
- ✅ **Unified loading method**: Uses only official-compatible web.js method
- ✅ **Removed direct import**: Completely removed problematic direct import method
- ✅ **3 CDN providers**: jsDelivr → UNPKG → esm.sh
- ✅ **Simplified implementation**: Greatly improved maintainability with single pattern

### **v1.2.2** (2025-09) - Added Official Compatible Implementation Fallback Feature
**Main Changes:**
- ✅ **3-stage fallback mechanism**: Direct import → Alternative CDN → Official web.js
- ✅ **Dynamic version management**: Get latest stable version from GitHub tags
- ✅ **@latest prohibition rule**: Made specific version specification mandatory
- ✅ **Enhanced diagnostics**: Logging of load method and metadata recording

### **v1.2.1** (2025-09) - Added User Requirements Verification Process
**Main Changes:**
- ✅ **Added Step 0**: Made verification process mandatory for unclear user requirements
- ✅ **Work suspension rule**: Prohibited automatic judgment on ambiguous instructions
- ✅ **Inquiry template**: Provided structured question format

### **v1.2** (2025-09) - Complete UI/UX Overhaul
**Main Changes:**
- ✅ **Adopted hamburger menu**: Efficient screen use with side menu
- ✅ **Tab-based settings management**: Logically separated app and WebAssembly settings
- ✅ **WASM stats toggle**: OFF by default, displayed only when needed
- ✅ **Responsive support**: From mobile to desktop

### **v1.1** (2025-09) - Resolved Race Condition Issues
**Main Changes:**
- ✅ **Adopted Dynamic Import**: Removed direct loading via script tags
- ✅ **Added retry mechanism**: Automatic retry and exponential backoff
- ✅ **Timeout protection**: Set upper limit for compiler loading

### **v1.0** (2025-09) - Initial Release
- Adopted modular architecture
- Auto-initialization feature
- 15 compilation options
- Error overlay feature

---

## **📊 Feature Comparison by Version**

| Feature | v1.0 | v1.1 | v1.2 | v1.2.1 | v1.2.2 | v1.3.0 | v1.3.1 | v1.4.0 | v1.4.1 | v1.4.2 | v1.4.3 |
|---------|------|------|------|--------|--------|--------|--------|--------|--------|--------|--------|
| Basic compilation | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Race condition mitigation | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Modern UI | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| User requirements verification | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Official compatible fallback | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Dynamic version management | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Official method compatibility | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| jsDelivr priority | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 2-CDN system | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ |
| 3-CDN system | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |
| esm.sh support | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Memory management explanation | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| Working samples | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| HTML template separation | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| Command-line preview | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ |
| Highlight on option change | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |

---

**Generated Date**: September 2025  
**Recipe Version**: 1.4.3  
**AssemblyScript Supported Version**: Dynamic retrieval (Fallback: 0.28.8)
