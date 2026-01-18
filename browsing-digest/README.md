# 📚 Browsing Digest

**An AI that summarizes your daily browsing into a 2-minute digest. 100% offline, 100% private.**

Ever wondered "What did I even read today?" This tool captures what you browse and uses a local AI to create a daily summary — no cloud, no tracking, no data leaving your machine.

![Demo](https://via.placeholder.com/800x400?text=Browsing+Digest+Demo)

## Features

- 🔒 **100% Private**: Everything runs locally. Your browsing data never leaves your machine.
- 🤖 **AI-Powered Summaries**: Uses Ollama with Llama 3.2 (or any local model) to generate insights.
- ⚡ **Lightweight**: Minimal extension that doesn't slow down your browser.
- 📊 **Smart Extraction**: Automatically extracts main content, skips ads and navigation.
- 📅 **Daily Digests**: Export any day's browsing and get a structured summary.

## How It Works

1. **Chrome Extension** captures page titles, URLs, and main content as you browse
2. **Export** your browsing data for any date (stored locally in browser)
3. **Python Script** sends the data to your local Ollama instance
4. **Get a Digest** with themes, insights, and action items

## Installation

### Prerequisites

1. **Ollama** - Install from [ollama.ai](https://ollama.ai)
   ```bash
   # macOS
   brew install ollama
   
   # Linux
   curl -fsSL https://ollama.ai/install.sh | sh
   ```

2. **Pull a model** (Llama 3.2 3B recommended):
   ```bash
   ollama pull llama3.2
   ```

3. **Start Ollama**:
   ```bash
   ollama serve
   ```

### Chrome Extension

1. Open Chrome and go to `chrome://extensions/`
2. Enable **Developer mode** (toggle in top right)
3. Click **Load unpacked**
4. Select the `extension` folder from this project
5. Pin the extension to your toolbar for easy access

### Python Summarizer

```bash
cd summarizer
python3 summarize.py --help
```

No pip dependencies required — uses only Python standard library!

## Usage

### 1. Browse the Web

Just browse normally. The extension automatically captures pages in the background.

### 2. Export Your Data

1. Click the Browsing Digest extension icon
2. Select a date (defaults to today)
3. Click **Export Today's Browsing**
4. Save the JSON file

### 3. Generate Your Digest

```bash
cd summarizer
python3 summarize.py browsing-digest-2025-01-19.json
```

Options:
```bash
# Use a different model
python3 summarize.py data.json --model mistral

# Custom output file
python3 summarize.py data.json --output my-digest.md

# List available models
python3 summarize.py --list-models
```

### 4. Read Your Digest

Open the generated markdown file. Example output:

```markdown
# 📚 Browsing Digest - 2025-01-19

## Main Themes
- Local AI development and optimization
- Fine-tuning techniques for small models
- AI productivity tools

## Key Insights
- 4-bit quantization preserves 95% quality with 4x memory reduction
- QLoRA is recommended for most fine-tuning use cases
- Local LLMs can replace cloud APIs for many tasks

## Action Items
- Try the cool-project toolkit for model deployment
- Experiment with QLoRA fine-tuning
```

## Project Structure

```
browsing-digest/
├── extension/           # Chrome extension
│   ├── manifest.json    # Extension config
│   ├── background.js    # Storage & export logic
│   ├── content.js       # Page content extraction
│   ├── popup.html       # Extension UI
│   ├── popup.js         # UI interactions
│   └── icons/           # Extension icons
│
├── summarizer/          # Python summarizer
│   ├── summarize.py     # Main script
│   ├── requirements.txt # Dependencies (none!)
│   └── sample-data.json # Test data
│
└── README.md
```

## Configuration

### Extension Settings

The extension automatically:
- Skips localhost, chrome://, and other non-web pages
- Waits 2 seconds for pages to load before capturing
- Limits content to 5000 characters per page
- Keeps last 500 pages in storage

To modify these, edit `content.js` and `background.js`.

### Model Selection

Recommended models for this task:

| Model | Size | Speed | Quality |
|-------|------|-------|---------|
| llama3.2 (default) | 3B | Fast | Good |
| llama3.2:1b | 1B | Very Fast | Acceptable |
| mistral | 7B | Medium | Better |
| phi3 | 3.8B | Fast | Good |

```bash
# Pull alternative models
ollama pull mistral
ollama pull phi3
ollama pull llama3.2:1b
```

## Privacy

This tool is designed with privacy as a core principle:

- ✅ All data stays on your machine
- ✅ No external API calls
- ✅ No tracking or analytics
- ✅ Extension uses only local storage
- ✅ You control when/what to export

The only network requests are to the websites you visit (normal browsing).

## Troubleshooting

### Extension not capturing pages

1. Check if the extension is enabled in `chrome://extensions/`
2. Refresh the page — content script loads on navigation
3. Check the console for errors (right-click extension → Inspect)

### Ollama not responding

```bash
# Check if running
ollama list

# Restart
pkill ollama
ollama serve
```

### Model too slow

Try a smaller model:
```bash
ollama pull llama3.2:1b
python3 summarize.py data.json --model llama3.2:1b
```

### Out of memory

1. Close other applications
2. Use a quantized model: `ollama pull llama3.2:1b`
3. Reduce content in export (edit JSON to fewer pages)

## Automation (Optional)

### Daily Cron Job

Generate digest every night at 11 PM:

```bash
# Add to crontab
crontab -e

# Add this line (adjust paths)
0 23 * * * cd /path/to/summarizer && python3 summarize.py ~/Downloads/browsing-digest-$(date +\%Y-\%m-\%d).json -o ~/Documents/digests/$(date +\%Y-\%m-\%d).md
```

### Bash Alias

```bash
# Add to ~/.bashrc or ~/.zshrc
alias digest='python3 /path/to/summarizer/summarize.py'

# Usage
digest ~/Downloads/browsing-digest-2025-01-19.json
```

## Contributing

PRs welcome! Some ideas:

- [ ] Firefox extension support
- [ ] Auto-export at end of day
- [ ] Weekly/monthly rollup summaries
- [ ] Integration with note-taking apps
- [ ] Custom prompt templates

## License

MIT License. Use it, modify it, share it.

## Acknowledgments

- [Ollama](https://ollama.ai) for making local LLMs accessible
- [Llama](https://ai.meta.com/llama/) by Meta AI
- The local-first AI community

---

**Made with ❤️ for people who value privacy**

*Star this repo if you found it useful!*
