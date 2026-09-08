# System Specification

## 1. Character Encoding

### 1.1 Source files
- UTF-8 is the standard for new and current areas.
- Some files in the legacy area remain encoded as Shift_JIS.
- Before changing the encoding of a legacy file, confirm the impact on callers and any conversion logic.

### 1.2 HTTP responses
- Browser-facing HTML responses are returned as UTF-8.
- The application area overrides the higher-level default and explicitly uses UTF-8.
- The production environment is accessed through a front-end proxy. Do not identify the application server solely from the Server header observed by the client.

## 2. File Download

### 2.1 Confirmation before download
- Show a confirmation dialog before downloading a target file.
- "Continue" starts the download.
- "Cancel" closes the dialog without downloading.
- Do not send the download request when the dialog is first displayed.

## 3. Account Data Export

### 3.1 Confirmation before export
- Account data export goes through a confirmation screen.

### 3.2 Export target
- The current search conditions are carried into the export request.
- The server determines the final export target.
- The number of rows displayed in the UI may differ from the number of exported rows.

## 4. Pre-release verification
- Treat implementation completion and real-device verification as separate states.
- Items that depend on external specification confirmation are implemented only after the specification is confirmed.
