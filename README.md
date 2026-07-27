# carbonsheet
This is a repositories containing CarbonSheet registeries for all cheatsheets. The web app will make use of this repo as the database for all the cheatsheets. You can also host your own cheatsheets in your own public GitHub repository. Check below for more information. 

![alt text](image-1.png)
![alt text](image.png)

## Cheatsheet Schemas
Check the SCHEMA.md to learn how to create it. 

## How to host Your Own Cheatsheets: 
1. Clone this repository
2. Create or edit a cheat sheet using the [web editor](https://carbonsheets.vercel.app/editor) <br>
3. or Generate a cheat sheet with AI (optional).
Ask your AI assistant to read the SCHEMA.md file in the repository and generate a cheat sheet that follows the schema. Output in JSON

4. Publish your cheat sheet.
Add your new cheat sheet entry to carbonsheet-registry.json, then commit and push the changes to your own public GitHub repository.

5. Import your cheat sheets on the web app
Open the [registry page](https://carbonsheets.vercel.app/registry): <br>Add your GitHub repository URL, then import the cheat sheets you'd like to use

Schema Documentation: https://carbonsheets.vercel.app/schema <br>
Schema Editor: https://carbonsheets.vercel.app/editor <br>
Web App: https://carbonsheets.vercel.app

## Keyboard Shortcuts Reference
Use the shortcuts to navigate faster in cheatsheet: 

| Category | Shortcut (macOS) | Shortcut (Windows) | Action / Command |
| :--- | :--- | :--- | :--- |
| **Search & Commands** | `F` / `⌘F` | `Ctrl + F` | In-Sheet Search |
| | `⌘K` | `Ctrl + K` | Global Command Palette |
| **Navigation & UI** | `T` | `T` | Toggle Sidebar |
| | `?` | `?` | Shortcuts Guide |
| **View Modes** | `D` | `D` | Detailed View |
| | `C` | `C` | Compact View |
| | `S` | `S` | Sheet View |
| | `P` | `P` | Poster View |
| | `M` | `M` | Cycle Views |
| **Grid Layouts** | `1` | `1` | 1-Column Layout |
| | `2` | `2` | 2-Column Layout |
| | `3` | `3` | 3-Column Layout |
| | `G` | `G` | Cycle Columns |
| **Focus & Controls** | `Z` | `Z` | Toggle Zen Mode |
| | `Esc` | `Esc` | Exit Zen / Close Modals |
