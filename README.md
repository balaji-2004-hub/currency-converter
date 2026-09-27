# Currency Converter

A simple responsive currency converter built with **HTML, CSS, and JavaScript**.

## Features

- Convert an amount between currencies.
- Currency dropdowns are populated from `js/country-list.js`.
- Currency flags update when a currency is selected.
- Swap From/To currencies with the exchange icon.
- Fetches the latest exchange rate from the ExchangeRate-API Open endpoint.
- Responsive layout for desktop and mobile.
- No API key is required for the current endpoint used by this project.

## Project Structure

```text
currency-converter/
├── index.html
├── style.css
├── README.md
└── js/
    ├── country-list.js
    └── script.js
```

## How to Run

### Option 1: VS Code + Live Server (recommended)

1. Extract the project ZIP.
2. Open the extracted `currency-converter` folder in VS Code.
3. Install the **Live Server** extension if it is not already installed.
4. Right-click `index.html`.
5. Select **Open with Live Server**.
6. The application will open in your browser.

Typical URL:

```text
http://127.0.0.1:5500/index.html
```

The exact port can be different if Live Server selects another available port.

### Option 2: Python local server

If Python is installed, open a terminal inside the project folder and run:

```bash
python -m http.server 5500
```

Then open:

```text
http://localhost:5500
```

### Option 3: Node.js local server

If you have Node.js installed, you can use any static HTTP server. For example, with `serve`:

```bash
npx serve .
```

Then open the URL printed in the terminal.

## Important

Do not open the HTML file with `file:///...` if the browser blocks requests. Run it through a local HTTP server such as Live Server or Python's `http.server`.

## What Was Fixed

- Reconstructed the project from the supplied nested ZIP into a normal runnable folder.
- Restored the expected file names and folder structure.
- Fixed the broken/incomplete URL strings in the outer copy by using the working JavaScript from the nested project.
- Removed the placeholder `YOUR_API_KEY` dependency by using the ExchangeRate-API Open endpoint.
- Added HTTP/API response validation and clearer error handling.
- Added this README with clear execution instructions.
- Kept the existing UI, currency list, flag handling, swap behavior, and conversion flow intact.

## Troubleshooting

### `python is not recognized`

Install Python and enable **Add Python to PATH** during installation, then reopen the terminal.

### `npx is not recognized`

Install Node.js, then reopen VS Code/Terminal.

### Exchange rate says it cannot be loaded

Check that the browser has internet access and that the application is being served through `http://localhost` or Live Server rather than opened directly as a local `file://` page.

### Flags are not displayed

Check your internet connection because the flags are loaded from FlagCDN.

## Technologies

- HTML5
- CSS3
- JavaScript (ES6+)
- Fetch API
- Font Awesome
- FlagCDN
- ExchangeRate-API Open endpoint
