# AI Web Research Assistant

A Python web-research tool that searches DuckDuckGo, scrapes text from the top four organic results, and uses a local Ollama model (`qwen3.5:9b`) to generate a factual summary.

## Features

- Searches the web using DuckDuckGo.
- Extracts up to four unique organic result links.
- Scrapes readable text from each website.
- Removes common page elements such as scripts, navigation, headers, and footers.
- Uses Ollama to summarize findings and compare information across sources.
- Includes the current date and time in the AI prompt.
- Limits the amount of text collected and sent to the model.

## Requirements

- Python 3.10+
- Ollama installed and running locally
- The `qwen3.5:9b` model
- Playwright and Chromium

## Installation

Install the Python dependencies:

```bash
pip install playwright aiohttp
playwright install chromium
```

Download the model:

```bash
ollama pull qwen3.5:9b
```

Make sure Ollama is running at `http://127.0.0.1:11434`.

## Configuration

The main settings are in `main.py`:

```python
URL = "https://duckduckgo.com/"
OLLAMA_URL = "http://127.0.0.1:11434/api/generate"
OLLAMA_MODEL = "qwen3.5:9b"

MAX_LINKS = 4
MAX_CHARS_PER_SITE = 10000
MAX_TOTAL_CHARS = 30000
```

- `MAX_LINKS`: Maximum number of search results to scrape.
- `MAX_CHARS_PER_SITE`: Maximum characters collected from each website.
- `MAX_TOTAL_CHARS`: Maximum combined text sent to Ollama.

## Change the Browser

The script currently launches Chromium:

```python
browser = await p.chromium.launch(
    headless=False,
    args=[
        # existing Chromium arguments
    ]
)
```

To use Firefox, install its browser binary:

```bash
playwright install firefox
```

Then change `p.chromium.launch(...)` to:

```python
browser = await p.firefox.launch(headless=False)
```

For WebKit, install it with `playwright install webkit` and use `p.webkit.launch(...)`. Browser-specific launch arguments may not work across browsers, so remove or adjust the Chromium-specific `args` when switching.

## Headless Toggle

The `headless` setting controls whether the browser window is visible:

```python
browser = await p.chromium.launch(
    headless=False
)
```

- `headless=False`: Shows the browser window, useful for watching or debugging the search and scraping process.
- `headless=True`: Runs the browser in the background without showing a window.

Change only the value of `headless` to switch modes.

## Run

Start the script:

```bash
python main.py
```

Enter a search query when prompted. The program searches DuckDuckGo, selects up to four organic results, scrapes their page text, and prints an Ollama-generated summary.

## Workflow

1. Enter a search query.
2. Search DuckDuckGo with Playwright.
3. Extract and deduplicate organic result URLs.
4. Scrape readable text from up to four websites.
5. Send the collected text to Ollama.
6. Print the summary and key findings.

## Limitations

- Some websites block automated browsing or require extra interaction.
- The tool processes only the top four organic results.
- Summary quality depends on the text successfully scraped.
- Ollama must be running locally for summarization.

## Technologies

Python · Playwright · DuckDuckGo · Ollama · Qwen 3.5 · aiohttp
