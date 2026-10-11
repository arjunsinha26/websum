 <div align="center">

# WebSum

### AI-Powered Web Research, Summarized Locally

**Search the web. Extract useful content. Generate a source-informed summary with your local Ollama model.**

<br>

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-Browser_Automation-2EAD33?style=for-the-badge&logo=playwright&logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-Local_AI-111111?style=for-the-badge)
![DuckDuckGo](https://img.shields.io/badge/Search-DuckDuckGo-DE5833?style=for-the-badge)

<br>

[Features](#-features) · [How It Works](#-how-it-works) · [Installation](#-installation) · [Configuration](#-configuration) · [Limitations](#-limitations) · [License](#-license)

</div>

---

## 🔎 Overview

**WebSum** is a Python-based web research assistant that searches DuckDuckGo, extracts readable text from selected search results using Playwright, and asks a locally running [Ollama](https://ollama.com/) model to summarize the collected information.

The goal is to make it easier to review information across several pages without manually opening and reading every result.

> **Privacy-minded by design:** The summarization request is sent to your local Ollama API. Web searches and website visits still connect to external services.

## ✨ Features

| Feature | Description |
|---|---|
| 🌐 Web search | Searches DuckDuckGo for your query |
| 🔗 Result collection | Collects up to four unique organic result links |
| 🧭 Browser automation | Uses Playwright with Chromium to open result pages |
| 🧹 Text extraction | Removes common page elements before collecting readable text |
| 🧠 Local AI summary | Sends collected text to a local Ollama model |
| 🗂️ Multi-source analysis | Prompts the model to summarize findings and compare sources |
| 📏 Text limits | Caps text collected per site and across all sources |
| 🖥️ Browser visibility | Supports visible or headless Chromium operation |

## ⚙️ How It Works

```mermaid
flowchart TD
    A[Enter a search query] --> B[Search DuckDuckGo]
    B --> C[Collect up to 4 organic links]
    C --> D[Open pages with Playwright]
    D --> E[Extract and clean page text]
    E --> F[Send text to local Ollama]
    F --> G[Generate and print summary]
```

1. Enter a search query in the terminal.
2. WebSum searches DuckDuckGo.
3. It collects up to four unique organic result links.
4. Playwright opens each page and extracts readable text.
5. The extracted text is sent to the local Ollama API.
6. The model generates a summary, which is printed in the terminal.

## 🧰 Tech Stack

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat-square&logo=playwright&logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-000000?style=flat-square)
![DuckDuckGo](https://img.shields.io/badge/DuckDuckGo-DE5833?style=flat-square)
![aiohttp](https://img.shields.io/badge/aiohttp-Async_HTTP-blue?style=flat-square)

</div>

- **Python** — core application logic and asynchronous execution
- **Playwright** — browser automation and website text extraction
- **playwright-stealth** — stealth-oriented browser configuration
- **aiohttp** — asynchronous requests to Ollama
- **DuckDuckGo** — web search
- **Ollama** — local language-model inference

## 📋 Requirements

- **Python 3.10 or newer**
- [Ollama](https://ollama.com/) installed and running locally
- The `qwen3.5:9b` model available in Ollama
- Playwright with Chromium installed
- An active internet connection for searching and visiting websites

## 🚀 Installation

### 1. Clone the repository

```bash
git clone https://github.com/arjunsinha26/websum.git
cd websum
```

### 2. Install Python dependencies

```bash
python -m pip install aiohttp playwright playwright-stealth
```

### 3. Install Chromium for Playwright

```bash
python -m playwright install chromium
```

### 4. Download the Ollama model

```bash
ollama pull qwen3.5:9b
```

Start Ollama and make sure its local API is available at:

```text
http://127.0.0.1:11434
```

You can check which models are available by running:

```bash
ollama list
```

## ▶️ Usage

Run WebSum from the project directory:

```bash
python main.py
```

Enter your search query when prompted. The script will search, scrape the selected pages, and print the model-generated summary.

Example:

```text
Enter your search: Latest developments in renewable energy
```

WebSum then processes the selected search results and displays the generated summary in the terminal.

## 🎛️ Configuration

The implementation keeps its settings as constants near the top of `main.py`.

| Setting | Default | Purpose |
|---|---|---|
| `URL` | `https://duckduckgo.com/` | Search endpoint |
| `OLLAMA_URL` | `http://127.0.0.1:11434/api/generate` | Local Ollama generation endpoint |
| `OLLAMA_MODEL` | `qwen3.5:9b` | Model used for summarization |
| `MAX_LINKS` | `4` | Maximum number of result links |
| `MAX_CHARS_PER_SITE` | `10000` | Maximum characters collected from one page |
| `MAX_TOTAL_CHARS` | `30000` | Maximum combined source text sent for analysis |

Change these constants in `main.py` to adjust the current configuration.

### 🖥️ Show or hide Chromium

The script launches Chromium using the `headless` option.

To display the browser window:

```python
headless=False
```

To run without displaying the browser window:

```python
headless=True
```

Headless mode may behave differently on some websites. Search pages can change their layout or present verification challenges, so test both modes if you encounter issues.

## 📁 Project Structure

```text
websum/
├── main.py       # Main search, scraping, and summarization workflow
├── README.md     # Project documentation
└── LICENSE       # MIT License
```

## ⚠️ Limitations

- **Limited search coverage:** Only the first four unique organic results are collected by default.
- **Blocked websites:** Some sites may block automated browsing or require consent, verification, or additional interactions.
- **Extraction quality:** Complex pages may produce incomplete or irrelevant text.
- **Context window:** Characters are not tokens. The combined text and prompt may exceed the model's available context, depending on tokenization and model settings.
- **Summary accuracy:** Summaries depend on the pages successfully scraped and the model's interpretation. Check important claims against the original sources.
- **Local performance:** Generation speed depends on your hardware and the selected Ollama model.
- **Search-result changes:** Changes to DuckDuckGo's page structure may require updates to the selectors used by Playwright.

## 🛠️ Troubleshooting

<details>
<summary><strong>Ollama connection error</strong></summary>

1. Make sure Ollama is installed and running.
2. Check that `http://127.0.0.1:11434` is reachable locally.
3. Confirm the model is available:

   ```bash
   ollama list
   ```

4. Download it if necessary:

   ```bash
   ollama pull qwen3.5:9b
   ```

</details>

<details>
<summary><strong>Playwright cannot find Chromium</strong></summary>

Run:

```bash
python -m playwright install chromium
```

</details>

<details>
<summary><strong>A website returns little or no text</strong></summary>

The site may block automated access, require interaction, or render content in a way the current scraper does not capture. Check the browser for redirects, consent prompts, or verification pages.

</details>

<details>
<summary><strong>Ollama reports a context-length error</strong></summary>

Reduce `MAX_CHARS_PER_SITE` or `MAX_TOTAL_CHARS` in `main.py`. The prompt, source text, and generated response must all fit within the model's context window.

</details>

## 🔐 Responsible Use

Use WebSum only on websites you are permitted to access. Respect website terms, applicable robots directives, copyright, and rate limits. Do not use it to bypass access controls or collect private information.

## 📄 License

WebSum is licensed under the **MIT License**.

You are free to use, copy, modify, merge, publish, distribute, sublicense, and sell copies of the software, subject to the conditions of the license.

See the [LICENSE](LICENSE) file for the full license text.

## 🤝 Contributing

Bug reports, ideas, and improvements are welcome.

1. Fork the repository.
2. Create a branch for your changes.
3. Make and test your improvements.
4. Submit a pull request describing your changes.

For substantial changes, open an issue first to discuss the proposed approach.

---

<div align="center">

**Built with Python, Playwright, and local AI.**

If WebSum is useful to you, consider giving the repository a ⭐ on GitHub.

</div>
