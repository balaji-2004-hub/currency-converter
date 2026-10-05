# Currency Converter

A responsive, client-side **Currency Converter web application** built with **HTML5, CSS, and JavaScript**. The application consumes exchange-rate data from the **ExchangeRate-API Open endpoint** and dynamically updates conversion results based on user input and currency selection.

The project demonstrates practical front-end engineering concepts including **DOM manipulation, event-driven programming, asynchronous API consumption, JSON parsing, client-side validation, error handling, dynamic UI rendering, and responsive CSS design**.

---

## Project Overview

The application allows users to:

- Enter an amount to convert.
- Select a source (`From`) currency.
- Select a target (`To`) currency.
- Retrieve exchange-rate data through an external API.
- Calculate and display the converted amount.
- Automatically refresh the conversion when the amount or selected currencies change.
- Swap source and target currencies.
- Dynamically update currency flags.
- Use the application across desktop and mobile screen sizes.

---

## Core Technical Implementation

### 1. Event-Driven JavaScript

The application uses DOM event listeners to respond to user interactions:

- `input` event → recalculates the conversion when the amount changes.
- `change` event → updates the selected currency/flag and recalculates the conversion.
- `click` event → triggers manual conversion and currency swapping.
- `load` event → initializes the first exchange-rate request.

Example implementation:

```javascript
document.querySelector("form input").addEventListener("input", getExchangeRate);

fromCurrency.addEventListener("change", getExchangeRate);

toCurrency.addEventListener("change", getExchangeRate);
```

This provides a more interactive user experience without requiring a page reload.

---

## API Integration

The application integrates the **ExchangeRate-API Open endpoint**:

```text
https://open.er-api.com/v6/latest/{BASE_CURRENCY}
```

For example:

```text
https://open.er-api.com/v6/latest/USD
```

The base currency is dynamically generated from the selected `From` currency:

```javascript
const url = `https://open.er-api.com/v6/latest/${fromCurrency.value}`;
```

### API Request Flow

```text
User selects From currency
          |
          v
Construct API endpoint
          |
          v
Fetch API using Fetch API
          |
          v
Receive JSON response
          |
          v
Validate HTTP response
          |
          v
Validate API response
          |
          v
Extract target currency rate
          |
          v
Calculate converted amount
          |
          v
Update DOM
```

The current Open endpoint used by the project does **not require an API key**, so no API credential is stored in the client-side source code.

---

## Asynchronous Programming

The project uses the browser's **Fetch API** with JavaScript Promises to perform asynchronous HTTP requests.

```javascript
fetch(url)
    .then(response => {
        if (!response.ok) {
            throw new Error(`HTTP ${response.status}`);
        }
        return response.json();
    })
    .then(result => {
        // Process exchange-rate data
    })
    .catch(error => {
        // Handle request/runtime errors
    });
```

The implementation:

- Performs a non-blocking HTTP request.
- Converts the HTTP response into a JavaScript object using `response.json()`.
- Validates the API response before using the data.
- Handles failures through `.catch()`.

---

## Data Validation & Error Handling

The application validates both the HTTP response and the API payload.

### HTTP Response Validation

```javascript
if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
}
```

### API Response Validation

```javascript
if (result.result !== "success" || !result.rates) {
    throw new Error(result["error-type"] || "Exchange rate unavailable");
}
```

### Exchange Rate Validation

```javascript
const exchangeRate = result.rates[toCurrency.value];

if (typeof exchangeRate !== "number") {
    throw new Error("Selected currency is unavailable");
}
```

This prevents the application from attempting calculations with invalid or missing API data.

---

## Conversion Logic

The application applies the standard currency conversion formula:

```text
Converted Amount = Input Amount × Exchange Rate
```

Implementation:

```javascript
const totalExRate =
    (Number(amountVal) * exchangeRate).toFixed(2);
```

The result is converted to a numeric value and formatted to **two decimal places** before being rendered in the interface.

---

## Dynamic DOM Manipulation

The application generates currency `<option>` elements dynamically rather than hard-coding every option into the HTML.

```javascript
let optionTag =
    `<option value="${currency_code}" ${selected}>${currency_code}</option>`;

dropList[i].insertAdjacentHTML("beforeend", optionTag);
```

This demonstrates:

- DOM querying
- Dynamic HTML generation
- Template literals
- `insertAdjacentHTML()`
- Event listener registration
- Runtime UI updates

---

## Currency-to-Country Mapping

`country-list.js` maintains a JavaScript object that maps currency codes to country codes.

Example:

```javascript
let country_list = {
    "USD": "US",
    "INR": "IN",
    "EUR": "FR",
    "GBP": "GB"
};
```

The mapping is used to construct the FlagCDN URL dynamically:

```javascript
imgTag.src =
    `https://flagcdn.com/48x36/${country_list[code].toLowerCase()}.png`;
```

This keeps currency metadata separate from the main conversion logic.

---

## Currency Swap Functionality

The exchange icon swaps the selected source and target currencies.

Implementation uses a temporary variable:

```javascript
let tempCode = fromCurrency.value;

fromCurrency.value = toCurrency.value;
toCurrency.value = tempCode;
```

After swapping, the application:

1. Updates the source flag.
2. Updates the target flag.
3. Requests the new exchange rate.
4. Updates the conversion result.

---

## Input Handling

The application handles empty and zero-value input:

```javascript
if (amountVal == "" || amountVal == "0") {
    amount.value = "1";
    amountVal = 1;
}
```

This provides a default conversion amount and prevents invalid zero/empty input from being used in the calculation.

---

## Responsive Front-End Design

The UI is implemented using CSS with:

- Flexbox-based layouts
- Responsive media queries
- Relative sizing
- Mobile-specific layout adjustments
- Custom form controls
- Hover states and transitions
- Responsive currency selection containers

The project includes a mobile breakpoint:

```css
@media (max-width: 700px) {
    .wrapper {
        width: 370px;
    }
}
```

This allows the application layout to adapt to smaller screens.

---

## Project Architecture

```text
currency-converter/
│
├── index.html
├── style.css
├── README.md
│
└── js/
    ├── country-list.js
    └── script.js
```

### File Responsibilities

| File | Responsibility |
|---|---|
| `index.html` | Defines the semantic UI structure, form controls, currency selectors, result area, and external resources |
| `style.css` | Implements responsive layout, visual styling, form states, transitions, and mobile responsiveness |
| `js/country-list.js` | Provides currency-code to country-code mapping used for flag resolution |
| `js/script.js` | Contains DOM initialization, event handling, currency selection, flag updates, API integration, conversion logic, validation, error handling, and swap functionality |
| `README.md` | Technical documentation, architecture, setup, and implementation details |

---

## Technologies & External Services

### Front-End Technologies

- **HTML5**
- **CSS**
- **JavaScript**

### Browser APIs / JavaScript Features

- Fetch API
- Promises
- DOM API
- Event Listeners
- Template Literals
- Object-based data mapping
- `Number()` conversion
- `toFixed()` formatting

### External Services

- **ExchangeRate-API Open Endpoint** — exchange-rate data
- **FlagCDN** — currency flag assets
- **Font Awesome 5.15.3 CDN** — exchange icon
- **Google Fonts** — Poppins typography

---

## Application Workflow

```text
Application Load
      |
      v
Populate Currency Dropdowns
      |
      v
Set Default Currencies
      |
      v
Register DOM Event Listeners
      |
      v
Request Exchange Rate
      |
      v
Validate HTTP/API Response
      |
      v
Extract Target Currency Rate
      |
      v
Calculate Conversion
      |
      v
Update Exchange-Rate Display
```

---

## Local Setup

### Prerequisites

- Modern web browser
- VS Code or another code editor
- Internet connection
- Optional: Live Server, Python, or Node.js static server

### Run with VS Code Live Server

1. Open the project folder in VS Code.
2. Install the **Live Server** extension.
3. Right-click `index.html`.
4. Select **Open with Live Server**.
5. Open the URL displayed by Live Server.

### Run with Python

From the project root:

```bash
python -m http.server 5500
```

Then open:

```text
http://localhost:5500
```

### Run with Node.js

```bash
npx serve .
```

Open the URL displayed in the terminal.

> Running through an HTTP server is recommended instead of opening the HTML file directly with `file:///...`, particularly when external resources or API requests are involved.

---

## Performance & Code Quality

The application follows a lightweight client-side architecture with:

- Minimal dependencies
- No backend server
- Asynchronous API requests
- Dynamic DOM rendering
- Reusable JavaScript functions
- Centralized currency mapping
- Responsive CSS
- Runtime validation
- Explicit error handling

The separation of HTML, CSS, and JavaScript keeps the presentation layer and application logic organized.

---

## Security Considerations

- The current API endpoint does not require an API key.
- No API credentials are embedded in the source code.
- The application validates API responses before consuming exchange-rate data.
- External resources are loaded through HTTPS URLs.
- No sensitive user information is stored by the application.

---

## Troubleshooting

### Exchange rate is not loading

Check:

1. Internet connectivity.
2. Browser Developer Tools → **Console**.
3. Whether the application is running through an HTTP server.
4. Whether the external API is reachable.
5. Whether the selected currency is available in the API response.

### Currency flags are not displayed

The application loads flag assets from FlagCDN. Check the browser console and network connection.

### `python` is not recognized

Install Python and add it to the system `PATH`, then restart the terminal.

### `npx` is not recognized

Install Node.js and reopen VS Code or the terminal.

---

## Future Enhancements

Potential production-oriented improvements include:

- API request caching
- Loading-state indicators
- Conversion history
- Favorite currencies
- Accessibility enhancements
- Automated unit testing
- API retry strategy
- Debouncing frequent input events
- Progressive Web App (PWA) support
- Backend API proxy for controlled production deployments

---

## Key Learning Outcomes

This project demonstrates hands-on experience with:

- Client-side web application development
- REST-style API consumption
- Asynchronous programming
- Fetch API
- Promise-based workflows
- JSON response processing
- DOM manipulation
- Event-driven architecture
- Input validation
- Runtime error handling
- Responsive UI development
- External CDN integration
- Modular separation of data and application logic

---

## Project Status

**Status:** Completed

**Application Type:** Responsive Client-Side Web Application

**Architecture:** Frontend-only API-integrated application

**Primary Focus:** Currency conversion, API integration, JavaScript event handling, DOM manipulation, validation, error handling, and responsive UI.

---

## Author

**Talapaneni Balaji**

B.Tech — Artificial Intelligence and Data Science

**Technical Stack:** HTML5 | CSS | JavaScript | Fetch API | Git | GitHub

---

## License

This project is intended for educational, portfolio, and demonstration purposes.
