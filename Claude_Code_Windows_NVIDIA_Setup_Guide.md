# Comprehensive Windows Setup Guide: Claude Code via NVIDIA NIM API Proxy

This guide provides a detailed, step-by-step deployment matrix to run **Anthropic's Claude Code** command-line interface (CLI) on a **Windows** environment, routing all requests completely through your custom **NVIDIA NIM API Key** utilizing the **`z-ai/glm-5.3`** model architecture.

---

## 🏗️ 1. Architecture Overview
Because the native Claude Code tool communicates exclusively using the **Anthropic Messages API structure**, it cannot natively speak to OpenAI-compatible endpoints like NVIDIA NIM. 

To bridge this language gap, we deploy a lightweight, local translation engine (**`free-claude-code`**) running right on your loopback interface.

```
[ Your Terminal ] ──> ( Claude Code CLI Tool )
                             │ (Sends Anthropic Formatted Request)
                             ▼
                      [ fcc-server (Port 8082) ]  <-- Local Proxy Bridge
                             │ (Translates to OpenAI Formatted Request)
                             ▼
                      [ NVIDIA NIM API Gateway ]
                             │ (Processes execution using z-ai/glm-5.3)
                             ▼
                      [ Your Screen ] <-- Returns Output Context & Thought Blocks
```

---

## 📋 2. Prerequisites
Before executing the deployment, ensure your Windows machine has the following foundational dependencies correctly installed and mapped to your system's global `Path`:

1. **Node.js (v18.0.0+)** - Required to execute the core Javascript application layer of Claude Code.
   * [Download Node.js for Windows](https://nodejs.org/)
2. **Python (v3.10+)** - Required to install and host the background proxy engine.
   * [Download Python for Windows](https://www.python.org/) *(Ensure you check the box for **"Add python.exe to PATH"** during installation!)*

---

## 🛠️ 3. Step-by-Step Deployment Matrix

### Step 1: Install the Core Claude Code Package Globally
Open a standard Windows **PowerShell** or **Command Prompt (cmd)** window and run the following command to download and mount the official CLI:
```cmd
npm install -g @anthropic-ai/claude-code
```

### Step 2: Install the Isolated Proxy Engine
Using Python's package manager, install the global adapter wrapper application:
```cmd
pip install free-claude-code
```

### Step 3: Launch the Background Proxy Server
Initialize the translation endpoint daemon, passing the explicit provider routing target parameter:
```cmd
fcc-server --provider nvidia
```
*⚠️ CRITICAL: Keep this terminal window open in the background! It acts as your active local database and routing pipeline gateway.*

### Step 4: Configure the Web UI Dashboard
Once `fcc-server` is up, it spins up an administrative configuration node on your loopback system.
1. Open any web browser and navigate directly to: **`http://127.0.0.1:8082`**
2. Locate the **NVIDIA NIM** parameters module.
3. Paste your secret credential key into the field: `nvapi-.........`
4. Select the explicit target model variant: **`z-ai/glm-5.3`**
5. Click **Save** on the floating credential modal box.
6. Scroll down to the bottom right edge of the central dashboard web page and click the main **Apply** button to commit your environment variables locally.

---

## 🚀 4. Initializing the Terminal Shell
Open a **brand new, separate Windows PowerShell window** to execute your operations workspace. Drop in the following script framework to intercept the standard Anthropic software tracking loop and override it with your custom local port settings:

```powershell
# 1. Force the client to direct all outbound payloads to your proxy bridge port
$env:ANTHROPIC_BASE_URL="http://127.0.0.1:8082"

# 2. Provide your validation token matrix
$env:ANTHROPIC_API_KEY="nvapi-rxiV9Mr3Qhd5bWFdjRdWrh2c8s"

# 3. Purge standard console cookie session caching variables
Remove-Item Env:\ANTHROPIC_AUTH_TOKEN -ErrorAction SilentlyContinue

# 4. Boot up Claude Code explicitly calling the NVIDIA model path string
claude --model z-ai/glm-5.3
```

Once initialized, type `hello` or pass a test query like `Which number is larger, 9.11 or 9.8?` to see the logic thinking block display natively inside your prompt terminal window.

---

## 🔍 5. Troubleshooting Diagnostics Matrix

| Observable Symptom / Error | Root Cause Analysis | Actionable Resolution Step |
| :--- | :--- | :--- |
| **`ECONNREFUSED (127.0.0.1:8082)`** | The `fcc-server` utility process has crashed, been closed, or is blocked by Windows Firewall. | Go to your background shell and re-execute `fcc-server --provider nvidia`. Verify the port logs print properly. |
| **`Command not found / Not recognized`** | The Python scripts folder has not been indexed inside your system's global environment variables. | Close terminal, run `setx PATH "%APPDATA%\Python\Python311\Scripts;%PATH%"` (adjust for your specific Python version), and restart the shell. |
| **`401 Unauthorized Response`** | The API key entered carries trailing whitespace formatting, or the web configuration page was not finalized. | Re-open `http://127.0.0.1:8082`, verify your key character array matches exactly, and make sure to hit the primary **Apply** button at the bottom page layer. |

---

## 💡 6. Daily Workflow Automation Tip
To skip copy-pasting the environment blocks every time you load your terminal, save the configuration as a reusable script wrapper. Create a file named `launch-claude.ps1` in your project folder, paste the code blocks from **Section 4** into it, and execute it anytime with:
```powershell
.\launch-claude.ps1
```
