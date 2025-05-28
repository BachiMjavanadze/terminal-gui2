# Change Log

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
