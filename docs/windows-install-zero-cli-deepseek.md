# Install Zero CLI on Windows and configure DeepSeek (beginner-friendly)

**Languages:** English (this page) · [简体中文](windows-install-zero-cli-deepseek.zh.md)

---

For readers who have **never used a terminal** and **do not yet have Node.js, npm, or Bun** installed. Follow the steps in order; if something fails, start with [Common issues](#common-issues).

---

## What you get

- **Zero CLI** in your terminal (command name: `zero`)
- Calls to the **official DeepSeek** Anthropic-compatible endpoint (`https://api.deepseek.com/anthropic`) using **your API key**

This guide installs Zero CLI with **npm bundled with Node.js**. You do **not** need Bun.

---

## Step 1: Install Node.js (includes npm)

Zero CLI requires **Node.js 18 or newer**. Install from the official site; that also installs **npm**.

1. Open **https://nodejs.org/** in a browser.
2. Download the Windows **LTS** installer (`.msi`).
3. Run it and accept the defaults (Next). If the wizard offers **“Automatically install the necessary tools”**, enabling it is usually fine.
4. **Restart the PC** once (or sign out and back in) so the terminal picks up `PATH` changes.

---

## Step 2: Open a terminal

Pick any of these.

### Option A: Windows Terminal (“Terminal”)

1. Press **Win**.
2. Type **Terminal**.
3. Open **Terminal** / **Windows Terminal**.

### Option B: PowerShell

1. Press **Win**.
2. Type **PowerShell**.
3. Open **Windows PowerShell** (or a PowerShell tab inside Terminal).

### Option C: Command Prompt (cmd)

1. Press **Win + R**.
2. Type `cmd`, press Enter.

**Note:** Installing Zero CLI does **not** require Administrator rights. Open the terminal normally unless you know you need elevation.

---

## Step 3: Verify Node and npm

Type each command below and press **Enter** after each line:

```text
node -v
```

You should see something like `v22.x.x` or `v20.x.x` (any **18+** is fine).

```text
npm -v
```

You should see an npm version (e.g. `10.x.x`).

If you see **“is not recognized”**, `PATH` may not be updated yet: **close every terminal**, open a new one, and try again. If it still fails, reboot.

---

## Step 4: Install Zero CLI globally

```bash
npm install -g @paean-ai/zero-cli
```

Wait until it finishes. If there is no red error at the end, it usually succeeded.

Check:

```bash
zero --version
```

You should see a version number.

---

## Step 5: Create a DeepSeek API key

1. Open **https://platform.deepseek.com/api_keys**
2. Sign in to the DeepSeek console.
3. Create an **API key**, copy it, and store it somewhere safe. **Do not** paste it into public chats, screenshots, or git repositories.

Official keys page: https://platform.deepseek.com/api_keys

---

## Step 6: Register DeepSeek with Zero CLI

Run (replace `YOUR_DEEPSEEK_API_KEY` with your real key — typically **no quotes** needed; never share the key):

```bash
zero provider set deepseek --api-key YOUR_DEEPSEEK_API_KEY
```

This writes settings and cache on your machine.

Optional checks:

```bash
zero provider
zero provider status --json
```

Notes:

- Config and cache usually live under a **`.zero`** folder in your profile (e.g. `C:\Users\YourName\.zero`). You do not need to edit files unless you know what you are doing.
- If you previously used **Paean login** and then set DeepSeek, whatever **`zero provider`** shows as active wins. To switch back, see **`zero provider clear`** in the root [`README.md`](../README.md).

---

## Step 7: Start Zero CLI

Go to the folder where you want to work (example: Documents).

**PowerShell:**

```powershell
cd $env:USERPROFILE\Documents
```

**Command Prompt (cmd):**

```bat
cd %USERPROFILE%\Documents
```

Then:

```bash
zero
```

Follow any on-screen prompts for permissions or network.

Handy commands:

```bash
zero provider list
zero provider use deepseek
```

`use` switches using **cached** credentials after you have run `set` at least once — no need to type the key again.

---

## Model tiers (optional)

Zero maps **pro / flash / lite** (aliases **opus / sonnet / haiku**) to provider models. The DeepSeek preset uses **flash** as the daily-driver default; see the DeepSeek table in the root [`README.md`](../README.md).

For scripting or non-interactive use, see [`README.md`](../README.md) and `zero --help`.

---

## Common issues

### 1. `zero` is not recognized

npm’s global bin folder may be missing from `PATH`. Try:

```bash
npm bin -g
```

If that fails on newer npm:

```bash
npm config get prefix
```

Executables are often under `%AppData%\npm` or `<prefix>\bin`. Add the folder to **Environment variables → Path**, then **open a new terminal**.

Temporary test (adjust path if yours differs):

```bash
"%AppData%\npm\zero.cmd" --version
```

### 2. Permission errors on `npm install -g`

Avoid forcing installs with Administrator unless you understand the consequences. On locked-down PCs, ask IT or configure a user-level npm prefix (advanced).

### 3. Corporate proxy blocks npm

Configure a proxy for npm or Windows; search for “npm proxy configuration” with the address your network team provides.

### 4. DeepSeek requests fail

- Key copied fully (no extra spaces or line breaks).
- System clock is correct.
- Retry later; check quotas and alerts in the DeepSeek console.

### 5. Supplying the key via environment variables (scripts / CI)

See **`ZERO_PROVIDER_API_KEY`** in the root [`README.md`](../README.md). Restart the terminal or Zero after changing env vars.

---

## Uninstall (optional)

```bash
npm uninstall -g @paean-ai/zero-cli
```

Your `.zero` directory remains until you delete it in File Explorer (exit Zero CLI first).

---

## Further reading

- Overview and providers: root [`README.md`](../README.md)
- Product and brand rules (upstream): [`docs/zero-cli-spec.md`](https://github.com/a8e-ai/zero-cli/blob/main/docs/zero-cli-spec.md)
