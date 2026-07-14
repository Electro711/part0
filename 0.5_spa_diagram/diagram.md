# Single-Page Application (SPA) Loading Sequence Diagram

This diagram illustrates the sequence of HTTP requests and browser actions that occur when a user navigates to the single-page application (SPA) version of the notes app at [https://studies.cs.helsinki.fi/exampleapp/spa](https://studies.cs.helsinki.fi/exampleapp/spa).

```mermaid
sequenceDiagram
    participant browser as Browser
    participant server as Server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/spa
    activate server
    server-->>browser: HTML document (spa)
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate server
    server-->>browser: CSS stylesheet (main.css)
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/spa.js
    activate server
    server-->>browser: JavaScript file (spa.js)
    deactivate server

    Note right of browser: The browser starts executing the JS code,<br/>which triggers an asynchronous GET request for data.json.

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: JSON raw data (e.g., [{"content": "...", "date": "..."}])
    deactivate server

    Note right of browser: The browser's callback function parses the JSON data<br/>and dynamically renders the notes into the DOM.
```
