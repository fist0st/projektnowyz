sequenceDiagram
    participant browser
    participant server

    %% A. Initial load of the single-page app
    Note right of browser: User opens https://studies.cs.helsinki.fi/exampleapp/spa
    browser->>server: GET /exampleapp/spa
    activate server
    server-->>browser: HTML document (SPA shell)
    deactivate server

    browser->>server: GET /exampleapp/main.css
    activate server
    server-->>browser: CSS file
    deactivate server

    browser->>server: GET /exampleapp/main.js
    activate server
    server-->>browser: JavaScript (SPA bundle)
    deactivate server

    Note right of browser: Browser executes SPA JavaScript (bootstrapping)
    Note right of browser: JS router / app initializes and requests initial data

    browser->>server: GET /exampleapp/data.json
    activate server
    server-->>browser: notes JSON (initial list)
    deactivate server

    Note right of browser: SPA renders notes into the DOM without full page reload

    %% B. User creates a new note inside the SPA
    Note right of browser: User types a note and clicks "Save" (SPA form)
    browser->>server: POST /exampleapp/notes   : { content: "New note", date: "2023-..." }
    activate server
    Note right of server: Server saves note and returns created resource
    server-->>browser: HTTP 201 Created + created note JSON
    deactivate server

    Note right of browser: SPA receives created note and updates UI (append note)
    Note right of browser: No full page reload — DOM updated by JS

    %% optional: SPA may also re-fetch the list instead of using returned note
    Note right of browser: (optional) SPA may GET /exampleapp/data.json again to sync
