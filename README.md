## `Terminal GUI`

**`Terminal GUI`** is a `VSCode` extension that lets you define custom terminal commands with interactive inputs. You can configure your commands in your `settings.json` (or in an external config file. See below how property `configFile` works), and when you run a command, any placeholders you've defined will prompt you for input.

   - Note: **`Terminal GUI`** supports `Git Bash` and `PowerShell` scripting languages, so you can write very complex commands.

   - Note: all examples below are for the `Git Bash` terminal. To set the `Git Bash` as your default terminal use this configuration in `settings.json` of the `VSCode`:

```json
"terminal.integrated.defaultProfile.windows": "Git Bash",
```

Below are some simple command examples to quickly evaluate the extension (at the end of the document you may find more practical commands for different frameworks).

   - Note: when you create a new command or update an existing one, changes do not take effect immediately; You additionally need to click on the `Refresh` button or use the keybinding `ctrl+alt+p` (for Mac: `cmd+alt+p`):

in the status bar:

   ![Image](https://raw.githubusercontent.com/BachiMjavanadze/terminal-gui2/refs/heads/main/src/media/refresh.avif)

<details>
<summary>📂 Simple Examples</summary>

```json
// settings.json
// "TerminalGui.configFile": ".vscode/terminalgui.config.json",
"TerminalGui.config": {
  "basic": {
    "autoSaveToggler": true,
    "recentlyUsedCommands": 7,
    "commandsMenuOnStatusBar": true,
    "commandsMenuRefreshOnStatusBar": true
  },
  "commands": {
    // Prints a basic message. Always visible, regardless of workspace state
    "Echo Basic Message": {
      "command": "echo \"Basic Message\"",
      "settings": {
        "showWhenEmptyWorkspace": "both",
        "revealConsole": true
      }
    },
    // Displays a message only when a workspace is open
    "Print Workspace Message": {
      "command": "echo \"Workspace Message\"",
      "group": "",
      "settings": {
        "showWhenEmptyWorkspace": "fullWorkspace",
        "revealConsole": true
      }
    },
    // Shows a message when no folder is open
    "Echo Empty Workspace Message": {
      "command": "echo \"Empty Workspace Message\"",
      "group": "good",
      "settings": {
        "showWhenEmptyWorkspace": "emptyWorkspace",
        "revealConsole": true
      }
    },
    // Opens a terminal labeled "Server" to run a server-related command
    "Launch Server Terminal": {
      "command": "ng serve",
      "icon": "▶;run server",
      "icon2": "▢;stop server",
      "group": "🅰️ Angular",
      "settings": {
        "terminalName": "Angular Server",
        "quickButton": "statusBar",
        "revealConsole": true
      }
    },
    // Uses an interactive prompt to select a folder
    "Print a Selected Folder Path": {
      "command": "echo _[Select a folder]_",
      "settings": {
        "revealConsole": true
      },
    },
    // Prompts for a color (choice between Red or Blue) and custom text, then prints them.
    "Print Color and Custom Text": {
      "command": "echo _[var1]_ && echo _[var2]_",
      "inputs": {
        "choose color; var1": {
          "Red": "red color",
          "Blue": "blue color"
        },
        "enter some text; var2": ""
      },
      "settings": {
        "reUseTerminal": true,
        "revealConsole": true,
        "contextMenu": false,
      }
    },
    // Demonstrates a multi-step command with several inputs
    "Execute Interactive Command Sequence": {
      "command": "echo _[var1]_ && _[var2]_ && _[var2]_ && echo _[var3]_ && echo _[var3]_ && echo _[Select a folder]_ && echo _[var4]_ && echo _[var4]_",
      "inputs": {
        "Enter text; var1": "",
        "your command; var2": {
          "Print hi": "echo \"hi\"",
          "Say bye": "echo \"bye\""
        },
        "choose item; var3": {
          "Hi": "Hello there!",
          "Bye": "Goodbye boys!"
        },
        "Put another text; var4": ""
      },
      "settings": {
        "reUseTerminal": true,
        "revealConsole": true,
        "contextMenu": false,
        "showWhenEmptyWorkspace": "both",
      }
    },
    // Displays various file and folder properties from the current context
    "File Context Info": {
      "command": "echo itemPath: _[itemPath]_ && echo parentPath: _[parentPath]_ && echo folderPath: _[folderPath]_ && echo itemFullName: _[itemFullName]_ && echo itemName: _[itemName]_ && echo itemExtension: _[itemExtension]_ && echo projectPath: _[projectPath]_",
      "settings": {
        "contextMenu": true,
        "revealConsole": true,
      }
    },
    // Demonstrates snippets syntax for writing data in a file
    "snippet command": {
      "command": "cd _[projectPath]_ && echo -e _[my snippet]_ > temp.txt",
      "snippets": {
        "my snippet": [
          "hello",
          "world"
        ]
      },
      "settings": {
        "revealConsole": true,
      }
    },
    "snippet with temp file": {
      "command": "cd _[folderPath]_ && cp _[my snippet]_.file temp.txt",
      "snippets": {
        "my snippet": [
          "hello",
          "world"
        ]
      },
      "settings": {
        "contextMenu": true
      }
    },
    "foo": {
      "command": "echo _[var1]_",
      "inputs": {
        "enter some text; var1; true; true; save": ""
      }
    },
  }
},
```
</details>

### Run Commands

   - There are two types of commands: regular commands and context menu commands.
   
   - If `"contextMenu": true` is set, the command is a context menu command, meaning it can be launched by right-clicking on a file or a folder in the `VSCode` explorer (or by combination of keybinding: `ctrl+shift+e`, navigate with up/down keyboard arrow buttons through files/folders, `shift+F10`, choose `Terminal GUI` menu item).

   - Regular commands can be launched via the button in the status bar (default keybinding: `ctrl+alt+l`. For Mac: `cmd+alt+l`).

![Image](https://raw.githubusercontent.com/BachiMjavanadze/terminal-gui2/refs/heads/main/src/media/regular%20commands.avif)

   - When a command contains placeholders, you'll be prompted to enter the required values.

   - You can navigate through inputs with left/right arrows located on the title bar of the command or use keybindings:

        - **Navigate Right:** `ctrl+alt+.` (for Mac: `cmd+alt+.`)
        - **Navigate Left:** `ctrl+alt+,`  (for Mac: `cmd+alt+,`)
   
   - The `Enter` button allows you to move forward and execute commands.

![Image](https://github.com/user-attachments/assets/80d5013c-8573-49ab-83c7-6ee3aca8e826)

### Terminal Freeze Prevention

   - To prevent `VSCode` terminals from freezing, `Terminal GUI` automatically sends special commands:
     - When a new terminal is created: `echo 'Terminal GUI started'`
     - Before each command execution: `echo 'HEALTH_CHECK_[number]'`
   
   - These commands help ensure the terminal remains responsive, especially when running invalid commands.

### Define Commands with interactive Inputs

   - In your configuration, create commands with placeholders wrapped in `_[ ... ]_` (for example, `_[Enter text]_`). At runtime, `Terminal GUI` will replace these placeholders with values you provide.

   - You can specify inputs for your commands – either as `free text` or as a `choice list` – to dynamically build your command string.

   - For command you may use optional `icon` property with emoji or HTML entity and optional `group` property to organize similar commands into one group.

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"🚀 Print Color and Custom Text": {
  "command": "echo _[choose color]_ && echo _[enter some text]_",
  "group": "🅰️ Angular",
  "inputs": {
    // choice list input
    "choose color": {
      "Red": "red color",
      "Blue": "blue color"
    },
    // free text input
    "enter some text": ""
  },
},
```

In the above example, if the user selects "Red" from "choose color" and enters "hello" in "enter some text", then the output in the `Git Bash` terminal will be:

```bash
$ echo red color && echo hello
red color
hello
```

### Custom Input Key Syntax

   You can differentiate between the input field prompt (what the user sees) and the command substitution placeholder (what gets replaced in your command).

   - Use a semicolon (`;`) in the input key to separate the two parts.  
     For example, `"choose color; my var"` will display **"choose color"** as the prompt while using **"my var"** as the placeholder in the command string.

   - If no semicolon is provided, the key is used for both the prompt and command substitution.

   - The free text input key supports an extended syntax:
     ```json
     "prompt text; substitution placeholder; allowEmpty; allowSpaces": "default value"
     ```
      - **prompt text:** What is displayed as the prompt.
      - **substitution placeholder:** The token in your command string to be replaced.
      - **allowEmpty:** Set to `"true"` if empty input is allowed (default is `"false"`).
      - **allowSpaces:** Set to `"true"` if spaces between words are allowed (default is `"true"`).

   - The choice list input key supports an extended syntax:
     ```
     "prompt text; substitution placeholder; allowCustomValue; allowSpaces"
     ```
      - **prompt text:** The text displayed as the prompt for the user.
      - **substitution placeholder:** The token in the command that will be replaced with the chosen (or entered) value.
      - **allowCustomValue:** A flag (`"true"` or `"false"`) that, when set to `"true"`, allows the user to enter a custom value that isn’t in the predefined choice list. If `"false"` (or omitted), the user must select one of the predefined options.
      - **allowSpaces:** Set to `"true"` if spaces between words are allowed (default is `"true"`). If set to `"false"`, the user cannot enter spaces.

For example:

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"🚀 Print Color and Custom Text": {
  "command": "echo _[var1]_ && echo _[var2]_",
  "inputs": {
    // Here the third parameter "true" enables free text input and the fourth parameter "false" disallows spaces.
    "choose color; var1; true; false": {
      "Red": "red color",
      "Blue": "blue color"
    },
    // Here the third parameter "false" means empty input is not allowed and the fourth parameter "false" disallows spaces.
    "enter some text; var2; false; false": "my value"
  }
}
```

#### Checkbox list input
You can present a multi-select (checkbox-style) list.  
Declare the choice object with a special key **`"connectItems"`** whose value is the string used to join the chosen command snippets.

- The checkbox list input key supports an extended syntax:
  ```text
  "prompt text; substitution placeholder; allowEmpty; save"
  ```
  - **prompt text:** The text displayed as the prompt for the user.
  - **substitution placeholder:** The token in the command that will be replaced with the chosen (or entered) values.
  - **allowEmpty:** Set to `"true"` if empty input is allowed (default is `"false"`).
  - **save:** Set to `"true"` to remember and pre-select previously chosen items on next run (default is `"false"`).

  When `save` is `true`, selected items are saved in `.vscode/terminal-gui.temp/terminalgui.temp.json` under the `checkboxCommands` section, and will be pre-checked on subsequent invocations.

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"temp": {
  "command": "echo _[var1]_ && _[var2]_ && echo _[var3]_",
  "inputs": {
    "enter txt1; var1": "",
    "choose some values; var2; false; true": { // allowEmpty = false, save = true
      "connectItems": "&&", // REQUIRED → identifies a checkbox list
      "Red": "echo 'red color'",
      "Blue": "echo 'blue color'",
      "Green": "echo 'green color'"
    },
    "enter txt2; var3": ""
  }
},
```

   - If the same input is used multiple times in the `command` property, the user will only see one input. eg.:

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"temp": {
  "command": "echo _[var1]_ && echo _[var1]_ && echo _[var1]_",
  "inputs": {
    "enter some text; var1": ""
  }
},
```

In the example above, the user will only see one input. If the user enters, say, "hello", the output in the terminal will be "hello" 3 times:

```bash
$ echo hello && echo hello && echo hello
hello
hello
hello
```

   - Sometimes you don't need the input to return a value immediately. In that case, use the `_[...]_.value` syntax.

For example:

```json
"temp": {
  "command": "_[var1]_.value echo \"Hello _[var1]_\"",
  "inputs": {
    "enter some text; var1": ""
  }
},
```

   - Note: In the above example, we don't use the `echo` at the start of the command or `&&` operator after `_[var1]_.value` because it doesn't return anything at that point, that part of the command will be removed when the command is executed. If the user enters, say, "John Smith", the output in the terminal will be:

```bash
$ echo "Hello John Smith"
Hello John Smith
```

* If you use `;save`, on each run you'll see a QuickPick with two checkboxes:

  * **Save** – remember the value but still prompt next time.
  * **Save & Skip** – remember the value and auto-apply on future runs.
* Persistent entries are stored in `.vscode/terminal-gui.temp/terminalgui.temp.json`. If `skip:true`, the prompt is bypassed.

For example:

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"foo": {
  "command": "echo _[var1]_",
  "inputs": {
    "enter some text; var1; true; true; save": ""
  }
},
```

Example for `ASP.NET WEB API` migrations:

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"Add Migration": {
  "command": "dotnet tool update --global dotnet-ef && dotnet ef migrations add \"_[migrationName]_\" --project \"_[dbContextPath]_\" --context _[dbContextClassName]_ --startup-project \"_[startProjectPath]_\" --output-dir \"_[migrationFolder]_\"",
  "group": "ASP.NET",
  "inputs": {
    "Enter a migration name; migrationName": "",
    "Enter a DbContext project path; dbContextPath; true; true; save": "",
    "Enter a DbContext class name; dbContextClassName; true; true; save": "",
    "Enter a StartUp project path; startProjectPath; true; true; save": "",
    "Enter a migration folder path; migrationFolder; true; true; save": "",
  },
  "settings": {
    "revealConsole": true
  }
},
"Update DataBase": {
  "command": "_[dbContextPath]_.value _[dbContextClassName]_.value _[startProjectPath]_.value dotnet ef database update --project \"_[dbContextPath]_\" --context _[dbContextClassName]_ --startup-project \"_[startProjectPath]_\"",
  "group": "ASP.NET",
  "inputs": {
    "Enter a DbContext project path; dbContextPath; true; true; save": "",
    "Enter a DbContext class name; dbContextClassName; true; true; save": "",
    "Enter a StartUp project path; startProjectPath; true; true; save": "",
  },
  "settings": {
    "revealConsole": true
  }
},
```

### Command settings

   You can customize a command with the `settings` property, which is an object with optional properties:

   - **`terminalName`**  
     Label to be shown as terminal name. If none, regular terminal name will be shown instead.

   - **`quickButton`**  
    Where to render this command’s icon as a quick-access button. One of:  
      - `"statusBar"` – show icon on the status bar.
      - `"tabBar"` – show icon in the editor title/tab bar (`VSCode` allows only 8 icons visible; others will be hidden under the `More Actions...` button).
      - `"both"` – show icon in both places.
        - Note: `VSCode` doesn’t support dynamic title/tab-bar updates at runtime, so `Terminal GUI` rewrites `package.json` and will prompt you to reload the window **twice** to apply changes.

`tabBar` commands:

![Image](https://raw.githubusercontent.com/BachiMjavanadze/terminal-gui2/refs/heads/main/src/media/tab-bar.webp)

`statusBar` commands:

![Image](https://raw.githubusercontent.com/BachiMjavanadze/terminal-gui2/refs/heads/main/src/media/satatus-bar.webp)

   - **`reUseTerminal`**  
     Reuse existing terminal or create new terminal for each command (default: true).

   - **`contextMenu`**  
     Determines whether the command is displayed in the context menu (default: false).

   - **`revealConsole`**  
     Whether to reveal the terminal console when running the command (default: `false`). `false` value keeps the terminal hidden on successful execution and automatically reveal it when a non-zero exit code is detected.

   - **`showWhenEmptyWorkspace`**  
     Show command only when:
        - `fullWorkspace` - a workspace is open (default).
        - `emptyWorkspace` - no folder is open.
        - `both` - always shown.

```json
"temp": {
  "command": "echo Hello",
  "settings": {
    "reUseTerminal": true,
    "revealConsole": true,
    "contextMenu": false,
    "terminalName": "Temp Command",
    "showWhenEmptyWorkspace": "fullWorkspace"
  },
},
```

### Long-Running Commands (`icon` and `icon2`)

   - If a command is long running (server watching, project creation), you may define `icon` and `icon2` poperies. You can use an emoji or an HTML entity as an icon.

       - `icon` - an icon and optional tooltip (format: `icon;tooltip`) to display when command is not running.
       - `icon2` - an icon, tooltip and optional interrupts count (format: `icon;tooltip;interrupts`) to display when command is running. The interrupt count controls how the command is stopped:

         - **Stopping Behavior:**  
         When you click a long-running command (one with `icon2` defined) a second time, `Terminal GUI` stops its execution by sending interrupt signals (`^C`) to the terminal.

         - **Custom Interrupt Count:**  
         You can specify how many `^C` signals should be sent by adding a number after the tooltip in `icon2`, separated by a semicolon (e.g. `"▢;stop server;4"`).

         - **Default Behavior:**  
        If no interrupt count is provided (for example `"▢;stop server"`), the extension will send two `^C` signals – the first immediately and the second after a 300ms delay. This is because `VSCode` sometimes "swallows" one `^C` signal, meaning that instead of sending *n* signals, only *n-1* actually reach the terminal. Sending an extra signal helps ensure that the process is properly stopped.

        **Auto-reveal once:**  
        Even when `settings.revealConsole` is `false`, the terminal panel is revealed **the first time** a long-running command starts. Subsequent launches keep the panel hidden unless `revealConsole` is `true` or the process exits with an error.

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"Launch Angular Server": {
  "command": "ng serve",
  "icon": "▶;run server",
  "icon2": "▢;stop server",
  "group": "🅰️ Angular",
  "settings": {
    "terminalName": "Angular Server",
    "statusBar": true,
  }
},
```

### `_[Select a folder]_` (Built-In Variable)

  - This built-in variable is an input field where the user can manually enter the path to a folder or press the `Enter` key to bring up the folder explorer and select the desired folder.

  ![Image](https://raw.githubusercontent.com/BachiMjavanadze/terminal-gui2/refs/heads/main/src/media/selectafolder.avif)

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
// Uses an interactive prompt to select a folder and then navigates to it
"Change Directory to Selected Folder": {
  "command": "cd _[Select a folder]_ && echo _[Select a folder]_",
},
```

   - You can include your own prompt text in parentheses. For example, `_[Select a folder(Hello World!)]_` will display **Hello World!** as the placeholder in the input box, while still behaving exactly like `_[Select a folder]_`.

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
// Uses a custom prompt message when selecting a folder
"Change Directory to Selected Folder 2": {
  "command": "cd _[Select a folder(Lorem ipsum Dolor)]_",
},
```

   - Note: By default `_[Select a folder]_` refers to the current project, but you can change this with the `defaultPath` parameter:

```json
// settings.json
"files.dialog.defaultPath": "D:/Development",
```

### `_[Select a file]_` (Built‑In Variable)

- This built‑in variable is an input field where the user can manually enter the path to a file or press the `Enter` key to bring up the file explorer and select the desired file.

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
// Uses an interactive prompt to select a file and then opens it in VS Code
"Open Selected File": {
  "command": "code _[Select a file]_",
},
```

- You can include your own prompt text in parentheses. For example, `_[Select a file(Hello World!)]_`.

### Other Built-In Variables

`Terminal GUI` supports several built-in variables for context menu commands (so `contextMenu` property should be `true`). These variables are embedded in your command strings using the format `_[variableName]_` and are automatically replaced at runtime with context-specific values based on the selected file or folder.

   - **`_[itemPath]_`**  
      The full path of the clicked item (either a file or a folder).

   - **`_[parentPath]_`**  
      The path to the parent directory of the clicked item.

   - **`_[folderPath]_`**  
      If the selected item is a folder, this is its path. If it's a file, this represents the path of the directory containing the file.

   - **`_[itemFullName]_`**  
      The complete name of the selected item, including its extension (if any).

   - **`_[itemName]_`**  
      The name of the selected item without the file extension.

   - **`_[itemExtension]_`**  
      The file extension (without the dot) of the selected item. For folders, this will be an empty string.

   - **`_[projectPath]_`**  
      The path of the workspace folder (project) in which the command was executed.

   - **`_[selectedText]_`**  
      The text currently selected in the active editor. If no text is selected, it returns empty quotes.

   - **`_[clickedWord]_`**  
      The word under the cursor at the time of the right‑click in the editor.

When a command is run, `Terminal GUI` scans for these built-in variables within your command string. The extension replaces each placeholder with its corresponding value before executing the command. This enables you to build dynamic, context-aware commands such as:

   - Changing directories to the folder containing the clicked file:

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"temp": {
  "command": "cd _[folderPath]_",
  "settings": {
    "contextMenu": true
  },
},
```
   - Echoing file-specific information:

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"temp": {
  "command": "echo \"Filename: _[itemName]_\"",
  "settings": {
    "contextMenu": true
  },
},
```

   - Example with `_[selectedText]_` variables:

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"temp": {
  "command": "echo _[selectedText]_",
  "settings": {
    "contextMenu": true
  }
},
```

   - In the above example, if user selects "hello world", then the output in terminal will be:

```bash
$ echo "hello world"
hello world
```
   - Example with `_[clickedWord]_` variables:

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"temp": {
  "command": "echo _[clickedWord]_",
  "settings": {
    "contextMenu": true
  }
},
```

   - In the above example, if the text "hello world" is present and the user right‑clicks on the word "hello" (without making a selection), the command will output:

```bash
$ echo "hello"
hello
```

   - Example with all other built in variables:

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"File Context Info": {
  "command": "echo itemPath: _[itemPath]_ && echo parentPath: _[parentPath]_ && echo folderPath: _[folderPath]_ && echo itemFullName: _[itemFullName]_ && echo itemName: _[itemName]_ && echo itemExtension: _[itemExtension]_ && echo projectPath: _[projectPath]_",
  "settings": {
    "contextMenu": true
  }
},
```

   - In the above example, if the clicked item is `F:\Development\terminal-gui\src\statusBar`, then the output in terminal will be:

```bash
$ echo itemPath: "f:\Development\terminal-gui\src\statusBar" && echo parentPath: "f:\Development\terminal-gui\src" && echo folderPath: "f:\Development\terminal-gui\src\statusBar" && echo itemFullName: "statusBar" && echo itemName: "statusBar" && echo itemExtension:  && echo projectPath: "f:\Development\terminal-gui"

itemPath: f:\Development\terminal-gui\src\statusBar
parentPath: f:\Development\terminal-gui\src
folderPath: f:\Development\terminal-gui\src\statusBar
itemFullName: statusBar
itemName: statusBar
itemExtension:
projectPath: f:\Development\terminal-gui
```

   - In the above example, if the clicked item is `F:\Development\terminal-gui\src\statusBar\StatusBarManager.ts`, then the output in terminal will be:

```bash
$ echo itemPath: "f:\Development\terminal-gui\src\statusBar\StatusBarManager.ts" && echo parentPath: "f:\Development\terminal-gui\src\statusBar" && echo folderPath: "f:\Development\terminal-gui\src\statusBar" && echo itemFullName: "StatusBarManager.ts" && echo itemName: "StatusBarManager" && echo itemExtension: ts && echo projectPath: "f:\Development\terminal-gui"

itemPath: f:\Development\terminal-gui\src\statusBar\StatusBarManager.ts
parentPath: f:\Development\terminal-gui\src\statusBar
folderPath: f:\Development\terminal-gui\src\statusBar
itemFullName: StatusBarManager.ts
itemName: StatusBarManager
itemExtension: ts
projectPath: f:\Development\terminal-gui
```

### Snippets

   The extension supports a snippets syntax that lets you create files with custom content on the fly (for creating templates I recommend using the [snippet-generator](https://snippet-generator.app/?description=&tabtrigger=&snippet=&mode=vscode). Copy only the `body` part).

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"temp": {
  "command": "echo -e _[my snippet]_ > temp.txt",
  "snippets": {
    "my snippet": [
      "hello",
      "world"
    ]
  },
},
```

In the above example, `my snippet` will be concatenated with the `\n` character and written to the file `temp.txt`. The terminal output will be:

```bash
$ echo -e "hello\nworld" > temp.txt
```

However, this approach has limitations: if the concatenated string contains special characters (`!`, `$`) or nested punctuation marks (eg: 'Lorem 'Ipsum' dolor'), then you need to be an expert bash script writer to handle such complex cases. These problems do not occur when copying from one file to another, so you may use this syntax:

  - `_[snippet]_.file` - creates a temporal file containing the provided content from the `snippets` property. The file is placed in `.vscode/terminal-gui.temp` folder (do not forget to add the folder in `.gitignore`).

For example:

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"temp": {
  "command": "cp _[my snippet 1]_.file temp1.txt && cp _[my snippet 2]_.file temp2.txt",
  "snippets": {
    "my snippet 1": [
      "hello",
      "world"
    ],
    "my snippet 2": [
      "lorem",
      "ipsum"
    ],
  },
},
```

In the above example, the terminal output will be:

```bash
cp "d:\my-project\.vscode\terminal-gui.temp\0-0.txt" temp1.txt && cp "d:\my-project\.vscode\terminal-gui.temp\0-1.txt" temp2.txt
```

### VSCode Snippets

You can also register **normal VSCode snippets** directly from your settings — no external `*.code-snippets` file needed. Simply add a `VSCodeSnippets` block next to `commands` (for creating snippets I recommend using the [snippet-generator](https://snippet-generator.app/?description=&tabtrigger=&snippet=&mode=vscode)):

```jsonc
// settings.json ➜ "TerminalGui.config"
{
  "commands": {
    "build": { "command": "npm run build" }
  },
  "VSCodeSnippets": {
    "if statement": {
      "scope": "javascript,typescript,javascriptreact,typescriptreact",
      "prefix": "if",
      "body": [
        "if ($1) {",
        "\t$0",
        "}"
      ],
      "description": "Standard JavaScript/TypeScript if statement"
    },
    "csharp-function": {
      "scope": "csharp",
      "prefix": "f",
      "body": [
        "${1:void} ${2:Foo}($3)",
        "{",
        "\t$0",
        "}"
      ],
      "description": "C# function template"
    }
  }
}
```

* Snippets appear in IntelliSense for the languages listed in `scope`.
* Full tab-stop and placeholder behaviour is supported.
* Press `Refresh` on the status bar or use `ctrl+alt+p` (for Mac: `cmd+alt+p`) to reload edited snippets immediately.

### Optional Scripts for Bash and PowerShell

You can define optional scripts in your configuration under the `scripts` property. These scripts let you source custom `Git Bash` or `PowerShell` code when executing commands.

For example, update your `settings.json` like this:

```json
// settings.json ➜ "TerminalGui.config": {...}
"commands": {
  "temp 1": {
    "command": "echo START && _[bashScript]_ && foo _[var1]_ && echo FINISH",
    "inputs": {
      "Enter any text; var1": ""
    }
  },
  "temp 2": {
    "command": "echo START ; _[shellScript]_ ; foo _[var1]_ ; echo FINISH",
    "inputs": {
      "Enter any text; var1": ""
    }
  }
},
"scripts": {
  "bash": [
    "foo() {",
    "  echo \"${1^^}.component\"",
    "}"
  ],
  "shell": [
    "function foo {",
    "    param([string]$text)",
    "    $text.ToUpper() + \".item\"",
    "}"
  ]
}
```

In the above example, if the user will enter "hello", then the output in terminal will be as follows:
```bash
$ echo START && source "d:\temp-app\.vscode\terminal-gui.temp\bash.sh" && foo hello && echo FINISH
START
HELLO.component
FINISH
```

**How It Works:**

- When your command contains `_[bashScript]_` (or `_[shellScript]_`), the extension checks if a script file (`bash.sh` or `shell.ps1`) exists in the `.vscode/terminal-gui.temp` folder.
- If the script file exists, it replaces the placeholder with a sourcing command (e.g. `source "path/to/bash.sh"` for `Git Bash` or `. "path/to/shell.ps1"` for `PowerShell`).
- If the script file doesn't exist but the corresponding `scripts` array is defined, the extension creates the script file with the given script content and then performs the substitution.

### Terminal-to-VSCode Modal Messages

This feature lets you pass information from the terminal to `VSCode` using a special command pattern. If the output of the command contains the syntax:

```bash
TERMINAL_GUI_MSG(Your message here)
```

then `VSCode` will extract the message from the brackets and display it in a modal window.

#### Example

```json
// settings.json
"TerminalGui.config": {
  "commands": {
    "temp": {
      "command": "_[bashScript]_ && foo _[var1]_",
      "inputs": {
        "Choose; var1": {
          "Agree": "true",
          "DisAgree": "false"
        }
      }
    },
  },
  "scripts": {
    "bash": [
      "foo() {",
      "    if [ \"$1\" = \"true\" ]; then",
      "        echo \"Ok\"",
      "    else",
      "        echo 'TERMINAL_GUI_MSG(Wrong Answer)'",
      "    fi",
      "}"
    ]
  }
},
```

In the example above, if the user selects "Agree", terminal output will be as follows:

```bash
$ source "d:\temp-app\.vscode\terminal-gui.temp\bash.sh" && foo true
Ok
```

but if the user selects "Disagree", a modal window will open with the message "Wrong Answer":

   ![Image](https://raw.githubusercontent.com/BachiMjavanadze/terminal-gui2/refs/heads/main/src/media/modal-window.avif)

and the terminal output will be as follows:

```bash
$ source "d:\temp-app\.vscode\terminal-gui.temp\bash.sh" && foo false
TERMINAL_GUI_MSG(Wrong Answer)
```

### Search and Filter Commands

  ![Image](https://github.com/user-attachments/assets/f12bfa46-c2c6-4518-a058-8e7b86074beb)

  - At the top of the commands dropdown, there is an input field that lets you search for commands.

  - In search mode, the extension will look through both ungrouped commands and those inside groups, displaying any command that matches your query.

  - When the search field is cleared, the dropdown reverts to its hierarchical view (including the recently used commands).

### Built-In Basic Settings

These settings, defined in your configuration under `settings.json ➜ "TerminalGui.config": { "basic": {} }`, control the appearance and behavior of built-in command buttons in the `VSCode` status and title bars.

```json
// settings.json
// "TerminalGui.configFile": ".vscode/terminalgui.config.json",

// settings.json ➜ "TerminalGui.config": { "basic": {...} }
"basic": {
  "recentlyUsedCommands": 7,
  "autoSaveToggler": true,
  "killAllTasks": true,
  "toggleTerminal": true,
  "pinTabBar": true
},
```

   - **autoSaveToggler**

     ![Image](https://github.com/user-attachments/assets/450403a2-0167-41d9-84bc-7175e7eae6d8)

     - When set to `true`, the extension displays auto save toggle buttons on the title bar.   
     - These buttons let you quickly switch between auto save enabled and disabled, giving you fast control over file saving behavior.

   - **killAllTasks**

     ![Image](https://github.com/user-attachments/assets/acc9fa62-4f82-45e7-b396-36fd8afc0859)

     Show `killAllTasks` button on status bar when `true` or `undefined`.

   - **toggleTerminal**

     ![Image](https://github.com/user-attachments/assets/618333cb-a471-4dc0-830e-cf875278e433)
   
     Show `toggleTerminal` button on status bar `true` or `undefined`.

   - **commandsMenuOnStatusBar**

     - Show commands menu button in the status-bar (right side. By default it is `true`).

   - **commandsMenuRefreshOnStatusBar**

     -  Show refresh button for Terminal GUI commands in the status-bar (right side. By default it is `true`).

   - **recentlyUsedCommands**

     - This setting controls how many recently used commands are shown at the top of the commands dropdown. The extension saves the full command names in a temporary folder (`.vscode/terminal-gui.temp`. Do not forget to add the folder in `.gitignore`) and displays them at the top of the dropdown for quick access. The maximum number of recently used commands displayed is configurable (default is 5; minimum 0, maximum 15).

   - **pinTabBar**

     - `VSCode` normally hides the tab bar when no files are open. When `true` (default), Terminal GUI “tricks” VS Code by opening a non-editable `Terminal GUI` placeholder file so the title/tab bar remains visible even with zero real files.  
     - Set to `false` to disable this workaround and allow the tab bar to collapse normally.

Set to false to disable this behavior.

### External Configuration File

You can use either JSON or JSONC (JSON with comments & trailing commas) for your external config.

The `TerminalGui.configFile` setting specifies the path to an external configuration file. You can name it `.json` or `.jsonc`, for example:

```jsonc
// settings.json
"TerminalGui.configFile": ".vscode/terminalgui.config.jsonc",
```

If `configFile` isn't defined, the extension will first look for `terminalgui.config.jsonc` in the workspace root, then for `terminalgui.config.json`. If neither file exists, it falls back to the `TerminalGui.config` section in your `settings.json`.

Example of `terminalgui.config.jsonc` might look like this:

```jsonc
{
  // Using a .jsonc file lets you add comments and trailing commas
  "basic": {
    "autoSaveToggler": true,
    "recentlyUsedCommands": 7,
    "commandsMenuOnStatusBar": true,
    "commandsMenuRefreshOnStatusBar": true,
  },
  "commands": {
    "bash example": {
      "command": "echo START && _[bashScript]_ && foo hello && echo FINISH"
    },
    "sell example": {
      "command": "echo START; _[shellScript]_; foo hello; echo FINISH"
    }
  },
  "scripts": {
    "bash": [
      "foo() {",
      "  echo \"${1^^}.component\"",
      "}"
    ],
    "shell": [
      "function foo {",
      "    param([string]$text)",
      "    $text.ToUpper() + \".item\"",
      "}"
    ]
  }
}
```

### Example Configuration

Below is an example of `settings.json` configuration for different frameworks.

- Note: all examples below are for the `Git Bash` terminal. To set the `Git Bash` as your default terminal use this configuration in `settings.json` of the `VSCode`:

```json
"terminal.integrated.defaultProfile.windows": "Git Bash",
```

- Note: If you copy and paste the example below into `settings.json` of `VSCode`, the changes will not take effect immediately; you will also need to `Refresh` the settings using the keyboard shortcut `ctrl+alt+p` (on Mac: `cmd+alt+p`).

<details>
<summary>🅰️ Angular</summary>

```json
// settings.json
"TerminalGui.config": {
  "commands": {
    "Create Angular Project": {
      "command": "cd _[Select a folder(Select a parent folder for the project)]_ _[name]_.value _[version]_.value && npm install -g @angular/cli@_[version]_ && ng new _[name]_ --style=_[styles]_ --inline-style=_[inline-style]_ --inline-template=_[inline-html]_ --skip-tests=_[tests]_ --ssr=_[ssr]_ && cd _[name]_ _[eslint]_ && code -r .",
      "group": "🅰️ Angular",
      "inputs": {
        "Enter a project name;name;false;false": "",
        "Choose the Angular version: eg.: 4.1.2; version": "latest",
        "Which stylesheet format would you like to use?;styles": {
          "CSS": "css",
          "SCSS": "scss",
        },
        "Inline Style?;inline-style": {
          "true": "true",
          "false": "false",
        },
        "Inline HTML?;inline-html": {
          "true": "true",
          "false": "false",
        },
        "Skip tests?;tests": {
          "true": "true",
          "false": "false",
        },
        "Do you want to enable SSR and SSG?;ssr": {
          "true": "true",
          "false": "false",
        },
        "Do you want to install ESlint?;eslint": {
          "true": "&& ng add @angular-eslint/schematics --skip-confirmation=true && ng lint",
          "false": " ",
        },
      },
      "settings": {
        "terminalName": "Angular Project Creation",
        "showWhenEmptyWorkspace": "emptyWorkspace",
        "revealConsole": true
      }
    },
    "Angular Server": {
      "command": "ng serve",
      "group": "🅰️ Angular",
      "icon": "▶;Run Angular Server",
      "icon2": "▢;Stop Angular Server",
      "settings": {
        "terminalName": "Angular Server",
        "quickButton": "statusBar",
        "showWhenEmptyWorkspace": "fullWorkspace"
      }
    },
    "Component": {
      "command": "cd _[folderPath]_ && ng g component --style css --standalone true --inline-style --inline-template --skip-tests --flat _[name]_",
      "group": "🅰️ Angular",
      "settings": {
        "contextMenu": true
      },
      "inputs": {
        "Enter the component name;name;false;false": "",
      },
    },
    "Service": {
      "command": "cd _[folderPath]_ && ng g service _[name]_",
      "group": "🅰️ Angular",
      "settings": {
        "contextMenu": true
      },
      "inputs": {
        "Enter the service name;name;false;false": "",
      },
    },
    "Directive": {
      "command": "cd _[folderPath]_ && ng g directive _[name]_",
      "group": "🅰️ Angular",
      "settings": {
        "contextMenu": true
      },
      "inputs": {
        "Enter the directive name;name;false;false": "",
      },
    },
    "Pipe": {
      "command": "cd _[folderPath]_ && ng g pipe _[name]_",
      "group": "🅰️ Angular",
      "settings": {
        "contextMenu": true
      },
      "inputs": {
        "Enter the pipe name;name;false;false": "",
      },
    },
    "Guard": {
      "command": "cd _[folderPath]_ && ng g guard _[name]_",
      "group": "🅰️ Angular",
      "settings": {
        "contextMenu": true
      },
      "inputs": {
        "Enter the guard name;name;false;false": "",
      },
    },
    "Interface": {
      "command": "cd _[folderPath]_ && ng g interface _[name]_",
      "group": "🅰️ Angular",
      "settings": {
        "contextMenu": true
      },
      "inputs": {
        "Enter the interface name;name;false;false": "",
      },
    },
    "Model": {
      "command": "cd _[folderPath]_ && ng g interface --type=model _[name]_",
      "group": "🅰️ Angular",
      "settings": {
        "contextMenu": true
      },
      "inputs": {
        "Enter the model name;name;false;false": "",
      },
    },
    "Class": {
      "command": "cd _[folderPath]_ && ng g class _[name]_",
      "group": "🅰️ Angular",
      "settings": {
        "contextMenu": true
      },
      "inputs": {
        "Enter the class name;name;false;false": "",
      },
    },
    "Enum": {
      "command": "cd _[folderPath]_ && ng g enum _[name]_",
      "group": "🅰️ Angular",
      "settings": {
        "contextMenu": true
      },
      "inputs": {
        "Enter the enum name;name;false;false": "",
      },
    },
    "Interceptor": {
      "command": "cd _[folderPath]_ && ng g interceptor _[name]_",
      "group": "🅰️ Angular",
      "settings": {
        "contextMenu": true
      },
      "inputs": {
        "Enter the interceptor name;name;false;false": "",
      },
    },
    "🛠️ Add Angular Libraries": {
      "command": "_[library]__[version]_ --skip-confirmation",
      "group": "🅰️ Angular",
      "inputs": {
        "Choose the library; library; true": {
          "NgRX": "ng add @ngrx/schematics@",
          "NgRX - store": "ng add @ngrx/store@",
          "NgRX - effects": "ng add @ngrx/effects@",
          "NgRX - entity": "ng add @ngrx/entity@",
          "Angular Material": "ng add @angular/material@",
          "NgBootstrap": "ng add @ng-bootstrap/ng-bootstrap@",
          "AngularFire": "ng add @angular/fire@",
        },
        "Choose the version: eg.: 4.1.2; version; false": "latest"
      },
      "settings": {
        "revealConsole": true
      },
    },
  },
  "VSCodeSnippets": {
    "class": {
      "scope": "html",
      "prefix": "a-class",
      "body": [
        "[class]=\"${1:expression}\""
      ],
      "description": "Angular [class] binding (Terminal GUI)"
    },
    "style": {
      "scope": "html",
      "prefix": "a-style",
      "body": [
        "[style.${1:property}]=\"${2:expression}\""
      ],
      "description": "Angular [style] binding"
    },
    "ngClass": {
      "scope": "html",
      "prefix": "a-ngClass",
      "body": [
        "[ngClass]=\"{${1:cssClass}: ${2:expression}}\""
      ],
      "description": "Angular ngClass (Terminal GUI)"
    },
    "ngFor": {
      "scope": "html",
      "prefix": "a-ngFor",
      "body": [
        "*ngFor=\"let ${1:item} of ${2:list}\"${0}"
      ],
      "description": "Angular *ngFor (Terminal GUI)"
    },
    "ngFor with trackBy": {
      "scope": "html",
      "prefix": "a-ngFor-trackBy",
      "body": [
        "*ngFor=\"let ${1:item} of ${2:list}; trackBy:${1:trackByFnName}\"${0}"
      ],
      "description": "Angular *ngFor with trackBy"
    },
    "ngForAsync": {
      "scope": "html",
      "prefix": "a-ngForAsync",
      "body": [
        "*ngFor=\"let ${1:item} of ${2:stream} | async as ${3:list}\"${0}"
      ],
      "description": "Angular *ngForAsync (Terminal GUI)"
    },
    "ngForm": {
      "scope": "html",
      "prefix": "a-form",
      "body": [
        "<form (ngSubmit)=\"onSubmit()\" #${1:form}=\"ngForm\">",
        "</form>"
      ],
      "description": "Form with ngSubmit and form attributes (Terminal GUI)"
    },
    "ngFormArrayName": {
      "scope": "html",
      "prefix": "a-formArrayName",
      "body": [
        "formArrayName=\"${1:control}\""
      ],
      "description": "Angular formArrayName (Terminal GUI)"
    },
    "ngFormControlName": {
      "scope": "html",
      "prefix": "a-formControlName",
      "body": [
        "formControlName=\"${1:control}\""
      ],
      "description": "Angular formControlName (Terminal GUI)"
    },
    "ngFormGroup": {
      "scope": "html",
      "prefix": "a-formGroup",
      "body": [
        "[formGroup]=\"${1:form}\""
      ],
      "description": "Angular formGroup (Terminal GUI)"
    },
    "ngFormGroupName": {
      "scope": "html",
      "prefix": "a-formGroupName",
      "body": [
        "[formGroupName]=\"${1:name}\""
      ],
      "description": "Angular formGroupName (Terminal GUI)"
    },
    "ngFormSubmit": {
      "scope": "html",
      "prefix": "a-form-submit",
      "body": [
        "<button type=\"submit\" [disabled]=\"!${1:form}.form.valid\">",
        "\tSave",
        "</button>"
      ],
      "description": "Angular form submit (Terminal GUI)"
    },
    "ngIf": {
      "scope": "html",
      "prefix": "a-ngIf",
      "body": [
        "*ngIf=\"${1:expression}\""
      ],
      "description": "Angular *ngIf (Terminal GUI)"
    },
    "ngIfElse": {
      "scope": "html",
      "prefix": "a-ngIfElse",
      "body": [
        "*ngIf=\"${1:expression};else ${2:templateName}\""
      ],
      "description": "Angular *ngIfElse (Terminal GUI)"
    },
    "ngModel": {
      "scope": "html",
      "prefix": "a-ngModel",
      "body": [
        "[(ngModel)]=\"${1:binding}\""
      ],
      "description": "Angular ngModel (Terminal GUI)"
    },
    "ngRouterLink": {
      "scope": "html",
      "prefix": "a-routerLink",
      "body": [
        "[routerLink]=\"['/${1:routePath}']\" routerLinkActive=\"${2:router-link-active}\" $0"
      ],
      "description": "Angular routerLink (Terminal GUI)"
    },
    "ngRouterLinkWithParameter": {
      "scope": "html",
      "prefix": "a-routerLink-param",
      "body": [
        "[routerLink]=\"['${1:routePath}', ${2:routeParameterValue}]\"",
        "routerLinkActive=\"${3:router-link-active}\"$0"
      ],
      "description": "Angular routerLink with a route parameter (Terminal GUI)"
    },
    "ngSelect": {
      "scope": "html",
      "prefix": "a-select",
      "body": [
        "<select [(ngModel)]=\"${1:model}\">",
        "\t<option *ngFor=\"let ${2:item} of ${3:list}\" [value]=\"${2:item}\">{{${2:item}}}</option>",
        "</select>"
      ],
      "description": "<select> control with ngModel (Terminal GUI)"
    },
    "ngStyle": {
      "scope": "html",
      "prefix": "a-ngStyle",
      "body": [
        "[ngStyle]=\"{${1:style}: ${2:expression}}\""
      ],
      "description": "Angular ngStyle (Terminal GUI)"
    },
    "ngSwitch": {
      "scope": "html",
      "prefix": "a-ngSwitch",
      "body": [
        "<div [ngSwitch]=\"${1:conditionExpression}\">",
        "\t<div *ngSwitchCase=\"${2:expression}\">${3:output}</div>",
        "\t<div *ngSwitchDefault>${4:output2}</div>",
        "</div>"
      ],
      "description": "Angular ngSwitch (Terminal GUI)"
    },
    "pre w/ json": {
      "scope": "html",
      "prefix": "a-prej",
      "body": [
        "<pre>{{${1:model} | json}}</pre>$0"
      ],
      "description": "Angular pre debug | json (Terminal GUI)"
    },
    "pre w/ async json": {
      "scope": "html",
      "prefix": "a-preja",
      "body": [
        "<pre>{{${1:model} | async | json}}</pre>$0"
      ],
      "description": "Angular pre debug | async | json (Terminal GUI)"
    },
    "ng-container": {
      "scope": "html",
      "prefix": "a-ng-container",
      "body": [
        "<ng-container $0></ng-container>"
      ],
      "description": "Angular ng-container (Terminal GUI)"
    },
    "ng-content": {
      "scope": "html",
      "prefix": "a-ng-content",
      "body": [
        "<ng-content select=\"[${0:selector}]\"></ng-content>"
      ],
      "description": "Angular ng-content (Terminal GUI)"
    }
  }
},
```
</details>

<details>
<summary>⚛️ React.js</summary>

```json
"TerminalGui.config": {
  "commands": {
    "Create React.js Project": {
      "command": "cd _[Select a folder]_ && _[projectName]_.value npx --yes create-vite@_[version]_ _[projectName]_ --template _[variant]_ && cd _[projectName]_ && npm i && code -r .",
      "group": "⚛️ React.js",
      "inputs": {
        "Enter a project name; projectName; false; false": "",
        "Choose the React.js version: eg.: 4.1.2; version; false": "latest",
        "Select a variant; variant": {
          "JavaScript": "react",
          "TypeScript": "react-ts"
        }
      },
      "settings": {
        "terminalName": "React Project Creation",
        "showWhenEmptyWorkspace": "emptyWorkspace",
        "revealConsole": true
      }
    },
    "React.js Server": {
      "command": "npm run dev",
      "group": "⚛️ React.js",
      "icon": "▶;Run React Dev Server",
      "icon2": "▢;Stop React Dev Server",
      "settings": {
        "terminalName": "React Server",
        "quickButton": "statusBar",
        "revealConsole": false,
        "showWhenEmptyWorkspace": "fullWorkspace"
      }
    },
    "Component": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && cmpName=\"_[componentName]_\" && capitalizeValue cmpName && createProcessedFile _[reactComponentSnippet]_.file \"${cmpName}.jsx\" cmpName",
      "group": "⚛️ React.js",
      "inputs": {
        "Enter component name; componentName; false; false": ""
      },
      "snippets": {
        "reactComponentSnippet": [
          "import React from 'react';",
          "",
          "const _{$cmpName}_ = () => {",
          "  return (",
          "    <div>",
          "      <h1>_{$cmpName}_ Component</h1>",
          "    </div>",
          "  );",
          "};",
          "",
          "export default _{$cmpName}_;"
        ]
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Custom Hook": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && hookName=\"_[hookName]_\" && capitalizeValue hookName && createProcessedFile _[reactHookSnippet]_.file \"${hookName}.js\" hookName",
      "group": "⚛️ React.js",
      "inputs": {
        "Enter hook name; hookName; false; false": ""
      },
      "snippets": {
        "reactHookSnippet": [
          "import { useState, useEffect, useCallback, useRef } from 'react';",
          "",
          "const use_{$hookName}_ = (initialValue, options = {}) => {",
          "  const [data, setData] = useState(initialValue);",
          "  const [loading, setLoading] = useState(false);",
          "  const [error, setError] = useState(null);",
          "",
          "  const isMounted = useRef(true);",
          "  const optionsRef = useRef(options);",
          "",
          "  useEffect(() => {",
          "    optionsRef.current = options;",
          "  }, [options]);",
          "",
          "  useEffect(() => {",
          "    return () => {",
          "      isMounted.current = false;",
          "    };",
          "  }, []);",
          "",
          "  const fetchData = useCallback(async (params = {}) => {",
          "    setLoading(true);",
          "    setError(null);",
          "",
          "    try {",
          "      // Your async logic here",
          "      const result = null; // Replace with actual result",
          "      if (isMounted.current) {",
          "        setData(result);",
          "      }",
          "      return result;",
          "    } catch (err) {",
          "      if (isMounted.current) {",
          "        setError(err);",
          "      }",
          "      return null;",
          "    } finally {",
          "      if (isMounted.current) {",
          "        setLoading(false);",
          "      }",
          "    }",
          "  }, []);",
          "",
          "  const reset = useCallback(() => {",
          "    setData(initialValue);",
          "    setError(null);",
          "  }, [initialValue]);",
          "",
          "  return {",
          "    data,",
          "    loading,",
          "    error,",
          "    fetchData,",
          "    reset,",
          "    setData",
          "  };",
          "};",
          "",
          "export default use_{$hookName}_;"
        ]
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Context Provider": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && ctxName=\"_[contextName]_\" && capitalizeValue ctxName && createProcessedFile _[reactContextSnippet]_.file \"${ctxName}Context.js\" ctxName",
      "group": "⚛️ React.js",
      "inputs": {
        "Enter context name; contextName; false; false": ""
      },
      "snippets": {
        "reactContextSnippet": [
          "import { createContext, useContext, useState } from 'react';",
          "",
          "const _{$ctxName}_Context = createContext(null);",
          "",
          "export const use_{$ctxName}_Context = () => {",
          "  const context = useContext(_{$ctxName}_Context);",
          "  if (!context) {",
          "    throw new Error('use_{$ctxName}_Context must be used within a _{$ctxName}_ContextProvider');",
          "  }",
          "  return context;",
          "};",
          "",
          "export const _{$ctxName}_ContextProvider = ({ children }) => {",
          "  const [value, setValue] = useState('default value');",
          "  const [loading, setLoading] = useState(false);",
          "",
          "  const updateValue = (newValue) => setValue(newValue);",
          "",
          "  const contextValue = {",
          "    value,",
          "    loading,",
          "    updateValue,",
          "    setLoading",
          "  };",
          "",
          "  return (",
          "    <_{$ctxName}_Context.Provider value={contextValue}>",
          "      {children}",
          "    </_{$ctxName}_Context.Provider>",
          "  );",
          "};"
        ]
      },
      "settings": {
        "contextMenu": true
      }
    },
    "🛠️ Install React Libraries": {
      "command": "npm i _[package]_@_[version]_ _[dependency]_",
      "group": "⚛️ React.js",
      "inputs": {
        "Choose the package; package; true": {
          "React Router": "react-router-dom",
          "Redux": "redux react-redux redux-thunk",
          "Material UI": "@mui/material @emotion/react @emotion/styled",
          "Styled Components": "styled-components",
          "Axios": "axios",
          "Formik & Yup": "formik yup",
          "React Query": "@tanstack/react-query"
        },
        "Choose the version: eg.: 4.1.2; version; false": "latest",
        "Saved package as a dependency or devDependency; dependency": {
          "dependency": "-S",
          "devDependency": "-D"
        }
      },
      "settings": {
        "revealConsole": true
      }
    },
  },
  "scripts": {
    "bash": [
      "# Capitalize value: lowercases everything and then capitalizes the first character.",
      "capitalizeValue() {",
      "  local v=\"${!1}\"",
      "  v=\"${v,,}\"",
      "  v=\"${v^}\"",
      "  printf -v \"$1\" '%s' \"$v\"",
      "}",
      "",
      "# File parser: replaces tokens of the form _{\\$<variable>}_ with the value of that variable.",
      "fileParser() {",
      "  sed \"s|_{\\\\\\$${1}}_|${!1}|g\" \"$2\"",
      "}",
      "",
      "# File create: copies a file if target does not exist; otherwise, reports an error.",
      "fileCreate() {",
      "  if [ -f \"$2\" ]; then",
      "    echo -e \"\\e[41m\\e[1m\\e[97mFile '$2' already exists!\\e[0m\" >&2",
      "    echo \"TERMINAL_GUI_MSG(Terminal GUI warning: File '$2' already exists!)\"",
      "    return 0",
      "  else",
      "    cp \"$1\" \"$2\" && code \"$2\"",
      "  fi",
      "}",
      "",
      "# Create processed file: checks if the destination file exists.",
      "# If not, it reads the source snippet file, uses sed to replace all tokens of the form _{\\$<variable>}_ ",
      "# with the current value of that variable, writes the result to the destination file, and opens it.",
      "createProcessedFile() {",
      "  local src=\"$1\"",
      "  local dest=\"$2\"",
      "  local varName=\"$3\"",
      "  if [ -f \"$dest\" ]; then",
      "    echo -e \"\\e[41m\\e[1m\\e[97mFile '$dest' already exists!\\e[0m\" >&2",
      "    echo \"TERMINAL_GUI_MSG(Terminal GUI warning: File '$2' already exists!)\"",
      "    return 0",
      "  fi",
      "  local varValue=\"${!varName}\"",
      "  sed \"s|_{\\\\\\$${varName}}_|${varValue}|g\" \"$src\" > \"$dest\" && code \"$dest\"",
      "}",
    ]
  }
},
```
</details>

<details>
<summary>🟩 Node.js</summary>

```json
// settings.json
"TerminalGui.config": {
  "commands": {
    "Create Node.js Project": {
      "command": "cd _[Select a folder]_ && mkdir _[projectName]_ && cd _[projectName]_ && npm init -y && npm pkg set type=\"module\" main=\"_[entryPoint]_.js\" scripts.start=\"node --watch _[entryPoint]_.js\" && npm pkg delete scripts.test && npm i express@_[version]_ && cp _[mainFile]_.file _[entryPoint]_.js && code -r .",
      "group": "🟩 Node.js",
      "inputs": {
        "Enter a project name; projectName": "",
        "Enter an entry-point file name; entryPoint; false; false": "index",
        "Choose the Express.js version: eg.: 4.1.2; version; false": "latest",
      },
      "snippets": {
        "mainFile": [
          "import express from 'express';",
          "",
          "const app = express();",
          "const PORT = process.env.PORT || 3000;",
          "",
          "app.get('/', (req, res) => {",
          "  res.send('Hello World!');",
          "});",
          "",
          "app.listen(PORT, () => {",
          "  console.log(`Server running on http://localhost:${PORT}`);",
          "});"
        ]
      },
      "settings": {
        "terminalName": "NodeJS Project Creation",
        "showWhenEmptyWorkspace": "emptyWorkspace",
        "revealConsole": true
      }
    },
    "Run Node.js Server": {
      "command": "npm run start",
      "icon": "▶;run server",
      "icon2": "▢;stop server",
      "group": "🟩 Node.js",
      "settings": {
        "terminalName": "Node.js Server",
        "quickButton": "statusBar",
        "showWhenEmptyWorkspace": "fullWorkspace"
      }
    },
    "🛠️ Install NPM Packages": {
      "command": "npm i _[package]_@_[version]_ _[dependency]_",
      "group": "🟩 Node.js",
      "inputs": {
        "Choose the package; package; true": {
          "nodemailer": "nodemailer",
          "socket.io": "socket.io",
          "helmet": "helmet",
          "dotenv": "dotenv",
          "cors": "cors",
          "mongoose": "mongoose",
          "jsonwebtoken": "jsonwebtoken",
          "bcrypt": "bcrypt",
          // "express": "express",
          // "winston": "winston",
        },
        "Choose the version: eg.: 4.1.2; version; false": "latest",
        "Saved package as a dependency or devDependency;dependency": {
          "dependency": "-S",
          "devDependency": "-D"
        },
      },
      "settings": {
        "revealConsole": true,
      }
    },
  }
},
```
</details>

### Known Issues
- Placeholder IntelliSense may not activate until `VSCode` is restarted after installing or reactivating the extension.

## Support and Feedback

Please feel free to report any issues or suggestions. Check out the [GitHub repository](https://github.com/BachiMjavanadze/terminal-gui2) for more details.

#### [Change Log](CHANGELOG.md)

## License

This project is licensed under the [EULA License](https://github.com/BachiMjavanadze/terminal-gui2/blob/main/LICENSE).
