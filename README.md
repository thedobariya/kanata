# Kanata Home Row Mods Configuration

A Kanata keyboard configuration that implements home row modifiers with tap-hold functionality for enhanced typing efficiency and ergonomics.

## 🚀 Features

This configuration provides:

- **Home Row Modifiers**: Transform your home row keys into modifiers when held
- **Copy/Cut/Paste Integration**: Built-in clipboard operations with tap-hold behavior
- **Minimal Key Override**: Only modifies specific keys, letting all others pass through normally

## ⌨️ Key Mappings

### Home Row Modifiers (QWERTY Layout)

| Key | Tap Action | Hold Action    |
| --- | ---------- | -------------- |
| `a` | Types "a"  | Left Alt       |
| `s` | Types "s"  | Left Meta/Win  |
| `d` | Types "d"  | Left Ctrl      |
| `f` | Types "f"  | Left Shift     |
| `j` | Types "j"  | Right Shift    |
| `k` | Types "k"  | Right Ctrl     |
| `l` | Types "l"  | Right Meta/Win |
| `h` | Types "h"  | Right Alt      |

### Clipboard Operations

| Key | Tap Action | Hold Action    |
| --- | ---------- | -------------- |
| `c` | Types "c"  | Copy (Ctrl+C)  |
| `x` | Types "x"  | Cut (Ctrl+X)   |
| `v` | Types "v"  | Paste (Ctrl+V) |

## 📋 Prerequisites

1. **Kanata**: Download the latest version from [GitHub Releases](https://github.com/jtroo/kanata/releases)
2. **Administrator Privileges**: Kanata requires admin rights on Windows to intercept keyboard input
3. **Windows 10/11**: This configuration is designed for Windows systems

## 🛠️ Installation

1. **Download Kanata**:

   ```bash
   # Download kanata.exe from the releases page
   # Place it in your desired directory (e.g., C:\kanata\)
   ```

2. **Clone or Download This Configuration**:

   ```bash
   git clone <your-repo-url>
   # Or download the german_iso_qwerty.kbd file directly
   ```

3. **Place Files**: Ensure both `kanata.exe` and `german_iso_qwerty.kbd` are in the same directory

## 🚀 Usage

### Running Kanata

1. **Open Command Prompt as Administrator**
2. **Navigate to your Kanata directory**:
   ```cmd
   cd C:\kanata
   ```
3. **Run Kanata with the configuration**:
   ```cmd
   kanata.exe --cfg german_iso_qwerty.kbd
   ```

### Running in Background

There are multiple ways to run Kanata silently in the background:

#### 🛠️ Option 1: Using kanata_gui.exe (Recommended)

If you have `kanata_gui.exe`, this is the easiest method as it runs without showing a terminal window:

```cmd
kanata_gui.exe --cfg german_iso_qwerty.kbd
```

#### 🛠️ Option 2: VBScript Wrapper (Silent Launcher)

If you only have `kanata.exe`, wrap it in a small invisible launcher script:

1. **Create a file called `kanata_silent.vbs`** with this content:

   ```vbscript
   Set WshShell = CreateObject("WScript.Shell")
   WshShell.Run "C:\kanata\kanata.exe --cfg ""C:\kanata\german_iso_qwerty.kbd""", 0
   ```

   The `, 0` tells Windows to launch it hidden (no terminal window).

2. **Run the VBScript**:
   ```cmd
   wscript.exe "C:\kanata\kanata_silent.vbs"
   ```

#### 📊 Method Summary

| Method                                | Silent | Recommended | Notes                        |
| ------------------------------------- | ------ | ----------- | ---------------------------- |
| `kanata_gui.exe`                      | ✅     | ✅ Best     | No terminal window, built-in |
| VBScript with `kanata.exe`            | ✅     | ✅ Safe     | Hides terminal window        |
| `kanata.exe` alone via Task Scheduler | ❌     | ❌          | Terminal shows               |

### Autostart (Optional)

To automatically start Kanata on Windows boot:

#### Method 1: Using Task Scheduler (Recommended)

1. **Open Task Scheduler** as Administrator
2. **Create Basic Task**:

   - **Name**: "Kanata Keyboard"
   - **Trigger**: "When the computer starts"
   - **Action**: "Start a program"

3. **For kanata_gui.exe**:

   - **Program/script**: `C:\kanata\kanata_gui.exe`
   - **Arguments**: `--cfg "C:\kanata\german_iso_qwerty.kbd"`

4. **For VBScript method**:

   - **Program/script**: `wscript.exe`
   - **Arguments**: `"C:\kanata\kanata_silent.vbs"`

5. **Set Properties**:
   - ✅ Run with highest privileges
   - ✅ Run whether user is logged on or not

✅ **Now Kanata will launch silently and run in the background, no terminal popup.**

#### Method 2: Startup Folder (Simple)

1. Create a shortcut to your preferred launch method
2. Place the shortcut in: `C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`
3. Ensure the shortcut runs as administrator

## ⚙️ Configuration Details

### Timing Settings

- **Tap-Hold Timeout**: 200ms
- **Hold Timeout**: 200ms

These timings can be adjusted in the configuration file by modifying the values in the `tap-hold` definitions:

```lisp
(tap-hold <tap-timeout> <hold-timeout> <tap-action> <hold-action>)
```

### Key Processing

The configuration uses `process-unmapped-keys yes`, which means:

- Only the specified keys are remapped
- All other keys work normally
- No interference with other applications

## 🔧 Customization

### Adding More Keys

To add more tap-hold keys:

1. **Add the key to `defsrc`**:

   ```lisp
   (defsrc
     d f j k a s l h c v x your-new-key
   )
   ```

2. **Define the alias in `defalias`**:

   ```lisp
   your-new-key (tap-hold 200 200 your-new-key your-hold-action)
   ```

3. **Add to the layer**:
   ```lisp
   (deflayer base
     @d @f @j @k @a @s @l @h @c @v @x @your-new-key
   )
   ```

### Changing Timing

Adjust the timing values to suit your typing style:

- **Faster typers**: Decrease the first value (tap timeout)
- **Slower typers**: Increase both values
- **More accidental holds**: Increase the hold timeout

## 🚨 Emergency Exit

If Kanata becomes unresponsive, you can force quit by pressing and holding these three keys simultaneously:

**Left Ctrl + Space + Escape**

This emergency exit works even when Kanata is active.

## 🐛 Troubleshooting

### Common Issues

1. **"Access Denied" Error**:

   - Ensure you're running Command Prompt as Administrator
   - Kanata requires elevated privileges to intercept keyboard input

2. **Configuration Parse Errors**:

   - Check the syntax in your `.kbd` file
   - Ensure all parentheses are properly balanced
   - Verify alias names are prefixed with `@` when referenced

3. **Keys Not Working**:

   - Verify the key names match Kanata's expected key codes
   - Check that all keys in `defsrc` have corresponding entries in `deflayer`

4. **Conflicts with Other Software**:
   - Close other keyboard remapping software
   - Some games or applications may conflict with Kanata

### Getting Help

- **Kanata Documentation**: [Configuration Guide](https://github.com/jtroo/kanata/blob/main/docs/config.adoc)
- **Community Support**: [GitHub Discussions](https://github.com/jtroo/kanata/discussions)
- **Issue Reporting**: [GitHub Issues](https://github.com/jtroo/kanata/issues)

## 📝 License

This configuration is provided as-is for educational and personal use. Kanata itself is licensed under the LGPL-3.0 license.

## 🤝 Contributing

Feel free to submit issues and enhancement requests! If you improve this configuration, please share your changes.

---

**Happy typing!** 🎉
