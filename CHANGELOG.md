# Change Log

## [0.9.100] - 2025-11-07
### Added
- New built-in inputs `_[Select a folder in project]_` and `_[Select a file in project]_`, which behave the same as `_[Select a folder]_` and `_[Select a file]_` but open the explorer dialog **restricted to the current workspace root**.
- New terminal-to-VSCode pattern `TERMINAL_GUI_COMMAND(command)` — allows one command’s output to **trigger another Terminal GUI command** defined in config.
- Added `hidden` option for `settings.showWhenEmptyWorkspace`, which hides commands from all menus and buttons but still allows them to be **executed internally via** `TERMINAL_GUI_COMMAND(command)`.

## [0.9.97] - 2025-10-29
### Fixed
- Dual-state (`icon2`) commands now automatically reset their `icon` state when the associated terminal is **manually closed or killed**, preventing them from staying stuck in the "active" state.

## [0.9.8] - 2025-08-03
### Added
- Support for two‐state “toggler” inputs via the new built-in variable `_[toggle]_` when `connectItems` is defined: renders checkbox items default-checked with 🟩/🟥 prefixes and concatenates the chosen command fragments.  

## [0.9.3] - 2025-07-22
### Fixed
- IntelliSense performance issue when editing placeholders.

## [0.9.2] - 2025-07-20
### Added
- Custom‐label support for folder placeholders: `_[Select a folder(Your text)]_` now shows “Your text” in the prompt.
- New built‑in variable `Select a file` (placeholder `_[Select a file]_`) for picking file paths.

## [0.9.1] - 2025-07-13
### Added
- IntelliSense/autocomplete for custom placeholders (`_[varName]_`) derived from each command’s `inputs` keys, with built-in variables listed afterward.
- Yellow-squiggly diagnostics (tooltip: “Unknown placeholder”) for any `_[…]_` token not defined in that command’s `inputs` or in built-ins.

### Changed
- Removed the `hidden` option from the `showWhenEmptyWorkspace` property; commands now only support `fullWorkspace`, `emptyWorkspace`, or `both`.

## [0.9.0] - 2025-07-08
### Changed
- Renamed persist flag for free-text inputs from `;radio` to `;save` (in `parseInputKey`).

## [0.8.6] - 2025-07-04
### Changed
- Replaced the old `settings.statusBar` / `settings.editorMenu` booleans with one `settings.quickButton` enum (`"statusBar"`, `"tabBar"`, `"both"`).  
- Updated JSON schema (`schema/terminalgui-schema.json`) and `VSCode` configuration (`package.json`) to reflect `quickButton`.

### Added
- `basic.pinTabBar` setting (default `true`) to keep a placeholder “Terminal GUI” tab open so the editor title bar never collapses.

## [0.8.1] - 2025-06-29
- Apply full JSON schema to all configuration sources (`settings.json`, `terminalgui.config.json`/`.jsonc`): now `basic`, `commands`, `scripts` and `VSCodeSnippets` definitions (including the required `command` property) are enforced everywhere.

## [0.8.0] - 2025-06-28
### Added
- **VSCodeSnippets** support: place a `VSCodeSnippets` object next to `commands` in `TerminalGui.config` to register normal `VSCode` style snippets (scope / prefix / body / description) entirely in-memory.
- Snippets appear in IntelliSense with full tab-stop & placeholder behaviour for the languages listed in `scope`.

## [0.7.8] - 2025-06-26
### Added
- IntelliSense/autosuggestions for built-in variables (`_[projectPath]_`, `_[itemPath]_`, etc.) when editing commands in:
  - `settings.json` (both workspace and user settings)
  - `terminalgui.config.json`
  - `terminalgui.config.jsonc`
- Descriptions for each variable appear in the completion list.

## [0.7.7] - 2025-06-24
### Added
- **Checkbox inputs**: define an input object with a `"connectItems"` key to show a multi-select (checkbox) list.  
  Selected option-values are concatenated using the provided joiner (e.g. `"&&"`).
- `allowEmpty` (3-rd key part) lets the user skip selection when set to `true`.
- `save` (4th key part) persists checked items in `checkboxCommands`, so they're pre-selected on next run.

## [0.7.3] - 2025-05-29
### Removed
- **Breaking Change**: The `Command Chaining` feature has been completely removed.

## [0.7.2] - 2025-05-28
### Fixed
- Terminal freeze issue.

## [0.7.1] - 2025-05-26
### Fixed
- Temporary snippet files now use index-based naming instead of command names.

## [0.7.0] - 2025-05-26
### Removed
- **Breaking Change**: Removed the sidebar launcher completely. Commands are now accessible only through Status bar buttons and Context menu.

## [0.6.0] - 2025-05-25

### Added
- Persistable free-text inputs via `;radio` flag: prompts now include "Save" and "Save & Skip" checkboxes to remember values across runs and auto-skip when chosen.

- New radioCommands section in `.vscode/terminal-gui.temp/terminalgui.temp.json` to store persistent entries and their skip mode.

## [0.5.9] - 2025-05-24

### Added
- Support for JSONC external config files: you can now use `terminalgui.config.jsonc` (JSON with comments & trailing commas).

## [0.5.8] - 2025-05-23

### Changed
- Breaking Change: The `configFile` property has been moved from `TerminalGui.config.basic.configFile` to `TerminalGui.configFile`. Update your `settings.json` accordingly.

## [0.5.7] - 2025-05-22

### Changed
- Default value for `revealConsole` setting changed from `true` to `false`. Commands will now keep the terminal hidden by default unless explicitly configured otherwise.
- Long-running commands (those with `icon2`) now **auto-reveal the terminal only the first time they start**; subsequent runs respect the `revealConsole` value.
- New `basic.commandsMenuOnStatusBar` and `basic.commandsMenuRefreshOnStatusBar` properties to show commands menu button and refresh button in the status-bar.

## [0.5.6] - 2025-04-08

### Changed
- Commands with `"revealConsole": false` now keep the terminal hidden on successful execution and automatically reveal it when a non-zero exit code is detected.

### Added
- Support for passing info from the terminal output to VSCode via the `TERMINAL_GUI_MSG(...)` command.

## [0.5.5] - 2025-03-28

### Added
- Context menu now available on open file pages (right‑click in the editor shows the `Terminal GUI` menu).
- New built-in variable `_[selectedText]_` to return the currently selected text.
- New built-in variable `_[clickedWord]_` to return the word under the cursor when right‑clicked.
