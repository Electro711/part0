# Full Stack Open - Note Creation Flow Diagram

This diagram illustrates the sequence of events when a user creates a new note on the example application at `https://studies.cs.helsinki.fi/exampleapp/notes`.

```mermaid
sequenceDiagram
    autonumber
    actor User as User (Student)
    participant Browser as Browser (Client)
    participant Server as Server (Web Server)

    %% 1. User Action
    User->>Browser: Types note text and clicks the "Save" button
    activate Browser
    Note over Browser: Browser captures form data<br/>Payload: "note=Learn+Web+Development"

    %% 2. Sending the Note Data
    Browser->>Server: HTTP POST https://studies.cs.helsinki.fi/exampleapp/new_note
    deactivate Browser
    activate Server
    Note over Server: Server parses request body,<br/>adds the new note to the array,<br/>and generates a redirect response.
    Server-->>Browser: HTTP 302 Found (Redirect to /exampleapp/notes)
    deactivate Server

    %% 3. Handling the Redirect
    activate Browser
    Note over Browser: Browser receives 302 status code,<br/>reads 'Location: /exampleapp/notes',<br/>and triggers a page reload.

    %% 4. Fetching the HTML
    Browser->>Server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/notes
    activate Server
    Server-->>Browser: HTTP 200 OK (HTML Document)
    deactivate Server
    Note over Browser: Browser parses HTML and discovers links<br/>to CSS stylesheet and JavaScript code.

    %% 5. Fetching CSS
    Browser->>Server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate Server
    Server-->>Browser: HTTP 200 OK (CSS Stylesheet)
    deactivate Server
    Note over Browser: Browser applies styles to render the layout.

    %% 6. Fetching JavaScript
    Browser->>Server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/main.js
    activate Server
    Server-->>Browser: HTTP 200 OK (JavaScript code file)
    deactivate Server
    Note over Browser: Browser starts executing main.js,<br/>which triggers an asynchronous data fetch.

    %% 7. Fetching Note Data (JSON)
    Browser->>Server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate Server
    Server-->>Browser: HTTP 200 OK (JSON array with all notes, including the new one)
    deactivate Server
    Note over Browser: Browser executes JS callback function:<br/>1. Parses JSON data<br/>2. Dynamically generates list elements (DOM)<br/>3. Renders list to the screen.

    %% 8. Completion
    Browser-->>User: Web page updates: New note appears on the list!
    deactivate Browser
```

## Summary of the Lifecycle

1. **HTTP POST Request:** The user types their note and clicks submit. The browser sends the data to `/new_note` via `POST`.
2. **HTTP 302 Redirect:** The server receives the data, updates its data store, and responds telling the browser to redirect its address to `/notes`.
3. **HTTP GET (HTML):** The browser loads `/notes` via a standard `GET` request.
4. **Subsequent Asset Loading:** The browser parses the HTML and initiates `GET` requests for the stylesheet (`main.css`) and script (`main.js`).
5. **Dynamic Data Loading:** The JavaScript code executes and fetches `/data.json` via a `GET` request.
6. **Rendering:** Once the data arrives, the browser dynamically renders the updated list of notes to the DOM.
