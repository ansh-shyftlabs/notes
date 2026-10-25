# Notes example

A small Electron app with one saved note. Click **Export notes** and the app writes `notes.csv` to your Downloads folder.

The note is stored in code, not in a file you edit from the window:

```js
{ id: 1, title: "First note" }
```

The CSV looks like this:

```csv
id,title
1,"First note"
```

The `title` column is quoted. A title that contains a quote, such as `Hello, "REA"`, is written as `"Hello, ""REA"""`.

## Start the app

From this folder:

```powershell
npx electron .
```

The first time you set the project up on a new machine, install Electron before that command:

```powershell
npm install
npx electron .
```

A window titled **Notes** opens. Click **Export notes**. The status line shows the full path of `notes.csv`, usually `C:\Users\<you>\Downloads\notes.csv`. Exporting again replaces that file.

## What happens on click

1. `index.html` shows the **Export notes** button.
2. `renderer.js` calls `window.notes.exportCsv()` when the button is clicked.
3. `preload.js` sends that call to the main process on the `notes:export` channel.
4. `main.js` builds the path with `app.getPath("downloads")` and `notes.csv`, turns the note into CSV with `csv.js`, and writes the file.
5. The main process returns the path, and `renderer.js` displays it in the status line.

## File structure

| File | Role |
| --- | --- |
| `package.json` | Project metadata. `main` is `main.js`. Electron is the dependency. |
| `main.js` | Starts the window, holds the note, and writes `notes.csv`. |
| `preload.js` | Exposes `notes.exportCsv()` to the page. |
| `index.html` | The window contents: heading, export button, and status line. |
| `renderer.js` | Click handler for the export button. |
| `csv.js` | Builds the `id,title` CSV text. |
| `package-lock.json` | Locks the Electron version installed by `npm install`. |
