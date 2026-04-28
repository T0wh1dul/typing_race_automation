# typing_race_automation

I built this small project to automate typing races on the Typeracer website. It's a single Jupyter notebook that launches a real browser, finds the race text, and types it in a human-like way using Playwright. The notebook includes a tiny UI (a WPM slider and a stop button) so you can control the bot while it runs.

This README explains what the project does, how it works, how to set it up, and how to run it — written in plain, simple language as if I (the author) were explaining it to you.

## What this does

- Opens a real browser (Chromium/Chrome) and goes to https://play.typeracer.com/
- Joins or starts a race when it can
- Reads the text to type from the page (it collects the visible spans and joins them)
- Types the text with delays, small mistakes, and jitter to look human
- Lets you control the target WPM with a slider and stop the bot with a button

## Files in this repository

- `typeracer_test.ipynb` — the main Jupyter notebook. Run this to start the automation.
- `README.md` — this file.

## How it works (brief, plain explanation)

I used Playwright inside a background thread to control the browser and `ipywidgets` to show a slider and a stop button in the notebook. When the engine finds the typing input box, it reads the visible text spans that make up the race text and then runs `simulate_human_typing()`.

`simulate_human_typing()` tries to mimic a human by:
- Using a target WPM (you set it with the slider)
- Ramping speed up at the start of a sentence instead of starting at full speed
- Introducing a small chance of a typo, then backspacing to correct it
- Adding a little random jitter to delays so timing varies naturally

There is a Windows-specific asyncio loop setup inside the Playwright thread to avoid event-loop errors on Windows.

## Requirements

- Python 3.8 or newer
- `pip` to install packages
- Google Chrome (recommended) or Chromium (Playwright can also download supported browsers)
- Jupyter Notebook or JupyterLab to open and run the `.ipynb` file

Python packages used (you can install them with pip):

```
pip install playwright ipywidgets jupyter
```

After installing Playwright, you must install the browsers it drives:

```
python -m playwright install
```

If you prefer to target your system Chrome instead of Playwright's bundled browsers, Playwright can be configured to use the `channel="chrome"` option (the notebook already tries that). Make sure Chrome is installed and up-to-date.

## Setup (quick start)

1. Clone this repo and open a terminal in the project folder.
2. (Optional but recommended) Create and activate a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1   # PowerShell on Windows
# or on Unix/macOS: source .venv/bin/activate
```

3. Install the Python packages:

```bash
pip install -r requirements.txt  # if you create this file
pip install playwright ipywidgets jupyter
python -m playwright install
```

4. Start Jupyter and open the notebook:

```bash
jupyter notebook typeracer_test.ipynb
```

5. Run the notebook cells. You will see a slider for target WPM and a Stop button. Use the slider to set the typing speed and click Stop to halt the engine.

## Running (tips)

- The notebook launches a visible browser by default (headful) so you can see what it does. That helps when debugging.
- If the browser closes or crashes, you can re-run the cells to relaunch it.
- If the input box does not appear immediately, the engine waits and retries; be patient if the page is slow.

## Customization

- Change the default WPM: edit `bot_state["wpm"]` in the notebook before starting the engine.
- Change typo chance: edit the `error_chance` parameter passed to `simulate_human_typing()` in the notebook.
- Make it headless: in the `run_playwright_engine()` function change `headless=False` to `True` (not recommended while testing).

## Troubleshooting

- Playwright errors: run `python -m playwright install` to make sure browsers are installed.
- Browser channel errors: if `channel="chrome"` fails, remove the channel argument so Playwright uses its bundled browser.
- If the notebook stalls on Windows with asyncio errors, re-run the notebook — the code sets a Windows Proactor event loop policy inside the worker thread which fixes that issue.

## Ethics & safety

This project automates typing on a public service. Please do not use it to cheat in competitive environments or to violate site terms of service. Use it only for learning, testing, or personal experiments where automation is allowed.

## If you want to help or send feedback

- Open an issue or send a pull request with improvements.
- If you find a bug, include the exact console output and the steps you took before it happened.

---

Thanks for checking out this notebook. Run `typeracer_test.ipynb` and tweak the slider — that's the quickest way to see how it works. If you'd like, I can turn this into a small CLI script or add a `requirements.txt` for you.


