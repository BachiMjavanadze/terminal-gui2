## `Terminal GUI`

**`Terminal GUI`** is a `VS Code` extension that lets you define custom terminal commands with interactive inputs. You can configure your commands in your `settings.json` (or in an external config file. See below how property `configFile` works), and when you run a command, any placeholders you've defined will prompt you for input.

   - Note: **`Terminal GUI`** supports `Git Bash` and `PowerShell` scripting languages, so you can write very complex commands.

   - Note: all examples below are for the `Git Bash` terminal. To set the `Git Bash` as your default terminal use this configuration in `settings.json` of the `VS Code`:

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
   
   - If `"contextMenu": true` is set, the command is a context menu command, meaning it can be launched by right-clicking on a file or a folder in the `VS Code` explorer (or by combination of keybinding: `ctrl+shift+e`, navigate with up/down keyboard arrow buttons through files/folders, `shift+F10`, choose `Terminal GUI` menu item).

   - Regular commands can be launched via the button in the status bar (default keybinding: `ctrl+alt+l`. For Mac: `cmd+alt+l`).

![Image](https://raw.githubusercontent.com/BachiMjavanadze/terminal-gui2/refs/heads/main/src/media/regular%20commands.avif)

   - When a command contains placeholders, you'll be prompted to enter the required values.

   - You can navigate through inputs with left/right arrows located on the title bar of the command or use keybindings:

        - **Navigate Right:** `ctrl+alt+.` (for Mac: `cmd+alt+.`)
        - **Navigate Left:** `ctrl+alt+,`  (for Mac: `cmd+alt+,`)
   
   - The `Enter` button allows you to move forward and execute commands.

### Input Fields Overview

`Terminal GUI` supports **five types of input fields** that you can mix and match inside your commands. Each type has its own syntax and behavior, allowing you to create dynamic and interactive command strings.

⚠️ **Important rule:** It is impossible to reference one input field inside another.

```json
"hello": {
  "command": "echo _[var1]_ && _[var2]_",
  "inputs": {
    "temp 1; var1": "",
    "temp 2; var2": {
      "Red": "echo red",
      "Green": "echo _[var1]_"  // impossible
    }
  }
}
```

The available input field types are:

1. **Free Text Input** – user types arbitrary text (with options for default value, allow empty, allow spaces, and saving).
2. **Choice List Input** – user selects one value from a predefined list (optionally allow custom values).
3. **Checkbox List Input** – user selects multiple values from a predefined list, joined with `connectItems`.
4. **Two-state “Toggler” Input** – behaves like a switch; checked/unchecked expands to different snippets.
5. **Built-in Special Inputs** – reserved placeholders like `_[Select a folder]_`, `_[Select a file]_`, or context variables (`_[projectPath]_`, `_[itemPath]_`, etc.).

Image of choice list input:

![Image](https://github.com/user-attachments/assets/80d5013c-8573-49ab-83c7-6ee3aca8e826)

### Terminal Freeze Prevention

   - To prevent `VS Code` terminals from freezing, `Terminal GUI` automatically sends special commands:
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

![Image](https://raw.githubusercontent.com/BachiMjavanadze/terminal-gui2/refs/heads/main/src/media/input-field.avif)

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
      - **allowEmpty:** Set to `true` if empty input is allowed (default is `false`).
      - **allowSpaces:** Set to `true` if spaces between words are allowed (default is `true`).
      - **save:** If you use `;save`, on each run you'll see a QuickPick with two checkboxes:
        * **Save** – remember the value but still prompt next time.
        * **Save & Skip** – remember the value and auto-apply on future runs.

For example:

![Image](https://raw.githubusercontent.com/BachiMjavanadze/terminal-gui2/refs/heads/main/src/media/save-input.avif)

```json
// settings.json ➜ "TerminalGui.config": { "commands": {...} }
"lorem 1": {
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
  "group": "ASP.NET WEB API",
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
  "group": "ASP.NET WEB API",
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

   - The choice list input key supports an extended syntax:
     ```
     "prompt text; substitution placeholder; allowCustomValue; allowSpaces"
     ```
      - **prompt text:** The text displayed as the prompt for the user.
      - **substitution placeholder:** The token in the command that will be replaced with the chosen (or entered) value.
      - **allowCustomValue:** A flag (`true` or `false`) that, when set to `true`, allows the user to enter a custom value that isn’t in the predefined choice list. If `false` (or omitted), the user must select one of the predefined options.
      - **allowSpaces:** Set to `true` if spaces between words are allowed (default is `true`). If set to `false`, the user cannot enter spaces.

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

![Image](https://raw.githubusercontent.com/BachiMjavanadze/terminal-gui2/refs/heads/main/src/media/checkbox-list.avif)

You can present a multi-select (checkbox-style) list.  
Declare the choice object with a special key **`"connectItems"`** whose value is the string used to join the chosen command snippets.

- The checkbox list input key supports an extended syntax:
  ```text
  "prompt text; substitution placeholder; allowEmpty; save"
  ```
  - **prompt text:** The text displayed as the prompt for the user.
  - **substitution placeholder:** The token in the command that will be replaced with the chosen (or entered) values.
  - **allowEmpty:** Set to `true` if empty input is allowed (default is `false`).
  - **save:** Set to `true` to remember and pre-select previously chosen items on next run (default is `false`).

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

#### Two‐state “toggler” input

![Image](https://raw.githubusercontent.com/BachiMjavanadze/terminal-gui2/refs/heads/main/src/media/toggler-input.avif)

You can define a two‐state toggler option by including exactly one `_[toggle]_` in both the key and the value of a checkbox input. It behaves like a single‐checkbox toggle:  
- 🟩 (checked) runs the **left** snippet  
- 🟥 (unchecked) runs the **right** snippet  

You still need `"connectItems"` defined to join multiple command fragments. By default, Two‐state “toggler” is checked.

**Syntax**  
```json
"prompt text; substitution placeholder; allowEmpty; save": {
  "connectItems": "<joiner>",
  "LabelA _[toggle]_ LabelB": "cmdA _[toggle]_ cmdB"
}
```
Example:

```json
"lorem": {
  "command": "_[var1]_",
  "inputs": {
    "tmp; var1; true; true; save": {
      "connectItems": "&&",
      "Red _[toggle]_ Brown": "echo 'red color' _[toggle]_ echo 'brown color'",
      "Blue _[toggle]_ Yellow": "echo 'blue color' _[toggle]_ echo 'yellow color'",
      "Green _[toggle]_ Black": "echo 'green color' _[toggle]_ echo 'black color'"
    },
  }
},
```

- Default shows 🟩 Red, Blue, Green

- Uncheck shows 🟥 Brown, Yellow, Black

If the same input is used multiple times in the `command` property, the user will only see one input. eg.:

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

### Command settings

   You can customize a command with the `settings` property, which is an object with optional properties:

   - **`terminalName`**  
     Label to be shown as terminal name. If none, regular terminal name will be shown instead.

   - **`quickButton`**  
    Where to render this command’s icon as a quick-access button. One of:  
      - `"statusBar"` – show icon on the status bar.
      - `"tabBar"` – show icon in the editor title/tab bar (`VS Code` allows only 8 icons visible; others will be hidden under the `More Actions...` button).
      - `"both"` – show icon in both places.
        - Note: `VS Code` doesn’t support dynamic title/tab-bar updates at runtime, so `Terminal GUI` rewrites `package.json` and will prompt you to reload the window **twice** to apply changes.

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
### Command Icons (`icon`)

You can assign an `icon` property to a command to display a quick-access button in the **status bar** or the **tab/title bar** (depending on the `quickButton` setting).
The icon can be an emoji or an HTML entity, and you may also include an optional tooltip after a semicolon.

**Syntax:**

```json
"commandName": {
  "command": "echo Hello",
  "icon": "⚡;Run Command",
  "settings": {
    "quickButton": "statusBar" // or "tabBar" or "both"
  }
}
```

* The first part (`⚡`) is the symbol.
* The second part (`Run Command`) is shown as a tooltip when you hover over the icon.
* The location is controlled by `"quickButton"` (status bar, tab bar, or both).

### Long-Running Commands (`icon2`)

   - If a command is long running (server watching, project creation), you may define `icon` and `icon2` poperies. You can use an emoji or an HTML entity as an icon.

       - `icon` - an icon and optional tooltip (format: `icon;tooltip`) to display when command is not running.
       - `icon2` - an icon, tooltip and optional interrupts count (format: `icon;tooltip;interrupts`) to display when command is running. The interrupt count controls how the command is stopped:

         - **Stopping Behavior:**  
         When you click a long-running command (one with `icon2` defined) a second time, `Terminal GUI` stops its execution by sending interrupt signals (`^C`) to the terminal.

         - **Custom Interrupt Count:**  
         You can specify how many `^C` signals should be sent by adding a number after the tooltip in `icon2`, separated by a semicolon (e.g. `"▢;stop server;4"`).

         - **Default Behavior:**  
        If no interrupt count is provided (for example `"▢;stop server"`), the extension will send two `^C` signals – the first immediately and the second after a 300ms delay. This is because `VS Code` sometimes "swallows" one `^C` signal, meaning that instead of sending *n* signals, only *n-1* actually reach the terminal. Sending an extra signal helps ensure that the process is properly stopped.

   - **Auto-reveal once:**
       For long-running commands, even when `settings.revealConsole` is `false`, the terminal panel is revealed **the first time** the command starts. On later runs the panel stays hidden unless `revealConsole` is `true` or the process exits with an error.

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

`Terminal GUI` supports several built-in variables for context menu commands (so `contextMenu` property should be `true`. Exception: `_[projectPath]_` — this variable always works, even for regular commands, not only context menu). These variables are embedded in your command strings using the format `_[variableName]_` and are automatically replaced at runtime with context-specific values based on the selected file or folder.

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
      The path of the workspace folder (project) in which the command was executed (works in both regular and context menu commands).

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
  "command": "echo Filename: _[itemName]_",
  "settings": {
    "contextMenu": true
  },
},
```

  - In the above example, if user selects a file with the name of "hello world.txt", then the output in terminal will be:

```bash
$ echo Filename: "hello world"
Filename: hello world
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

This feature lets you pass information from the terminal to `VS Code` using a special command pattern. If the output of the command contains the syntax:

```bash
TERMINAL_GUI_MSG(Your message here)
```

then `VS Code` will extract the message from the brackets and display it in a modal window.

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

These settings, defined in your configuration under `settings.json ➜ "TerminalGui.config": { "basic": {} }`, control the appearance and behavior of built-in command buttons in the `VS Code` status and title bars.

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

     - `VS Code` normally hides the tab bar when no files are open. When `true` (default), Terminal GUI “tricks” `VS Code` by opening a non-editable `Terminal GUI` placeholder file so the title/tab bar remains visible even with zero real files.  
     - Set to `false` to disable this workaround and allow the tab bar to collapse normally.

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

- Note: all examples below are for the `Git Bash` terminal. To set the `Git Bash` as your default terminal use this configuration in `settings.json` of the `VS Code`:

```json
"terminal.integrated.defaultProfile.windows": "Git Bash",
```

- Note: If you copy and paste the example below into `settings.json` of `VS Code`, the changes will not take effect immediately; you will also need to `Refresh` the settings using the keyboard shortcut `ctrl+alt+p` (on Mac: `cmd+alt+p`).

<details>
<summary>🅰️ Angular</summary>

```json
// settings.json
"TerminalGui.config": {
  "commands": {
    "Create Angular Project": {
      "command": "cd _[Select a folder(Select a parent folder for the project (press ENTER))]_ _[name]_.value _[version]_.value && npm install -g @angular/cli@_[version]_ && angularProjectPath=\"_[name]_\" && ng new \"_[name]_\" _[opts]_ && { [[ \"${PWD##*/}\" == \"$angularProjectPath\" ]] || cd \"$angularProjectPath\"; } && code -r .",
      "group": "🅰️ Angular",
      "inputs": {
        "Enter a project name; name; false; false": "",
        "Choose the Angular version: eg.: 4.1.2; version": "latest",
        "Angular options; opts; false; true": {
          "connectItems": " ",
          "CSS _[toggle]_ SCSS": "--style=css _[toggle]_ --style=scss",
          "Inline Style _[toggle]_ No Inline Style": "--inline-style=true _[toggle]_ --inline-style=false",
          "Inline HTML _[toggle]_ No Inline HTML": "--inline-template=true _[toggle]_ --inline-template=false",
          "Skip tests _[toggle]_ Don't skip tests": "--skip-tests=true _[toggle]_ --skip-tests=false",
          "Enable SSR/SSG _[toggle]_ No SSR/SSG": "--ssr=true _[toggle]_ --ssr=false",
          "Install ESLint _[toggle]_ Skip ESLint": "&& cd \"$angularProjectPath\" && ng add @angular-eslint/schematics --skip-confirmation=true && ng lint _[toggle]_ && :"
        }
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
// settings.json
"TerminalGui.config": {
  "commands": {
    "Create React.js Project": {
      "command": "cd _[Select a folder(Select a parent folder for the project)]_ && tmpl=react && TG_DO_CSS=0 && TG_DO_PRETTIER=0 _[projectName]_.value _[version]_.value && _[reactOptions]_ && npx --yes create-vite@_[version]_ \"_[projectName]_\" --template \"$tmpl\" && cd \"_[projectName]_\" && npm i && _[bashScript]_ && tg_lang \"$PWD\" && ( [[ ${TG_DO_CSS:-0} -eq 1 ]] && tg_css_to_scss || : ) && ( [[ ${TG_DO_PRETTIER:-0} -eq 1 ]] && tg_setup_prettier _[prettierRc]_.file _[prettierIgnore]_.file || : ) && code -r .",
      "group": "⚛️ React.js",
      "inputs": {
        "Enter a project name; projectName; false; false": "",
        "Choose the React.js version: eg.: 4.1.2; version; false": "latest",
        "React options; reactOptions; false; false": {
          "connectItems": "&&",
          "JavaScript _[toggle]_ TypeScript": "tmpl=react _[toggle]_ tmpl=react-ts",
          "CSS _[toggle]_ SCSS": "TG_DO_CSS=0 _[toggle]_ TG_DO_CSS=1",
          "Prettier _[toggle]_ Skip Prettier": "TG_DO_PRETTIER=1 _[toggle]_ TG_DO_PRETTIER=0"
        }
      },
      "snippets": {
        "prettierRc": [
          "{",
          "  \"singleQuote\": true,",
          "  \"semi\": true,",
          "  \"trailingComma\": \"es5\",",
          "  \"printWidth\": 100",
          "}"
        ],
        "prettierIgnore": [
          "dist",
          "build",
          "coverage",
          ".vscode/terminal-gui.temp"
        ]
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
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_lang _[projectPath]_ && cmpName=\"_[componentName]_\" && capitalizeValue cmpName && SNIP_JS=_[reactComponentSnippetJS]_.file && SNIP_TS=_[reactComponentSnippetTS]_.file && SRC=\"$SNIP_JS\"; [ \"$TG_LANG\" = ts ] && SRC=\"$SNIP_TS\"; createProcessedFile \"$SRC\" \"${cmpName}.${JSX_EXT}\" cmpName",
      "group": "⚛️ React.js",
      "inputs": {
        "Enter component name; componentName; false; false": ""
      },
      "snippets": {
        "reactComponentSnippetJS": [
          "const _{$cmpName}_ = () => (",
          "  <div>",
          "    <h1>_{$cmpName}_ Component</h1>",
          "  </div>",
          ");",
          "",
          "export default _{$cmpName}_;"
        ],
        "reactComponentSnippetTS": [
          "const _{$cmpName}_ = () => (",
          "  <div>",
          "    <h1>_{$cmpName}_ Component</h1>",
          "  </div>",
          ");",
          "",
          "export default _{$cmpName}_;"
        ]
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Custom Hook": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_lang _[projectPath]_ && hookName=\"_[name]_\" && capitalizeValue hookName && SNIP_JS=_[reactHookSnippetJS]_.file && SNIP_TS=_[reactHookSnippetTS]_.file && SRC=\"$SNIP_JS\"; [ \"$TG_LANG\" = ts ] && SRC=\"$SNIP_TS\"; createProcessedFile \"$SRC\" \"use${hookName}.${HOOK_EXT}\" hookName && code \"use${hookName}.${HOOK_EXT}\"",
      "group": "⚛️ React.js",
      "inputs": {
        "Hook base name (e.g. Toggle); name; false; false": ""
      },
      "snippets": {
        "reactHookSnippetJS": [
          "import { useState, useCallback } from 'react';",
          "",
          "export default function use_{$hookName}_ (initial = false) {",
          "  const [on, setOn] = useState(initial);",
          "  const toggle = useCallback(() => {",
          "    setOn(v => !v);",
          "  }, []);",
          "  return { on, toggle, setOn };",
          "}"
        ],
        "reactHookSnippetTS": [
          "import { useState, useCallback } from 'react';",
          "",
          "export default function use_{$hookName}_(initial: boolean = false) {",
          "  const [on, setOn] = useState<boolean>(initial);",
          "  const toggle = useCallback(() => {",
          "    setOn(v => !v);",
          "  }, []);",
          "  return { on, toggle, setOn } as const;",
          "}"
        ]
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Context": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_lang _[projectPath]_ && ctxName=\"_[contextName]_\" && capitalizeValue ctxName && CORE_JS=_[reactContextCoreJS]_.file && CORE_TS=_[reactContextCoreTS]_.file && PROV_JS=_[reactContextProviderJS]_.file && PROV_TS=_[reactContextProviderTS]_.file && CORE=\"$CORE_JS\" && PROV=\"$PROV_JS\" && [ \"$TG_LANG\" = ts ] && CORE=\"$CORE_TS\" && PROV=\"$PROV_TS\" ; createProcessedFile \"$CORE\" \"${ctxName}Context.${HOOK_EXT}\" ctxName && createProcessedFile \"$PROV\" \"${ctxName}Provider.${JSX_EXT}\" ctxName",
      "group": "⚛️ React.js",
      "inputs": {
        "Enter context name; contextName; false; false": ""
      },
      "snippets": {
        "reactContextCoreJS": [
          "import { createContext, useContext } from 'react';",
          "",
          "export const _{$ctxName}_Context = createContext(null);",
          "_{$ctxName}_Context.displayName = '_{$ctxName}_Context';",
          "",
          "export function use_{$ctxName}_() {",
          "  const c = useContext(_{$ctxName}_Context);",
          "  if (!c) throw new Error('use_{$ctxName}_ must be used within _{$ctxName}_Provider');",
          "  return c;",
          "}"
        ],
        "reactContextProviderJS": [
          "import { useMemo, useState } from 'react';",
          "import { _{$ctxName}_Context } from './_{$ctxName}_Context';",
          "",
          "export default function _{$ctxName}_Provider({ children, initial = null }) {",
          "  const [value, setValue] = useState(initial);",
          "  const ctx = useMemo(() => ({ value, setValue }), [value]);",
          "  return (",
          "    <_{$ctxName}_Context.Provider value={ctx}>",
          "      {children}",
          "    </_{$ctxName}_Context.Provider>",
          "  );",
          "}"
        ],
        "reactContextCoreTS": [
          "import { createContext, useContext, type Dispatch, type SetStateAction } from 'react';",
          "",
          "export type _{$ctxName}_Ctx = {",
          "  value: unknown;",
          "  setValue: Dispatch<SetStateAction<unknown>>;",
          "};",
          "",
          "export const _{$ctxName}_Context = createContext<_{$ctxName}_Ctx | null>(null);",
          "_{$ctxName}_Context.displayName = '_{$ctxName}_Context';",
          "",
          "export function use_{$ctxName}_(): _{$ctxName}_Ctx {",
          "  const c = useContext(_{$ctxName}_Context);",
          "  if (!c) throw new Error('use_{$ctxName}_ must be used within _{$ctxName}_Provider');",
          "  return c;",
          "}"
        ],
        "reactContextProviderTS": [
          "import { useMemo, useState, type ReactNode } from 'react';",
          "import { _{$ctxName}_Context, type _{$ctxName}_Ctx } from './_{$ctxName}_Context';",
          "",
          "type _{$ctxName}_ProviderProps = {",
          "  children: ReactNode;",
          "  initial?: unknown;",
          "};",
          "",
          "export default function _{$ctxName}_Provider({ children, initial = null }: _{$ctxName}_ProviderProps) {",
          "  const [value, setValue] = useState<unknown>(initial);",
          "  const ctx = useMemo<_{$ctxName}_Ctx>(() => ({ value, setValue }), [value]);",
          "  return (",
          "    <_{$ctxName}_Context.Provider value={ctx}>",
          "      {children}",
          "    </_{$ctxName}_Context.Provider>",
          "  );",
          "}"
        ]
      },
      "settings": {
        "contextMenu": true
      }
    },
    "API service": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_lang _[projectPath]_ && SNIP_JS=_[APIserviceSnippetJS]_.file && SNIP_TS=_[APIserviceSnippetTS]_.file && SRC=\"$SNIP_JS\"; [ \"$TG_LANG\" = ts ] && SRC=\"$SNIP_TS\"; cp \"$SRC\" \"_[fileName]_.${SERVICE_EXT}\" && code -r \"_[fileName]_.${SERVICE_EXT}\"",
      "group": "⚛️ React.js",
      "inputs": {
        "Enter file name; fileName; false; false": ""
      },
      "snippets": {
        "APIserviceSnippetJS": [
          "export async function getJson(url, init) {",
          "  const r = await fetch(url, init);",
          "  if (!r.ok) throw new Error(String(r.status));",
          "  return r.json();",
          "}",
          "",
          "export async function postJson(url, body, init = {}) {",
          "  return getJson(url, {",
          "    ...init,",
          "    method: 'POST',",
          "    headers: { 'Content-Type': 'application/json', ...(init.headers || {}) },",
          "    body: JSON.stringify(body)",
          "  });",
          "}"
        ],
        "APIserviceSnippetTS": [
          "export async function getJson<T = unknown>(url: string, init?: RequestInit): Promise<T> {",
          "  const r = await fetch(url, init);",
          "  if (!r.ok) throw new Error(String(r.status));",
          "  return r.json() as Promise<T>;",
          "}",
          "",
          "export async function postJson<T = unknown>(url: string, body: unknown, init: RequestInit = {}): Promise<T> {",
          "  return getJson<T>(url, {",
          "    ...init,",
          "    method: 'POST',",
          "    headers: { 'Content-Type': 'application/json', ...(init.headers || {}) },",
          "    body: JSON.stringify(body)",
          "  });",
          "}"
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
      "# React helpers — root-scoped cache",
      "TG_LANG=''; JSX_EXT=jsx; HOOK_EXT=js; SERVICE_EXT=js",
      "",
      "tg_lang() {",
      "  # Usage: tg_lang <project-root>; creates/reads cache at <root>/.vscode/terminal-gui.temp/react_lang.txt",
      "  local root=\"${1:-$PWD}\"",
      "  local cache_dir=\"$root/.vscode/terminal-gui.temp\"",
      "  local cache_file=\"$cache_dir/react_lang.txt\"",
      "  local lang=''",
      "",
      "  # read cache if present",
      "  if [ -f \"$cache_file\" ]; then",
      "    lang=$(tr -d '\\r\\n' < \"$cache_file\")",
      "  fi",
      "",
      "  # detect/write if missing",
      "  if [ -z \"$lang\" ]; then",
      "    if compgen -G \"$root/tsconfig*.json\" >/dev/null; then lang=ts;",
      "    elif [ -f \"$root/package.json\" ] && grep -qiE '\"typescript\"|\"@types/react\"|@typescript-eslint' \"$root/package.json\"; then lang=ts;",
      "    elif compgen -G \"$root/**/*.ts\" >/dev/null || compgen -G \"$root/**/*.tsx\" >/dev/null; then lang=ts;",
      "    else lang=js; fi",
      "    mkdir -p \"$cache_dir\"",
      "    printf '%s' \"$lang\" > \"$cache_file\"",
      "  fi",
      "",
      "  TG_LANG=\"$lang\"",
      "  if [ \"$TG_LANG\" = ts ]; then JSX_EXT=tsx; HOOK_EXT=ts; SERVICE_EXT=ts; else JSX_EXT=jsx; HOOK_EXT=js; SERVICE_EXT=js; fi",
      "}",
      "",
      "# utils used by the React commands",
      "capitalizeValue() {",
      "  local v=\"${!1}\"; v=\"${v,,}\"; v=\"${v^}\"; printf -v \"$1\" '%s' \"$v\"",
      "}",
      "createProcessedFile() {",
      "  local src=\"$1\" dest=\"$2\" varName=\"$3\"",
      "  if [ -e \"$dest\" ]; then",
      "    echo \"TERMINAL_GUI_MSG(Terminal GUI warning: File '$dest' already exists!)\"",
      "    return 0",
      "  fi",
      "  local vv=\"${!varName}\"",
      "  sed \"s|_{\\\\\\$${varName}}_|${vv}|g\" \"$src\" > \"$dest\" && code \"$dest\"",
      "}",
      "tg_css_to_scss() {",
      "  npm i -D sass || return 0",
      "  for f in src/App.css src/index.css; do [ -f \"$f\" ] && mv \"$f\" \"${f%.css}.scss\"; done",
      "  for f in src/main.jsx src/main.tsx src/App.jsx src/App.tsx; do [ -f \"$f\" ] && sed -i 's/\\.css/\\.scss/g' \"$f\"; done",
      "}",
      "tg_setup_prettier() {",
      "  local rc=\"$1\" ig=\"$2\"",
      "  rc=${rc%\\\"}; rc=${rc#\\\"}; rc=${rc%\\'}; rc=${rc#\\'}",
      "  ig=${ig%\\\"}; ig=${ig#\\\"}; ig=${ig%\\'}; ig=${ig#\\'}",
      "  npm i -D prettier || return 0",
      "  if [ -f \"$rc\" ]; then cp \"$rc\" .prettierrc.json; else echo '{\"singleQuote\":true,\"semi\":true,\"trailingComma\":\"es5\",\"printWidth\":100}' > .prettierrc.json; fi",
      "  if [ -f \"$ig\" ]; then cp \"$ig\" .prettierignore; else printf 'dist\\nbuild\\ncoverage\\n.vscode/terminal-gui.temp\\n' > .prettierignore; fi",
      "  npm pkg set scripts.format='prettier . --write' scripts.'format:check'='prettier . --check' 2>/dev/null || node -e 'const fs=require(\"fs\");const f=\"package.json\";const j=JSON.parse(fs.readFileSync(f,\"utf8\"));j.scripts=j.scripts||{};j.scripts.format=\"prettier . --write\";j.scripts[\"format:check\"]=\"prettier . --check\";fs.writeFileSync(f, JSON.stringify(j,null,2));'",
      "}"
    ]
  },
  "VSCodeSnippets": {
    "useMemo (JS)": {
      "scope": "javascript,javascriptreact",
      "prefix": "ume",
      "body": [
        "const ${1:value} = useMemo(() => {",
        "  ${2:// compute}",
        "  return ${3:result};",
        "}, [${4:deps}]);"
      ],
      "description": "useMemo (JS)"
    },
    "useMemo (TS)": {
      "scope": "typescript,typescriptreact",
      "prefix": "ume",
      "body": [
        "const ${1:value} = useMemo<${2:Type}>(() => {",
        "  ${3:// compute}",
        "  return ${4:result};",
        "}, [${5:deps}]);"
      ],
      "description": "useMemo (TS)"
    },
    "useId (JS)": {
      "scope": "javascript,javascriptreact",
      "prefix": "uid",
      "body": [
        "const ${1:id} = useId();"
      ],
      "description": "useId (JS)"
    },
    "useId (TS)": {
      "scope": "typescript,typescriptreact",
      "prefix": "uid",
      "body": [
        "const ${1:id} = useId();"
      ],
      "description": "useId (TS)"
    },
    "useTransition (JS)": {
      "scope": "javascript,javascriptreact",
      "prefix": "utr",
      "body": [
        "const [${1:isPending}, ${2:startTransition}] = useTransition();",
        "${2:startTransition}(() => {",
        "  ${3:// state update}",
        "});"
      ],
      "description": "useTransition (JS)"
    },
    "useTransition (TS)": {
      "scope": "typescript,typescriptreact",
      "prefix": "utr",
      "body": [
        "const [${1:isPending}, ${2:startTransition}] = useTransition();",
        "${2:startTransition}(() => {",
        "  ${3:// state update}",
        "});"
      ],
      "description": "useTransition (TS)"
    },
    "useDeferredValue (JS)": {
      "scope": "javascript,javascriptreact",
      "prefix": "udv",
      "body": [
        "const ${1:deferred} = useDeferredValue(${2:value});"
      ],
      "description": "useDeferredValue (JS)"
    },
    "useDeferredValue (TS)": {
      "scope": "typescript,typescriptreact",
      "prefix": "udv",
      "body": [
        "const ${1:deferred} = useDeferredValue(${2:value});"
      ],
      "description": "useDeferredValue (TS)"
    },
    "useReducer (init, JS)": {
      "scope": "javascript,javascriptreact",
      "prefix": "ured",
      "body": [
        "const ${1:initialState} = ${2:{ count: 0 }};",
        "function ${3:reducer}(state = ${1:initialState}, action) {",
        "  switch (action.type) {",
        "    case '${4:increment}': return { ...state, count: state.count + 1 };",
        "    default: return state;",
        "  }",
        "}",
        "const [${5:state}, ${6:dispatch}] = useReducer(${3:reducer}, ${1:initialState});"
      ],
      "description": "useReducer with initial state (JS)"
    },
    "useReducer (init, TS)": {
      "scope": "typescript,typescriptreact",
      "prefix": "ured",
      "body": [
        "type ${1:State} = { count: number };",
        "type ${2:Action} = { type: 'increment' } | { type: 'decrement' };",
        "function ${3:reducer}(state: ${1:State}, action: ${2:Action}): ${1:State} {",
        "  switch (action.type) {",
        "    case 'increment': return { ...state, count: state.count + 1 };",
        "    case 'decrement': return { ...state, count: state.count - 1 };",
        "    default: return state;",
        "  }",
        "}",
        "const [${4:state}, ${5:dispatch}] = useReducer(${3:reducer}, { count: 0 });"
      ],
      "description": "useReducer with initial state (TS)"
    },
    "useRef (JS)": {
      "scope": "javascript,javascriptreact",
      "prefix": "uref",
      "body": [
        "const ${1:ref} = useRef(null);"
      ],
      "description": "useRef (JS)"
    },
    "useRef (TS)": {
      "scope": "typescript,typescriptreact",
      "prefix": "uref",
      "body": [
        "const ${1:ref} = useRef<${2:HTMLDivElement} | null>(null);"
      ],
      "description": "useRef (typed TS)"
    },
    "React.lazy + Suspense (JS)": {
      "scope": "javascriptreact",
      "prefix": "rlz",
      "body": [
        "import { lazy, Suspense } from 'react';",
        "const ${1:Page} = lazy(() => import('${2:./Page}'));",
        "",
        "<Suspense fallback={<div>Loading...</div>}>",
        "  <${1:Page} />",
        "</Suspense>"
      ],
      "description": "lazy() + Suspense (JSX)"
    },
    "React.lazy + Suspense (TS)": {
      "scope": "typescriptreact",
      "prefix": "rlz",
      "body": [
        "import { lazy, Suspense } from 'react';",
        "const ${1:Page} = lazy(() => import('${2:./Page}'));",
        "",
        "<Suspense fallback={<div>Loading...</div>}>",
        "  <${1:Page} />",
        "</Suspense>"
      ],
      "description": "lazy() + Suspense (TSX)"
    },
    "Suspense boundary (JS)": {
      "scope": "javascriptreact",
      "prefix": "rsus",
      "body": [
        "import { Suspense } from 'react';",
        "<Suspense fallback={<div>Loading...</div>}>",
        "  ${1:/* children */}",
        "</Suspense>"
      ],
      "description": "Suspense boundary (JSX)"
    },
    "Suspense boundary (TS)": {
      "scope": "typescriptreact",
      "prefix": "rsus",
      "body": [
        "import { Suspense } from 'react';",
        "<Suspense fallback={<div>Loading...</div>}>",
        "  ${1:/* children */}",
        "</Suspense>"
      ],
      "description": "Suspense boundary (TSX)"
    },
    "createRoot entry (JS)": {
      "scope": "javascriptreact",
      "prefix": "crt",
      "body": [
        "import { createRoot } from 'react-dom/client';",
        "import ${1:App} from '${2:./App}';",
        "",
        "const root = createRoot(document.getElementById('root'));",
        "root.render(<${1:App} />);"
      ],
      "description": "React 18/19 entry (JS)"
    },
    "createRoot entry (TS)": {
      "scope": "typescriptreact",
      "prefix": "crt",
      "body": [
        "import { createRoot } from 'react-dom/client';",
        "import ${1:App} from '${2:./App}';",
        "",
        "const el = document.getElementById('root');",
        "if (!el) throw new Error('Root element not found');",
        "createRoot(el).render(<${1:App} />);"
      ],
      "description": "React 18/19 entry (TS)"
    },
    "List render .map (JS)": {
      "scope": "javascriptreact",
      "prefix": "rmap",
      "body": [
        "{${1:items}.map((${2:item}) => (",
        "  <li key={${2}.id}>${3}</li>",
        "))}"
      ],
      "description": "Render list with .map (JSX)"
    },
    "List render .map (TS)": {
      "scope": "typescriptreact",
      "prefix": "rmap",
      "body": [
        "{${1:items}.map((${2:item}) => (",
        "  <li key={${2}.id}>${3}</li>",
        "))}"
      ],
      "description": "Render list with .map (TSX)"
    },
    "Conditional className (JS)": {
      "scope": "javascriptreact",
      "prefix": "rcls",
      "body": [
        "className={[${1:'base'}, ${2:cond} && '${3:whenTrue}'].filter(Boolean).join(' ')}"
      ],
      "description": "Conditional className (JSX)"
    },
    "Conditional className (TS)": {
      "scope": "typescriptreact",
      "prefix": "rcls",
      "body": [
        "className={[${1:'base'}, ${2:cond} && '${3:whenTrue}'].filter(Boolean).join(' ')}"
      ],
      "description": "Conditional className (TSX)"
    },
    "Vite env import.meta.env (JS)": {
      "scope": "javascript,javascriptreact",
      "prefix": "ienv",
      "body": [
        "const ${1:apiUrl} = import.meta.env.${2:VITE_API_URL};"
      ],
      "description": "Vite env access (JS)"
    },
    "Vite env import.meta.env (TS)": {
      "scope": "typescript,typescriptreact",
      "prefix": "ienv",
      "body": [
        "const ${1:apiUrl}: string = import.meta.env.${2:VITE_API_URL};"
      ],
      "description": "Vite env access (TS)"
    },
    "Fetch in useEffect (AbortController, JS)": {
      "scope": "javascript,javascriptreact",
      "prefix": "ufetch",
      "body": [
        "useEffect(() => {",
        "  const ac = new AbortController();",
        "  (async () => {",
        "    try {",
        "      const r = await fetch('${1:/api}', { signal: ac.signal });",
        "      if (!r.ok) throw new Error(String(r.status));",
        "      const data = await r.json();",
        "      ${2:setState}(data);",
        "    } catch (e) {",
        "      if (e.name !== 'AbortError') console.error(e);",
        "    }",
        "  })();",
        "  return () => ac.abort();",
        "}, [${3:deps}]);"
      ],
      "description": "Fetch in useEffect with AbortController (JS)"
    },
    "Fetch in useEffect (AbortController, TS)": {
      "scope": "typescript,typescriptreact",
      "prefix": "ufetch",
      "body": [
        "useEffect(() => {",
        "  const ac = new AbortController();",
        "  (async () => {",
        "    try {",
        "      const r = await fetch('${1:/api}', { signal: ac.signal });",
        "      if (!r.ok) throw new Error(String(r.status));",
        "      const data: ${2:any} = await r.json();",
        "      ${3:setState}(data);",
        "    } catch (e) {",
        "      if ((e as any).name !== 'AbortError') console.error(e);",
        "    }",
        "  })();",
        "  return () => ac.abort();",
        "}, [${4:deps}]);"
      ],
      "description": "Fetch in useEffect with AbortController (TS)"
    },
    "CSS Module import (JS/TS)": {
      "scope": "javascript,javascriptreact,typescript,typescriptreact",
      "prefix": "icssm",
      "body": [
        "import styles from '${1:./File.module.css}';",
        "<div className={styles.${2:box}}>${3}</div>"
      ],
      "description": "Import & use CSS Module"
    }
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
      "command": "cd _[Select a folder(Select a parent folder for the project (press ENTER))]_ && _[bashScript]_ && tg_node_create_project \"_[projectName]_\" \"_[entryPoint]_\" _[indexFile]_.file _[envFile]_.file _[gitignore]_.file _[prettierIgnore]_.file _[prettierRc]_.file _[eslintConfig]_.file",
      "group": "🟩 Node.js",
      "inputs": {
        "Enter a project name; projectName; false; false": "",
        "Enter an entry-point file name; entryPoint; false; false": "index"
      },
      "snippets": {
        "indexFile": [
          "import 'dotenv/config';",
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
          "  console.log(`Server running at http://localhost:${PORT}`);",
          "});"
        ],
        "envFile": [
          "PORT=3000"
        ],
        "gitignore": [
          "node_modules/",
          "dist/",
          "logs/",
          ".vscode/",
          ".idea/"
        ],
        "prettierIgnore": [
          "node_modules",
          "dist",
          "logs",
          ".vscode",
          "eslint.config.js",
          ".prettierrc",
          "README.md"
        ],
        "prettierRc": [
          "{",
          "  \"printWidth\": 80,",
          "  \"useTabs\": false,",
          "  \"tabWidth\": 4,",
          "  \"singleQuote\": true,",
          "  \"trailingComma\": \"es5\",",
          "  \"semi\": true",
          "}"
        ],
        "eslintConfig": [
          "export default [",
          "  {",
          "    ignores: [",
          "      'eslint.config.js',",
          "      'node_modules',",
          "      'dist',",
          "      'logs',",
          "      '.vscode',",
          "    ],",
          "  },",
          "  {",
          "    files: ['**/*.js'],",
          "    languageOptions: {",
          "      ecmaVersion: 'latest',",
          "      sourceType: 'module',",
          "    },",
          "    rules: {",
          "      indent: ['error', 4],",
          "      quotes: ['error', 'single'],",
          "      semi: ['error', 'always'],",
          "      'linebreak-style': ['error', 'unix'],",
          "    },",
          "  },",
          "];"
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
      "icon": "▶;Run Node.js Server",
      "icon2": "▢;Stop Node.js Server",
      "group": "🟩 Node.js",
      "settings": {
        "terminalName": "Node.js Server",
        "quickButton": "statusBar",
        "showWhenEmptyWorkspace": "fullWorkspace"
      }
    },
    "Service": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_make_service \"_[name]_\" _[serviceSnippet]_.file",
      "group": "🟩 Node.js",
      "inputs": {
        "Service name (e.g. user); name; false; false": ""
      },
      "snippets": {
        "serviceSnippet": [
          "export const __CAMEL__Service = {",
          "    async create__PASCAL__() {},",
          "};"
        ]
      },
      "settings": {
        "contextMenu": true,
        "revealConsole": true
      }
    },
    "Route": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_make_route \"_[name]_\" _[routeSnippet]_.file",
      "group": "🟩 Node.js",
      "inputs": {
        "Route name (e.g. profile); name; false; false": ""
      },
      "snippets": {
        "routeSnippet": [
          "import { Router } from 'express';",
          "",
          "export const __CAMEL__Router = Router();",
          "",
          "__CAMEL__Router.get('/__BASE__', async (req, res) => {",
          "",
          "});"
        ]
      },
      "settings": {
        "contextMenu": true,
        "revealConsole": true
      }
    },
    "Middleware": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_make_middleware \"_[name]_\" \"_[kind]_\" _[mwStandard]_.file _[mwError]_.file _[mwFactory]_.file",
      "group": "🟩 Node.js",
      "inputs": {
        "Middleware name (e.g. auth); name; false; false": "",
        "Choose middleware type; kind": {
          "Standard": "standard",
          "Error handler": "error",
          "Factory (with options)": "factory"
        }
      },
      "snippets": {
        "mwStandard": [
          "export const __CAMEL__Middleware = (req, res, next) => {",
          "    next();",
          "};"
        ],
        "mwError": [
          "export const __CAMEL__Middleware = (err, req, res, next) => {",
          "    res.status(500).json({ error: 'Internal Server Error' });",
          "};"
        ],
        "mwFactory": [
          "export const __CAMEL__Middleware = (options = {}) => (req, res, next) => {",
          "    next();",
          "};"
        ]
      },
      "settings": {
        "contextMenu": true,
        "revealConsole": true
      }
    },
    "🛠️ Install NPM Packages": {
      "command": "cd _[projectPath]_ && _[packages]_",
      "group": "🟩 Node.js",
      "inputs": {
        "Choose the package; packages; true": {
          "bcrypt": "npm i bcrypt",
          "jsonwebtoken": "npm i jsonwebtoken",
          "nodemailer": "npm i nodemailer",
          "cors": "npm i cors",
          "socket.io": "npm i socket.io",
          "helmet": "npm i helmet",
          "express-rate-limit": "npm i express-rate-limit",
          "Swagger (docs)": "npm i swagger-ui-express swagger-jsdoc",
          "mongoose": "npm i mongoose",
          "Prisma (ORM) + init (PostgreSQL)": "npm i @prisma/client && npm i -D prisma && npx prisma init --datasource-provider postgresql",
          "Vitest (unit tests)": "npm i -D vitest",
          "Supertest (HTTP tests)": "npm i -D supertest",
        },
      },
      "settings": {
        "revealConsole": true
      }
    }
  },
  "scripts": {
    "bash": [
      "# --- Node.Js helpers ---",
      "tg_slugify() {",
      "  # to-lower; replace non-alnum with '-'; collapse repeats; trim dashes",
      "  local s=\"$*\"",
      "  s=$(echo \"$s\" | tr '[:upper:]' '[:lower:]' | sed -E 's/[^a-z0-9]+/-/g; s/-+/-/g; s/^-|-$//g')",
      "  printf '%s' \"$s\"",
      "}",
      "",
      "tg_to_camel() {",
      "  # slug -> camelCase",
      "  local slug=\"$1\"",
      "  local snake=${slug//-/_}",
      "  echo \"$snake\" | awk -F'_' '{",
      "    for (i=1; i<=NF; i++) {",
      "      w=$i;",
      "      if (i==1) printf(\"%s\", w);",
      "      else printf(\"%s%s\", toupper(substr(w,1,1)), substr(w,2));",
      "    }",
      "  }'",
      "}",
      "",
      "tg_to_pascal() {",
      "  local camel=\"$1\"",
      "  echo \"$camel\" | awk '{print toupper(substr($0,1,1)) substr($0,2)}'",
      "}",
      "",
      "tg_scaffold_from_snip() {",
      "  # Usage: tg_scaffold_from_snip <snippet-file> <name> <suffix>",
      "  local snip=\"$1\"",
      "  local rawname=\"$2\"",
      "  local suffix=\"$3\"",
      "  local base; base=$(tg_slugify \"$rawname\")",
      "  local camel; camel=$(tg_to_camel \"$base\")",
      "  local pascal; pascal=$(tg_to_pascal \"$camel\")",
      "  local out=\"${base}.${suffix}\"",
      "  local tmp=\"$out.tmp\"",
      "  cp \"$snip\" \"$tmp\"",
      "  # Replace placeholders",
      "  sed -i \"s/__CAMEL__/${camel}/g; s/__PASCAL__/${pascal}/g; s/__BASE__/${base}/g\" \"$tmp\"",
      "  mv \"$tmp\" \"$out\"",
      "  code -r \"$out\"",
      "}",
      "",
      "tg_make_service() {",
      "  # tg_make_service <name> <snippet-file>",
      "  local name=\"$1\" snip=\"$2\"",
      "  tg_scaffold_from_snip \"$snip\" \"$name\" \"service.js\"",
      "}",
      "",
      "tg_make_route() {",
      "  # tg_make_route <name> <snippet-file>",
      "  local name=\"$1\" snip=\"$2\"",
      "  tg_scaffold_from_snip \"$snip\" \"$name\" \"routes.js\"",
      "}",
      "",
      "# Scaffold a middleware file from name + kind",
      "tg_make_middleware() {",
      "  # tg_make_middleware <name> <kind> <snipStd> <snipErr> <snipFac>",
      "  local name=\"$1\" kind=\"$2\" snStd=\"$3\" snErr=\"$4\" snFac=\"$5\"",
      "  local base; base=$(tg_slugify \"$name\")",
      "  local camel; camel=$(tg_to_camel \"$base\")",
      "  local pascal; pascal=$(tg_to_pascal \"$camel\")",
      "  local out=\"${base}.middleware.js\"",
      "  local src",
      "  case \"$kind\" in",
      "    standard) src=\"$snStd\" ;;",
      "    error)    src=\"$snErr\" ;;",
      "    factory)  src=\"$snFac\" ;;",
      "    *)        src=\"$snStd\" ;;",
      "  esac",
      "  local tmp=\"$out.tmp\"",
      "  cp \"$src\" \"$tmp\"",
      "  sed -i \"s/__CAMEL__/${camel}/g; s/__PASCAL__/${pascal}/g; s/__BASE__/${base}/g\" \"$tmp\"",
      "  mv \"$tmp\" \"$out\"",
      "  code -r \"$out\"",
      "}",
      "",
      "tg_node_create_project() {",
      "  local proj=\"$1\"",
      "  local entry=\"$2\"",
      "  local sn_index=\"$3\"",
      "  local sn_env=\"$4\"",
      "  local sn_gitignore=\"$5\"",
      "  local sn_prettier_ignore=\"$6\"",
      "  local sn_prettier_rc=\"$7\"",
      "  local sn_eslint=\"$8\"",
      "",
      "  # Guard: existing folder",
      "  if [ -d \"$proj\" ]; then",
      "    echo \"TERMINAL_GUI_MSG('$proj' folder already exists. Please choose another project name.)\"",
      "    return 0",
      "  fi",
      "",
      "  echo \"▶ Creating project folder: $proj\"",
      "  mkdir -p \"$proj\" || { echo \"TERMINAL_GUI_MSG(Failed to create '$proj' folder.)\"; return 0; }",
      "  cd \"$proj\" || { echo \"TERMINAL_GUI_MSG(Failed to enter '$proj' folder.)\"; return 0; }",
      "",
      "  echo \"▶ Initializing package.json\"",
      "  npm init -y",
      "",
      "  echo \"▶ Setting package metadata & scripts\"",
      "  npm pkg set type=\"module\" \\",
      "    main=\"src/${entry}.js\" \\",
      "    scripts.start=\"node src/${entry}.js\" \\",
      "    scripts.watch=\"node --watch src/${entry}.js\" \\",
      "    scripts.dev=\"node --inspect --watch src/${entry}.js\" \\",
      "    scripts.lint=\"eslint .\" \\",
      "    scripts.'prettier:check'=\"prettier --check .\" \\",
      "    scripts.'prettier:write'=\"prettier --write .\" \\",
      "    scripts.build=\"npm run lint && npm run prettier:check\"",
      "",
      "  echo \"▶ Installing runtime deps: express dotenv winston\"",
      "  npm i express dotenv winston",
      "",
      "  echo \"▶ Installing dev deps: eslint eslint-plugin-prettier husky lint-staged prettier\"",
      "  npm i -D eslint eslint-plugin-prettier husky lint-staged prettier",
      "",
      "  echo \"▶ Writing files\"",
      "  mkdir -p src",
      "  cp \"$sn_index\" \"src/${entry}.js\"",
      "  cp \"$sn_env\" \".env\"",
      "  cp \"$sn_gitignore\" \".gitignore\"",
      "  cp \"$sn_prettier_ignore\" \".prettierignore\"",
      "  cp \"$sn_prettier_rc\" \".prettierrc\"",
      "  cp \"$sn_eslint\" \"eslint.config.js\"",
      "",
      "  echo \"▶ Cleaning unwanted scripts (if present)\"",
      "  npm pkg delete scripts.test || true",
      "  npm pkg delete scripts.'backup:users' || true",
      "",
      "  echo \"▶ Initializing Git (if available)\"",
      "  if command -v git >/dev/null 2>&1; then",
      "    if [ -d \".git\" ]; then",
      "      echo \"... Git already initialized, skipping\"",
      "    else",
      "      # use default branch from user config; keep it simple",
      "      git init",
      "      echo \"... Git repository initialized\"",
      "    fi",
      "  else",
      "    echo \"... Git not found on PATH, skipping git init\"",
      "  fi",
      "",
      "  echo \"✅ Project ready → opening in VS Code\"",
      "  code -r .",
      "}",
      ""
    ]
  }
},
```
</details>

<details>
<summary>🐦 NestJS</summary>

```json
// settings.json
"TerminalGui.config": {
  "commands": {
    "Create NestJS Project": {
      "command": "cd _[Select a folder(Select a parent folder for the project)]_ && : _[projectName]_.value && : _[version]_.value && _[projectOptions]_ && ( SKIP=''; [[ \"${TG_GIT_MODE:-terminal}\" = \"terminal\" ]] && SKIP='--skip-git'; npx --yes @nestjs/cli@_[version]_ new \"_[projectName]_\" --package-manager npm --language ts --strict $SKIP ) && cd \"_[projectName]_\" && ( [[ \"${TG_GIT_MODE:-terminal}\" = \"terminal\" ]] && git init || : ) && _[projectOptions]_ && ( [[ -f .gitignore ]] || cp _[gitignore]_.file .gitignore ) && ( [[ ${TG_DO_DEBUG:-1} -eq 1 ]] && mkdir -p .vscode && cp _[launchJson]_.file .vscode/launch.json || : ) && ( [[ ${TG_DO_PRETTIER:-0} -eq 1 ]] && _[bashScript]_ && tg_setup_prettier _[prettierRc]_.file _[prettierIgnore]_.file || : ) && code -r .",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter a project name; projectName; false; false": "",
        "Choose the NestJS CLI version (e.g., 10.3.2); version; false": "latest",
        "Project options; projectOptions; true; true": {
          "connectItems": "&&",
          "Use custom Prettier config _[toggle]_ Use Nest CLI default Prettier": "TG_DO_PRETTIER=1 _[toggle]_ TG_DO_PRETTIER=0",
          // Choose how to initialize Git: 
          // - NestJS → lets Nest CLI handle git init (use this if the built-in bug is fixed)
          // - Terminal GUI → adds --skip-git and runs `git init` manually (works around VS Code Git Bash issues; sometimes the built-in terminal may not init the repo)
          "Git (NestJS) _[toggle]_ Git (Terminal GUI)": "TG_GIT_MODE=nestjs _[toggle]_ TG_GIT_MODE=terminal",
          "VS Code debugging (launch.json) _[toggle]_ Skip debugging config": "TG_DO_DEBUG=1 _[toggle]_ TG_DO_DEBUG=0"
        }
      },
      "snippets": {
        "prettierRc": [
          "{",
          "  \"singleQuote\": true,",
          "  \"trailingComma\": \"all\",",
          "  \"endOfLine\": \"auto\"",
          "}"
        ],
        "prettierIgnore": [
          "dist",
          "node_modules",
          ".vscode"
        ],
        "gitignore": [
          "# log",
          "logs",
          "*.log",
          "npm-debug.log*",
          "yarn-debug.log*",
          "yarn-error.log*",
          "lerna-debug.log*",
          "pnpm-debug.log*",
          "",
          "# Diagnostic reports",
          "report.[0-9]*.[0-9]*.[0-9]*.[0-9]*.json",
          "",
          "# Runtime data",
          "pids",
          "*.pid",
          "*.seed",
          "*.pid.lock",
          "",
          "# Coverage",
          "coverage",
          "*.lcov",
          ".nyc_output",
          "",
          "# Compiled addons",
          "build/Release",
          "",
          "# Dependencies",
          "node_modules/",
          "jspm_packages/",
          "web_modules/",
          "",
          "# TypeScript",
          "*.tsbuildinfo",
          "",
          "# Caches",
          ".npm",
          ".eslintcache",
          ".stylelintcache",
          ".cache",
          ".parcel-cache",
          ".vite/",
          ".svelte-kit/",
          ".docusaurus",
          ".serverless/",
          ".fusebox/",
          ".dynamodb/",
          ".firebase/",
          ".tern-port",
          ".vscode-test",
          "",
          "# Yarn v3 (PnP)",
          ".pnp.*",
          ".yarn/*",
          "!.yarn/patches",
          "!.yarn/plugins",
          "!.yarn/releases",
          "!.yarn/sdks",
          "!.yarn/versions",
          "",
          "# Build outputs",
          "dist/",
          "out/",
          ".next",
          ".nuxt",
          ".output",
          "",
          "# dotenv files",
          ".env",
          ".env.*",
          "!.env.example",
          "",
          "# IDEs",
          ".idea/",
          "",
          "# VS Code",
          ".vscode/*",
          "!.vscode/settings.json",
          "!.vscode/tasks.json",
          "!.vscode/launch.json",
          "!.vscode/extensions.json",
          "!.vscode/*.code-snippets",
          "",
          "# OS",
          ".DS_Store",
          "Thumbs.db",
          "ehthumbs.db",
          "Desktop.ini",
          "",
          ""
        ],
        "launchJson": [
          "{",
          "  \"version\": \"0.2.0\",",
          "  \"configurations\": [",
          "    {",
          "      \"type\": \"node\",",
          "      \"request\": \"launch\",",
          "      \"name\": \"Debug NestJS App\",",
          "      \"runtimeExecutable\": \"npm\",",
          "      \"runtimeArgs\": [",
          "        \"run\",",
          "        \"start:debug\"",
          "      ]",
          "    }",
          "  ]",
          "}"
        ]
      },
      "settings": {
        "terminalName": "NestJS Project Creation",
        "showWhenEmptyWorkspace": "emptyWorkspace",
        "revealConsole": true
      }
    },
    "Add Prisma Migrations": {
      "command": "cd _[projectPath]_ && npx prisma migrate dev --name _[migrationName]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter a migration name; migrationName": "",
      },
      "settings": {
        "revealConsole": true
      }
    },
    "Run Prisma Studio": {
      "command": "cd _[projectPath]_ && npx prisma studio --browser none",
      "group": "🐦 NestJS",
      "icon": "⚫;Run Prisma Studio",
      "icon2": "🌎;Stop Prisma Studio",
      "settings": {
        "terminalName": "Prisma Studio",
        "quickButton": "statusBar",
      }
    },
    "NestJS Server": {
      "command": "npm run start:dev",
      "group": "🐦 NestJS",
      "icon": "▶;Run NestJS Server",
      "icon2": "▢;Stop NestJS Server",
      "settings": {
        "terminalName": "NestJS Server",
        "quickButton": "statusBar",
      }
    },
    "Resource": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open resource _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the resource name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "No Spec _[toggle]_ With Spec": "--no-spec _[toggle]_ \"\"",
          "Into Folder _[toggle]_ Flat": "\"\" _[toggle]_ --flat"
        }
      },
      "settings": {
        "contextMenu": true,
        "revealConsole": true
      }
    },
    "Controller": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open controller _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the controller name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "No Spec _[toggle]_ With Spec": "--no-spec _[toggle]_ \" \"",
          "Into Folder _[toggle]_ Flat": "\"\" _[toggle]_ --flat"
        }
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Class": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open class _[name]_ --no-spec --flat",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the class name; name; false; false": "",
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Class - DTO": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open class _[name]_.dto --no-spec --flat",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the class name; name; false; false": "",
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Class - Model": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open class _[name]_.model --no-spec --flat",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the class name; name; false; false": "",
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Interface": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open interface _[name]_ --no-spec --flat",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the interface name; name; false; false": ""
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Middleware": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open middleware _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the middleware name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "Flat _[toggle]_ Not Flat": "--flat _[toggle]_ \" \"",
          "No Spec _[toggle]_ With Spec": "--no-spec _[toggle]_ \" \""
        }
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Service": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open service _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the service name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "Flat _[toggle]_ Not Flat": "--flat _[toggle]_ \" \"",
          "No Spec _[toggle]_ With Spec": "--no-spec _[toggle]_ \" \""
        }
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Filter": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open filter _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the filter name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "Flat _[toggle]_ Not Flat": "--flat _[toggle]_ \" \"",
          "No Spec _[toggle]_ With Spec": "--no-spec _[toggle]_ \" \""
        }
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Guard": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open guard _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the guard name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "Flat _[toggle]_ Not Flat": "--flat _[toggle]_ \" \"",
          "No Spec _[toggle]_ With Spec": "--no-spec _[toggle]_ \" \""
        }
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Pipe": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open pipe _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the pipe name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "Flat _[toggle]_ Not Flat": "--flat _[toggle]_ \" \"",
          "No Spec _[toggle]_ With Spec": "--no-spec _[toggle]_ \" \""
        }
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Module": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open module _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the module name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "Into Folder _[toggle]_ Flat": "\" \" _[toggle]_ --flat"
        }
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Provider": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open provider _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the provider name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "No Spec _[toggle]_ With Spec": "--no-spec _[toggle]_ \" \"",
          "Into Folder _[toggle]_ Flat": "\" \" _[toggle]_ --flat",
        }
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Resolver": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open resolver _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the resolver name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "No Spec _[toggle]_ With Spec": "--no-spec _[toggle]_ \" \"",
          "Into Folder _[toggle]_ Flat": "\" \" _[toggle]_ --flat",
        }
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Interceptor": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open interceptor _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the interceptor name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "No Spec _[toggle]_ With Spec": "--no-spec _[toggle]_ \" \"",
          "Into Folder _[toggle]_ Flat": "\" \" _[toggle]_ --flat",
        }
      },
      "settings": {
        "contextMenu": true
      }
    },
    "Library": {
      "command": "cd _[projectPath]_ && _[bashScript]_ && tg_nest_generate_and_open library _[name]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the library name; name; false; false": ""
      },
      "settings": {
        "revealConsole": true
      }
    },
    "Gateway": {
      "command": "cd _[folderPath]_ && _[bashScript]_ && tg_nest_generate_and_open gateway _[name]_ _[options]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Enter the gateway name; name; false; false": "",
        "Options; options; true; true; save": {
          "connectItems": " ",
          "No Spec _[toggle]_ With Spec": "--no-spec _[toggle]_ \" \"",
          "Into Folder _[toggle]_ Flat": "\" \" _[toggle]_ --flat",
        }
      },
      "settings": {
        "contextMenu": true
      }
    },
    "🛠️ Add NestJS Libraries": {
      "command": "cd _[projectPath]_ && _[packages]_",
      "group": "🐦 NestJS",
      "inputs": {
        "Choose libraries to install; packages; true; true": {
          "Config": "npm i @nestjs/config",
          "TS Config Paths": "npm i -D tsconfig-paths",
          "Swagger": "npm i @nestjs/swagger swagger-ui-express",
          "Validator & Transformer": "npm i class-validator class-transformer",
          "Passport": "npm i @nestjs/passport passport passport-local",
          "JWT (Passport strategy)": "npm i @nestjs/passport passport passport-jwt @types/passport-jwt @types/passport",
          "Helmet": "npm i helmet",
          "Mongoose": "npm i @nestjs/mongoose mongoose",
          "TypeORM (PostgreSQL)": "npm i @nestjs/typeorm typeorm pg && npm i -D ts-node @types/node",
          "Prisma (PostgreSQL)": "npm i -D prisma && npm i @prisma/client && npx prisma init --datasource-provider postgresql",
          "Prisma (SQLite)": "npm i -D prisma && npm i @prisma/client && npx prisma init --datasource-provider sqlite",
          "GraphQL (Apollo)": "npm i @nestjs/graphql @nestjs/apollo graphql @apollo/server",
          "CQRS": "npm i @nestjs/cqrs",
          "Rate Limiting (Throttler)": "npm i @nestjs/throttler",
          "Scheduling (Cron)": "npm i @nestjs/schedule",
          "Health Checks (Terminus)": "npm i @nestjs/terminus",
          "WebSockets (Socket.IO)": "npm i @nestjs/websockets @nestjs/platform-socket.io",
          "WebSockets (GraphQL)": "npm i @nestjs/websockets @nestjs/platform-socket.io @nestjs/graphql @nestjs/apollo @apollo/server graphql",
          "Event Emitter": "npm i @nestjs/event-emitter",
          "Serve Static": "npm i @nestjs/serve-static",
          "Cache (in-memory)": "npm i @nestjs/cache-manager cache-manager",
          "Cache (Redis store)": "npm i @nestjs/cache-manager cache-manager cache-manager-ioredis-yet ioredis",
          "Queues (Bull + Redis)": "npm i @nestjs/bull bull ioredis",
          "Logging (Pino)": "npm i nestjs-pino pino",
          "Logging (Winston)": "npm i nest-winston winston",
          "Internationalization (i18n)": "npm i nestjs-i18n",
          "Metrics (Prometheus)": "npm i @willsoto/nestjs-prometheus prom-client",
          "Microservices: NestJS": "npm i --save @nestjs/microservices",
          "Microservices: Kafka": "npm i kafkajs",
          "Microservices: NATS": "npm i nats",
          "Microservices: RabbitMQ": "npm i amqplib",
          "Microservices: Redis": "npm i ioredis",
          "Testing": "npm i -D @nestjs/testing supertest"
        }
      },
      "settings": {
        "revealConsole": true
      }
    }
  },
  "scripts": {
    "bash": [
      "tg_setup_prettier() {",
      "  local rc=\"$1\" ig=\"$2\"",
      "  npm i -D prettier || return 0",
      "  cp \"$rc\" .prettierrc",
      "  cp \"$ig\" .prettierignore",
      "  npm pkg set scripts.format='prettier --write \"src/**/*.ts\" \"test/**/*.ts\"'",
      "}",
      "",
      "tg_strip_ansi_cr() {",
      "  tr -d '\\r' | sed -E 's/\\x1B\\[[0-9;]*[[:alpha:]]//g'",
      "}",
      "",
      "tg_cache_dir() {",
      "  local script=\"${BASH_SOURCE[0]}\"",
      "  local dir",
      "  dir=\"$(cd \"$(dirname \"$script\")\" && pwd)\"",
      "  printf '%s\\n' \"$dir\"",
      "}",
      "",
      "tg_nest_generate_and_open() {",
      "  local type=\"$1\"",
      "  local name=\"$2\"",
      "  shift 2",
      "  local options=\"$@\"",
      "",
      "  local log_dir=\"$(tg_cache_dir)\"",
      "  local log_file=\"$log_dir/nest_last.log\"",
      "  mkdir -p \"$log_dir\"",
      "",
      "  if [[ \"$type\" == \"resource\" ]]; then",
      "    nest g \"$type\" \"$name\" $options",
      "    : > \"$log_file\"",
      "  else",
      "    nest g \"$type\" \"$name\" $options 2>&1 | tee /dev/tty | tg_strip_ansi_cr > \"$log_file\"",
      "  fi",
      "",
      "  local clean created_lines",
      "  clean=$(cat \"$log_file\" 2>/dev/null)",
      "  created_lines=$(printf '%s\\n' \"$clean\" | grep -E 'CREATE[[:space:]]+')",
      "  local created=()",
      "  while IFS= read -r line; do",
      "    [[ -z \"$line\" ]] && continue",
      "    local p",
      "    p=$(sed -E 's/.*CREATE[[:space:]]+([^[:space:]]+).*/\\1/' <<< \"$line\")",
      "    [[ -n \"$p\" ]] && created+=(\"${p//\\\\//}\")",
      "  done <<< \"$created_lines\"",
      "",
      "  local picks=() p",
      "  for p in \"${created[@]}\"; do",
      "    [[ \"$p\" == *.spec.ts ]] && continue",
      "    picks+=(\"$p\")",
      "  done",
      "",
      "  local pattern file_to_open=\"\"",
      "  case \"$type\" in",
      "    resource)    pattern='\\.service\\.ts$' ;;",
      "    library)     pattern='^libs/.*/src/index\\.ts$' ;;",
      "    interface)   pattern='\\.interface\\.ts$' ;;",
      "    controller)  pattern='\\.controller\\.ts$' ;;",
      "    service)     pattern='\\.service\\.ts$' ;;",
      "    guard)       pattern='\\.guard\\.ts$' ;;",
      "    pipe)        pattern='\\.pipe\\.ts$' ;;",
      "    filter)      pattern='\\.filter\\.ts$' ;;",
      "    interceptor) pattern='\\.interceptor\\.ts$' ;;",
      "    resolver)    pattern='\\.resolver\\.ts$' ;;",
      "    gateway)     pattern='\\.gateway\\.ts$' ;;",
      "    module)      pattern='\\.module\\.ts$' ;;",
      "    middleware)  pattern='\\.middleware\\.ts$' ;;",
      "    provider)    pattern='\\.provider\\.ts$' ;;",
      "    class)       pattern='\\.ts$' ;;",
      "    *)           pattern='\\.ts$' ;;",
      "  esac",
      "",
      "  for p in \"${picks[@]}\"; do",
      "    if [[ \"$type\" == \"class\" ]]; then",
      "      [[ \"$p\" == *.controller.ts || \"$p\" == *.service.ts || \"$p\" == *.guard.ts || \"$p\" == *.pipe.ts || \\",
      "         \"$p\" == *.filter.ts || \"$p\" == *.interceptor.ts || \"$p\" == *.resolver.ts || \"$p\" == *.gateway.ts || \\",
      "         \"$p\" == *.module.ts || \"$p\" == *.middleware.ts || \"$p\" == *.interface.ts || \"$p\" == *.provider.ts ]] && continue",
      "      [[ \"$p\" == *.ts ]] && file_to_open=\"$p\"",
      "    else",
      "      [[ \"$p\" =~ $pattern ]] && file_to_open=\"$p\"",
      "    fi",
      "  done",
      "",
      "  if [[ -z \"$file_to_open\" && ${#picks[@]} -gt 0 ]]; then",
      "    file_to_open=\"${picks[${#picks[@]}-1]}\"",
      "  fi",
      "",
      "  if [[ -z \"$file_to_open\" && \"$type\" == \"resource\" ]]; then",
      "    local flat=0",
      "    [[ \"$options\" == *\"--flat\"* ]] && flat=1",
      "    local in_src=0",
      "    [[ \"${PWD##*/}\" == \"src\" ]] && in_src=1",
      "",
      "    declare -a candidates=()",
      "    if [[ $flat -eq 1 ]]; then",
      "      candidates+=(\"${name}.service.ts\" \"${name}.controller.ts\" \"${name}.module.ts\")",
      "      if [[ $in_src -eq 0 ]]; then",
      "        candidates+=(\"src/${name}.service.ts\" \"src/${name}.controller.ts\" \"src/${name}.module.ts\")",
      "      fi",
      "    else",
      "      candidates+=(\"${name}/${name}.service.ts\" \"${name}/${name}.controller.ts\" \"${name}/${name}.module.ts\")",
      "      if [[ $in_src -eq 0 ]]; then",
      "        candidates+=(\"src/${name}/${name}.service.ts\" \"src/${name}/${name}.controller.ts\" \"src/${name}/${name}.module.ts\")",
      "      fi",
      "    fi",
      "",
      "    for f in \"${candidates[@]}\"; do",
      "      if [[ -f \"$f\" ]]; then",
      "        file_to_open=\"$f\"",
      "        break",
      "      fi",
      "    done",
      "  fi",
      "",
      "  if [[ -n \"$file_to_open\" && -f \"$file_to_open\" ]]; then",
      "    code -r \"$file_to_open\"",
      "  else",
      "    echo \"TERMINAL_GUI_MSG(Warning: Could not find the generated file to open it: ${file_to_open:-unknown})\"",
      "  fi",
      "}",
      ""
    ]
  },
  "VSCodeSnippets": {
    "NestJS Controller": {
      "scope": "typescript",
      "prefix": "n-controller",
      "body": [
        "import { Controller, Get } from '@nestjs/common';",
        "",
        "@Controller('${1:path}')",
        "export class ${2:My}Controller {",
        "\tconstructor(private readonly ${3:my}Service: ${2:My}Service) {}",
        "",
        "\t@Get()",
        "\tfindAll() {",
        "\t\treturn this.${3:my}Service.findAll();",
        "\t}",
        "}"
      ],
      "description": "NestJS Controller"
    },
    "NestJS Service": {
      "scope": "typescript",
      "prefix": "n-service",
      "body": [
        "import { Injectable } from '@nestjs/common';",
        "",
        "@Injectable()",
        "export class ${1:My}Service {",
        "\tfindAll() {",
        "\t\treturn `This action returns all items`;",
        "\t}",
        "}"
      ],
      "description": "NestJS Service"
    },
    "NestJS Module": {
      "scope": "typescript",
      "prefix": "n-module",
      "body": [
        "import { Module } from '@nestjs/common';",
        "",
        "@Module({",
        "\tcontrollers: [${1:My}Controller],",
        "\tproviders: [${2:My}Service],",
        "})",
        "export class ${3:My}Module {}"
      ],
      "description": "NestJS Module"
    },
    "NestJS DTO": {
      "scope": "typescript",
      "prefix": "n-dto",
      "body": [
        "import { IsString, IsInt } from 'class-validator';",
        "",
        "export class Create${1:Item}Dto {",
        "\t@IsString()",
        "\treadonly name: string;",
        "",
        "\t@IsInt()",
        "\treadonly age: number;",
        "}"
      ],
      "description": "NestJS Data Transfer Object (DTO)"
    },
    "NestJS GET Route": {
      "scope": "typescript",
      "prefix": "n-get",
      "body": [
        "@Get('${1:id}')",
        "findOne(@Param('${1:id}') id: string) {",
        "\treturn this.${2:my}Service.findOne(+id);",
        "}"
      ],
      "description": "NestJS GET route handler"
    },
    "NestJS POST Route": {
      "scope": "typescript",
      "prefix": "n-post",
      "body": [
        "@Post()",
        "create(@Body() create${1:Item}Dto: Create${1:Item}Dto) {",
        "\treturn this.${2:my}Service.create(create${1:Item}Dto);",
        "}"
      ],
      "description": "NestJS POST route handler"
    }
  }
},
```
</details>

### Known Issues
- Placeholder IntelliSense may not activate until `VS Code` is restarted after installing or reactivating the extension.

## Support and Feedback

Please feel free to report any issues or suggestions. Check out the [GitHub repository](https://github.com/BachiMjavanadze/terminal-gui2) for more details.

#### [Change Log](CHANGELOG.md)

## License

This project is licensed under the [EULA License](https://github.com/BachiMjavanadze/terminal-gui2/blob/main/LICENSE).
