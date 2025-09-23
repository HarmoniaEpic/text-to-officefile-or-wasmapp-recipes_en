# WAT_SingleHTML_WASI_Recipe_v3 - Complete Template for Compiling & Executing "AI->WAT->wasm32-wasi" in **Single HTML** (No COI Required / CDN ESM Only / Minimal WASI Direct Execution)

> This recipe adopts the attached single HTML (latest version) as a **full text template** and is v3 refined to achieve **"template power" equivalent to Math-RECIPE**.  
> This enables seamless reproduction of the PoC flow: **User request -> AI generates WAT -> Single HTML assembles .wat->.wasm -> Browser executes with WASI**.

---

## 1. Role Division (AI and Single HTML)

- **AI (WAT Generation Agent)**
  - Converts user requests (natural language) to **WAT (WebAssembly Text)**.
  - **Inserts (replaces) the generated WAT code directly into `<script type="text/plain" id="wat-source">` content** in the template HTML below.
  - Ensures **byte length match** for string length (iovec.len) etc. if necessary.

- **Single HTML (Executor)**
  - Automatically loads **wabt@1.0.37 (ESM `index.js`)** from CDN when page displays.
  - **Assemble** converts WAT->Wasm, **Run** executes with minimal WASI (`fd_write`).  
  - Includes **Self-tests**, **SHA-256**, **WASM download**, **Direct execution of existing WASM**, **(optional) auto-run toggle**, **auto-scroll according to progress**.

---

## 2. AI Implementation Steps (**For Copy-Paste Operation / Including Unchangeable Points**)

**Step A: Generate WAT (SYSTEM template required)**
- Target is **wasm32-wasi**.  
- **Export `_start`** (don't use `(start ...)`).  
- Import `fd_write` for stdout only.  
- Define **(memory (export "memory") 1)**, use iovec(0..7) / message(8..) / nwritten(20..23).  
- **String length must be exact byte count** (newline is `\0a`, no terminating NUL needed).

**Step B: Insert into Template HTML (this is the "fixed position")**
- **Replace the entire content** of `<script type="text/plain" id="wat-source"> ... </script>`.  
- **Must not change the tag's attributes, ID, or `type`** (deletion also prohibited).

**Step C: Don't touch other parts of HTML**
- **CDN URL is `index.js` only** (**don't use** `/wabt.js` or `/dist/wabt.js`).  
- **Load order modification prohibited** (jsDelivr -> UNPKG -> esm.sh).  
- UI ID names, log formats (`stdout equality - PASS` etc.) modification prohibited.

---

## 3. **Unchangeable Checklist** (Both AI/Humans Must Comply)

- [ ] **Tag attributes of `<script id="wat-source">` are immutable**. Replace only the content.  
- [ ] Import only **`index.js` of wabt@1.0.37** (UMD `/wabt.js` series **prohibited**).  
- [ ] Load order: **jsDelivr -> UNPKG -> esm.sh**.  
- [ ] **Must export `_start`**. **Using `(start ...)` is prohibited**.  
- [ ] String length (`iov.len`) must be **exact** (e.g.: `"Hello, WAT!\n"` is **12**).  
- [ ] Self-test log formats (`stdout equality - ...` / `syntax error detection - ...`) are **unchangeable**.

---

## 4. WAT Generation Template (SYSTEM Instructions for AI)

````text
SYSTEM (Immutable Rules / WAT Generation Agent):

**Target**
- wasm32-wasi. Output single (module). Code only.

**Export**
- Must define (func (export "_start") ...). Don't use (start ...).

**Import**
- For stdout only:
  (import "wasi_snapshot_preview1" "fd_write"
    (func $fd_write (param i32 i32 i32 i32) (result i32)))

**Memory and Layout**
- Define (memory (export "memory") 1).
- iovec[0] = {{buf(0..3), len(4..7)}}. Message at 8.., nwritten at 20..23.
- String length (len) is exact byte count. No terminating NUL. Newline is "\0a".

**Quality Check**
- All i32.store/load on 4-byte boundaries.
- fd_write's 4th argument is address to write back written byte count.
- No syntax/type errors with wabt(parseWat->toBinary).

**Prohibitions**
- (start ...) / experimental features (threads, simd, ref.func etc.) prohibited.
- Generating huge data or infinite loops prohibited.

**Output Format**
- Output only WAT wrapped in ```wat. No explanatory text before/after.

USER Request:
<<Insert user's request here>>
````

---

## 5. Complete **Single HTML Template (Full Text)**
> Save the following **as-is as .html**, **replace only the content of `<script id="wat-source">` with WAT** to use.  
> Based on attached HTML (latest version). **AI_INSERT markers** added to some comments.

```html
<!doctype html>
<html lang="ja">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>WAT -> Wasm32-WASI Assembly & Execution (No COI Required•CDN Load Only•Existing WASM Loading)</title>
  <style>
    :root { color-scheme: light dark; }
    body { margin: 0; font-family: ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Noto Sans", Arial; line-height: 1.5; }
    header { padding: 12px 16px; background: #0001; }
    main { display: grid; grid-template-columns: 1fr; gap: 12px; padding: 12px; }
    /* @media (min-width: 1120px) { main { grid-template-columns: 1fr 1fr; } } */
    h1 { font-size: 1.05rem; margin: 0; }
    textarea, pre, button, input { font: inherit; }
    textarea { width: 100%; min-height: 260px; padding: 8px; border-radius: 8px; border: 1px solid #0002; font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; }
    .card { border: 1px solid #0002; border-radius: 12px; padding: 12px; background: #00000008; }
    .row { display: flex; gap: 8px; flex-wrap: wrap; align-items: center; }
    .mono { font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; }
    .muted { opacity: .8; }
    .ok { color: #176b37; }
    .err { color: #b22222; }
    .out { height: 220px; overflow: auto; white-space: pre-wrap; background: #0000; padding: 8px; border-radius: 8px; border: 1px dashed #0003; }
    .kv { display: grid; grid-template-columns: max-content 1fr; gap: 4px 12px; }
    .kv div:nth-child(odd){ color: #000a; }
    .btn { padding: 8px 12px; border: 1px solid #0003; border-radius: 8px; background: #fff; cursor: pointer; }
    .btn[disabled]{ opacity: .5; cursor: not-allowed; }
    .footer { font-size: .9rem; padding-top: 4px; }
    .badge { padding: 2px 8px; border-radius: 999px; background: #9992; font-size: .85rem; }
    .tip { background: #ffeaa7; color: #4d3b00; padding: 6px 8px; border-radius: 8px; border: 1px solid #d8c16e; }
    .warn { background: #ffdddd; color: #6b1a1a; padding: 6px 8px; border-radius: 8px; border: 1px solid #e4a6a6; }
  </style>
</head>
<body>
  <header>
    <h1>WAT -> Wasm32-WASI Assembly & Execution <span class="badge">No COI Required / CDN Load Only (Official URL) / Existing WASM Loading</span></h1>
  </header>

  <main>
    <section class="card" id="sec-load">
      <h2>1) Load WABT (Assembler) from CDN</h2>
      <p class="muted">wabt uses only the <b>official ESM entry (<code>index.js</code>)</b>. UMD <code>/wabt.js</code> or <code>/dist/wabt.js</code> are not used.</p>
      <div class="row" style="margin-top:6px">
        <button id="load-wabt-cdn" class="btn">Reload CDN</button>
        <span id="wabt-cdn-status" class="muted">not loaded</span>
      </div>
      <details style="margin-top:6px">
        <summary>URLs to use (retry in order)</summary>
        <ul class="mono muted">
          <li>1) jsDelivr: https://cdn.jsdelivr.net/npm/wabt@1.0.37/index.js</li>
          <li>2) UNPKG: https://unpkg.com/wabt@1.0.37/index.js</li>
          <li>3) esm.sh: https://esm.sh/wabt@1.0.37</li>
        </ul>
      </details>
      <div id="cdn-warn" class="warn" style="margin-top:6px; display:none"></div>
    </section>

    <section class="card" id="sec-wat">
      <h2>2) WAT Source (AI embeds / Editable)</h2>
      <p class="muted">Default is "Hello, WAT!". <span class="tip">Please Assemble -> Run after CDN load.</span></p>
      <textarea id="wat" class="mono"></textarea>
      <div class="row" style="margin-top:8px">
        <button id="assemble" class="btn" disabled>Assemble (WAT -> Wasm)</button>
        <span id="asm-status" class="muted">-</span>
      </div>
      <div class="row" style="margin-top:4px">
        <label class="row" style="gap:6px; align-items:center">
          <input type="checkbox" id="auto-run" />
          <span class="muted">Auto-run on successful assembly</span>
        </label>
      </div>
    </section>

    <section class="card" id="sec-wasm">
      <h2>3) Load Existing Wasm (Direct execution without assembly)</h2>
      <p class="muted">Load pre-built <code>.wasm</code> for direct execution (when step 2 is unnecessary).</p>
      <div class="row" style="margin-top:6px">
        <label class="btn" for="wasm-file">Select main.wasm</label>
        <input id="wasm-file" type="file" accept=".wasm,application/wasm" style="display:none" />
        <span id="wasm-file-name" class="mono muted">(not selected)</span>
      </div>
    </section>

    <section class="card" id="sec-run">
      <h2>4) Execution and Download</h2>
      <div class="row" style="margin-top:8px">
        <button id="run" class="btn" disabled>Run (WASI: fd_write only)</button>
        <button id="tests" class="btn" disabled>Run self-tests</button>
        <span id="status" class="muted">idle</span>
      </div>
      <div class="kv" style="margin-top:8px">
        <div>WASM source:</div><div id="wasm-src" class="mono muted">(none)</div>
        <div>WASM size:</div><div id="wasm-size" class="mono muted">-</div>
        <div>SHA-256:</div><div id="sha256" class="mono muted">-</div>
        <div>Exit code:</div><div id="exit" class="mono muted">-</div>
      </div>
      <div class="row" style="margin-top:8px">
        <button id="dl-wasm" class="btn" disabled>Download wasm (main.wasm)</button>
      </div>
    </section>

    <section class="card" id="sec-log">
      <h2>5) Log / Output</h2>
      <div style="display:grid; grid-template-columns:1fr; gap:8px">
        <div>
          <div class="muted">Log</div>
          <pre id="log" class="out mono"></pre>
        </div>
        <div>
          <div class="muted">stdout</div>
          <pre id="stdout" class="out mono"></pre>
        </div>
        <div>
          <div class="muted">stderr</div>
          <pre id="stderr" class="out mono"></pre>
        </div>
      </div>
      <div class="footer muted">Generate WAT->Wasm with wabt.js (CDN only). Execute with minimal WASI (fd_write) hook.</div>
    </section>
      <section class="card" id="sec-links">
      <h2>Reference Links</h2>
      <ul>
        <li><a href="https://developer.mozilla.org/en-US/docs/WebAssembly/Guides/Concepts" target="_blank" rel="noopener">WebAssembly concepts - WebAssembly | MDN</a></li>
        <li><a href="https://developer.mozilla.org/en-US/docs/WebAssembly/Guides/Text_format_to_Wasm" target="_blank" rel="noopener">Converting WebAssembly text format to binary - WebAssembly | MDN</a></li>
      </ul>
    </section>
  </main>

  <!-- Default WAT source (Hello example) -->
  <!-- AI_INSERT: Replace the innerText of this tag with generated WAT -->
  <script type="text/plain" id="wat-source">;; wasm32-wasi / output "Hello, WAT!
" with _start
(module
  (import "wasi_snapshot_preview1" "fd_write"
    (func $fd_write (param i32 i32 i32 i32) (result i32)))
  (memory (export "memory") 1)
  ;; iovec[0] at 0..7 (buf, len), message at 8.., nwritten at 20..23
  (data (i32.const 8) "Hello, WAT!\0a")
  (func (export "_start")
    (i32.store (i32.const 0) (i32.const 8))   ;; iov.buf = 8
    (i32.store (i32.const 4) (i32.const 12))  ;; iov.len = 12
    (call $fd_write
      (i32.const 1) (i32.const 0) (i32.const 1) (i32.const 20))
    drop
  )
)</script>

  <script type="module">
    const $ = (s) => document.querySelector(s);
    const log = (m, cls="") => { const p = $("#log"); const t = document.createElement("div"); t.textContent = m; if(cls) t.className = cls; p.appendChild(t); p.scrollTop = p.scrollHeight; };
    const setStatus = (s) => $("#status").textContent = s;

    // Initial text
    $("#wat").value = (document.getElementById("wat-source")?.textContent || "").trim();

    // ==== State management ====
    let wabt = null;           // WABT instance
    let wabtReady = false;     // Can parseWat
    let wasmBytes = null;      // Currently selected/generated Wasm
    let wasmName = "main.wasm";// Download name
    let wasmSource = "(none)"; // assembled / uploaded

    function setWasm(bytes, name, sourceTag) {
      wasmBytes = bytes; wasmName = name || "main.wasm"; wasmSource = sourceTag || "(unknown)";
      $("#wasm-src").textContent = wasmSource;
      $("#wasm-size").textContent = bytes ? String(bytes.length) + " bytes" : "-";
      $("#dl-wasm").disabled = !bytes;
      $("#run").disabled = !bytes;
      updateSha(bytes);
    }

    async function updateSha(bytes){
      if (!bytes) { $("#sha256").textContent = "-"; return; }
      const hash = await crypto.subtle.digest("SHA-256", bytes);
      const hex = [...new Uint8Array(hash)].map(b=>b.toString(16).padStart(2,"0")).join("");
      $("#sha256").textContent = hex;
    }

    function download(name, u8, mime="application/wasm"){
      const blob = new Blob([u8], { type: mime });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a"); a.href = url; a.download = name; a.click(); URL.revokeObjectURL(url);
    }

    // ---- Auto scroll helper (only when auto-run is checked) ----
    function autoScrollTo(selector){
      const auto = document.getElementById("auto-run");
      if (!auto || !auto.checked) return;
      const el = document.querySelector(selector);
      if (el) el.scrollIntoView({ behavior: 'smooth', block: 'start' });
    }

    // ==== CDN Loader (official ESM entry only) ====
    async function loadWabtFromCDN() {
      // Reload action: Initialize state and retry
      $("#cdn-warn").style.display = 'none';
      $("#cdn-warn").textContent = '';
      wabt = null; wabtReady = false;
      $("#assemble").disabled = true; $("#tests").disabled = true;
      $("#wabt-cdn-status").textContent = "loading ...";

      const ts = Date.now();
      const addTs = (u) => u + (u.includes('?') ? '&' : '?') + 'ts=' + ts; // Cache avoidance
      const tryList = [
        async () => { const m = await import(addTs("https://cdn.jsdelivr.net/npm/wabt@1.0.37/index.js")); return m.default(); },
        async () => { const m = await import(addTs("https://unpkg.com/wabt@1.0.37/index.js")); return m.default(); },
        async () => { const m = await import(addTs("https://esm.sh/wabt@1.0.37")); return (m && m.default) ? m.default() : (typeof m === 'function' ? m() : null); },
      ];
      for (const fn of tryList) {
        try {
          const inst = await fn();
          if (inst && typeof inst.parseWat === "function") {
            wabt = inst; wabtReady = true;
            $("#wabt-cdn-status").textContent = "loaded";
            $("#assemble").disabled = false; $("#tests").disabled = false;
            log("wabt.js ready (CDN)", "ok");
            return;
          }
        } catch (e) { console.warn("CDN load failed:", e); }
      }
      $("#wabt-cdn-status").textContent = "failed";
      const warn = $("#cdn-warn");
      warn.textContent = "Cannot execute when all CDNs are blocked.";
      warn.style.display = 'block';
      log("All CDN loaders failed.", "err");
    }

    $("#load-wabt-cdn").addEventListener("click", loadWabtFromCDN);

    // ==== Assemble (WAT -> Wasm) ====
    async function assemble() {
      try {
        if (!wabtReady) throw new Error("Please load WABT from CDN first.");
        setStatus("assembling ..."); $("#stdout").textContent = ""; $("#stderr").textContent = ""; $("#exit").textContent = "-"; $("#sha256").textContent = "-"; $("#wasm-size").textContent = "-"; $("#dl-wasm").disabled = true; $("#run").disabled = true; autoScrollTo('#sec-wat');
        const watText = $("#wat").value; log("[wabt] parse & toBinary ...");
        const mod = wabt.parseWat("inline", watText, {}); const bin = mod.toBinary({ log:false, write_debug_names:true }); mod.destroy();
        setWasm(bin.buffer, "main.wasm", "assembled"); $("#asm-status").textContent = "ok"; setStatus("assemble ok"); log("[wabt] ok", "ok");
        if (document.getElementById("auto-run")?.checked) {
          autoScrollTo('#sec-run');
          log("[auto] run after assemble");
          await runWasi();
        }
      } catch (e) {
        console.error(e); setStatus("assemble error");
        autoScrollTo('#sec-log'); $("#asm-status").textContent = "error"; log("Assemble error: "+String(e?.message || e), "err");
      }
    }
    $("#assemble").addEventListener("click", assemble);

    // ==== Load existing Wasm ====
    $("#wasm-file").addEventListener("change", async (e)=>{
      const f = e.target.files?.[0] || null; $("#wasm-file-name").textContent = f ? f.name : "(not selected)";
      if (!f) return; const bytes = new Uint8Array(await f.arrayBuffer()); setWasm(bytes, f.name || "main.wasm", "uploaded"); log("wasm uploaded: "+(f.name||"(unnamed)"), "ok");
    });

    // ==== Execution (minimal WASI: fd_write hook) ====
    async function runWasi() {
      if (!wasmBytes) return;
      try {
        setStatus("running ..."); $("#stdout").textContent = ""; $("#stderr").textContent = ""; $("#exit").textContent = "-"; autoScrollTo('#sec-run');
        let memory = null; const out = { 1: [], 2: [] }; const decoder = new TextDecoder();
        function fd_write(fd, iovs_ptr, iovs_len, nwritten_ptr) {
          const mem = new DataView(memory.buffer); let written = 0;
          for (let i = 0; i < iovs_len; i++) { const base = iovs_ptr + i * 8; const ptr = mem.getUint32(base, true); const len = mem.getUint32(base + 4, true); const bytes = new Uint8Array(memory.buffer, ptr, len); out[fd]?.push(decoder.decode(bytes)); written += len; }
          if (nwritten_ptr) mem.setUint32(nwritten_ptr, written, true); return 0;
        }
        const imports = { wasi_snapshot_preview1: { fd_write } };
        const { instance } = await WebAssembly.instantiate(wasmBytes, imports);
        memory = instance.exports.memory;
        if (typeof instance.exports._start === "function") instance.exports._start(); else if (typeof instance.exports._main === "function") instance.exports._main(); else throw new Error("Neither _start nor _main found");
        $("#stdout").textContent = (out[1]||[]).join(""); $("#stderr").textContent = (out[2]||[]).join(""); $("#exit").textContent = "0"; setStatus("done"); autoScrollTo('#sec-log');
      } catch (e) { console.error(e); log("Run error: "+String(e), "err"); autoScrollTo('#sec-log');
        setStatus("run error"); }
    }
    $("#run").addEventListener("click", runWasi);

    // ==== Self-tests (additional tests) ====
    async function runTests() {
      if (!wabtReady) { log("[TEST] wabt not loaded", "err"); return; }
      log("[TEST] start ...");
      // Test 1: Hello WAT -> assemble -> run -> stdout match
      try {
        const wat = document.getElementById("wat-source").textContent.trim();
        const m = wabt.parseWat("t1", wat, {}); const bin = m.toBinary({ log:false, write_debug_names:true }); m.destroy();
        const bytes = bin.buffer; const stdout = await (async () => {
          let memory=null; const acc={1:[],2:[]}; const dec=new TextDecoder();
          function fd_write(fd,iovs,iovlen,nw){ const dv=new DataView(memory.buffer); let w=0; for(let i=0;i<iovlen;i++){ const b=iovs+i*8; const p=dv.getUint32(b,true); const l=dv.getUint32(b+4,true); const byt=new Uint8Array(memory.buffer,p,l); acc[fd]?.push(dec.decode(byt)); w+=l;} if(nw) dv.setUint32(nw,w,true); return 0; }
          const { instance } = await WebAssembly.instantiate(bytes,{wasi_snapshot_preview1:{fd_write}}); memory=instance.exports.memory; instance.exports._start(); return (acc[1]||[]).join("");
        })();
        const expected = "Hello, WAT!\n"; if (stdout===expected) log("[TEST1] stdout equality - PASS", "ok"); else log(`[TEST1] stdout equality - FAIL (expected "${expected}", got "${stdout}")`, "err");
      } catch (e) { log("[TEST1] stdout equality - ERROR: "+String(e), "err"); }
      // Test 2: Syntax error detection
      try {
        const bad = "(module (func (export \"_start\") unreachable)"; let threw=false; try { wabt.parseWat("t2", bad, {}); } catch (_) { threw=true; }
        if (threw) log("[TEST2] syntax error detection - PASS", "ok"); else log("[TEST2] syntax error detection - FAIL (no error thrown)", "err");
      } catch (e) { log("[TEST2] syntax error detection - ERROR: "+String(e), "err"); }
      log("[TEST] done.");
    }
    $("#tests").addEventListener("click", runTests);

    // Download
    $("#dl-wasm").addEventListener("click", () => { if (wasmBytes) download(wasmName||"main.wasm", wasmBytes); });

    // Initial state
    $("#asm-status").textContent = "-";
    // Auto-execute CDN load on startup
    loadWabtFromCDN();
  </script>
</body>
</html>

```

---

## 6. Operation Verification (Checklist)
1) Open page -> Top status becomes **loaded** (CDN success).  
2) **Assemble** -> **WASM size / SHA-256** updated, **Download** becomes available.  
3) **Run** -> Expected string displayed in `stdout`, Exit code=0.  
4) **Run self-tests** ->  
   - `[TEST1] stdout equality - PASS`  
   - `[TEST2] syntax error detection - PASS`  
5) Works even when selecting existing `main.wasm` and **Run**.

---

## 7. Known Limitations and Troubleshooting
- **Cannot execute when all CDNs are blocked.** (This template doesn't have local loading)  
- String length mistake (`iov.len`) can cause **FAIL despite visual match** (NUL insertion etc.).  
- `"Neither _start nor _main found"`: Possibly not exporting `_start` in WAT.  
- **HTTPS delivery recommended** to reduce browser differences (often works with `file://` but environment-dependent).
