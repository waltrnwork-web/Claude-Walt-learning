---
name: canary
description: Run a quick canary test to check that the current build is alive. Use when the user says "run canary", "canary test", "check the canary", "is the build alive", or otherwise asks for a basic smoke check that the project still loads or runs. Finds an existing canary file in the repo root (canary.html, canary.py, canary.js, or similar), creates the simplest one for the project type if none exists, runs it, and reports CANARY: PASS or CANARY: FAIL. If the canary fails, all other work stops until the user has been told what is broken. Only for the canary check, not for fixing failures or running full test suites.
---

# Canary

A canary is a tiny check that answers one question: does the build load or run at all? It prints or shows exactly `CANARY: PASS` or `CANARY: FAIL`.

## 1. Find an existing canary

Look in the repo root only for a file named `canary.*` (for example `canary.html`, `canary.py`, `canary.js`, `canary.sh`):

```bash
ls canary.* 2>/dev/null
```

If one exists, use it as is and go to step 3. Don't rewrite it. If there are several, run each one.

## 2. Create a canary if none exists

Work out the project type from the repo root, then create the simplest canary for it.

- **Web**: the repo has an `index.html` (or a similar main page) and no code entry point. Create `canary.html`.
- **Script/code**: the repo has a main entry point, such as `main.py`, `app.py`, `index.js`, a `package.json` `main` field, or a `pyproject.toml` package. Create a canary in that language (`canary.py`, `canary.js`, and so on).
- **Neither**: there's nothing to check. Tell the user there's no main page or entry point, and stop without creating a file.

### Web: canary.html

Use this template. Set `PAGE` to the main page's file name and `SELECTOR` to an element that must be on that page when it works (by default `h1`; if the main page has no `h1`, pick a selector for its main content, such as `main` or `#app`).

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Canary</title>
  <style>
    body { font-family: system-ui, sans-serif; display: grid; place-items: center; min-height: 100vh; margin: 0; background: #f5f6f8; }
    #result { font-size: 2.5rem; font-weight: 700; }
    .pass { color: #1e7b34; }
    .fail { color: #b42318; }
  </style>
</head>
<body>
  <div id="result">CANARY: …</div>
  <script>
    const PAGE = "index.html";
    const SELECTOR = "h1";
    const out = document.getElementById("result");
    function show(ok, why) {
      out.textContent = ok ? "CANARY: PASS" : "CANARY: FAIL";
      out.className = ok ? "pass" : "fail";
      if (!ok && why) out.title = why;
      document.title = out.textContent;
    }
    fetch(PAGE)
      .then(r => { if (!r.ok) throw new Error(PAGE + " returned HTTP " + r.status); return r.text(); })
      .then(html => {
        const el = new DOMParser().parseFromString(html, "text/html").querySelector(SELECTOR);
        if (el && el.textContent.trim()) show(true);
        else show(false, "No content found for " + SELECTOR + " in " + PAGE);
      })
      .catch(e => show(false, e.message));
  </script>
</body>
</html>
```

### Script/code: canary script

Keep it to a few lines: import or call the main entry point, print `CANARY: PASS` if that works, otherwise print `CANARY: FAIL` with the error, and exit non-zero on failure. Don't call anything with side effects, such as network calls, writes or long-running servers. If the only entry point does those, just import the module. For example, for Python with `main.py`:

```python
import sys
try:
    import main  # noqa: F401
    print("CANARY: PASS")
except Exception as e:
    print(f"CANARY: FAIL ({type(e).__name__}: {e})")
    sys.exit(1)
```

## 3. Run the canary

- **canary.html**: browsers block `fetch` on `file://` pages, so serve the repo root over HTTP and read the result with headless Chromium (Playwright is pre-installed in cloud sessions):

  ```bash
  python3 -m http.server 8765 --bind 127.0.0.1 >/dev/null 2>&1 &
  SERVER=$!
  sleep 1
  node -e '
  const { chromium } = require("playwright");
  (async () => {
    const b = await chromium.launch();
    const p = await b.newPage();
    await p.goto("http://127.0.0.1:8765/canary.html");
    await p.waitForFunction(() => /PASS|FAIL/.test(document.getElementById("result")?.textContent || ""), null, { timeout: 10000 }).catch(() => {});
    const text = await p.textContent("#result").catch(() => "");
    const why = await p.getAttribute("#result", "title").catch(() => null);
    console.log(/CANARY: (PASS|FAIL)/.test(text) ? text : "CANARY: FAIL (canary page did not report a result)");
    if (why) console.log("Reason: " + why);
    await b.close();
  })();'
  kill $SERVER
  ```

  If the canary page has no `#result` element (a custom canary), report whatever `CANARY: PASS` or `CANARY: FAIL` text appears on the page instead.

- **Scripts**: run them from the repo root with the matching interpreter, for example `python3 canary.py`, `node canary.js` or `sh canary.sh`.

A canary passes only if its output says `CANARY: PASS` (and, for scripts, it exits with 0). Anything else, including a crash, a timeout or no output, is a fail.

## 4. Report the result

Start the reply with the result on its own line: **CANARY: PASS** or **CANARY: FAIL**. Then add one line saying which canary ran and whether you created it this time.

If you created a new canary file, commit only that file and push the current branch:

```bash
git add canary.<ext>
git commit -m "Add canary check"
git push -u origin "$(git branch --show-current)"
```

## 5. If the canary fails, stop

Don't continue with any other work, including the rest of the user's request. Tell the user what is broken, using the canary's output: which file or entry point failed, and the error or missing content. Then wait for the user to decide what to do. Don't try to fix it unless they ask.
