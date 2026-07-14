# SPA New Note Creation Sequence Diagram

This diagram depicts the sequence of events when a user creates a new note using the single-page version of the notes app at [https://studies.cs.helsinki.fi/exampleapp/spa](https://studies.cs.helsinki.fi/exampleapp/spa).

```mermaid
sequenceDiagram
    participant browser as Browser
    participant server as Server

    Note over browser: The user writes a note in the text field and clicks 'Save'

    Note right of browser: JavaScript intercepts the submit event (e.preventDefault()),<br/>adds the new note to the local array, clears the input field,<br/>and redraws the notes list locally in the DOM immediately.

    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note_spa
    activate server
    Note over browser,server: Headers: Content-type: application/json<br/>Body: {"content": "your note content", "date": "2026-07-14T13:50:00.000Z"}
    server-->>browser: HTTP 201 Created (JSON: {"message": "note created"})
    deactivate server

    Note right of browser: The callback function executes and logs the server response.
```
