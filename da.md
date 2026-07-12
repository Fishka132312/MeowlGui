# MeowlLibrary

> A clean, modern, and powerful UI library for Roblox.
> Built for developers who want premium-looking interfaces with minimal effort.

## ✨ Installation

Simply load the library into your script using `loadstring`:

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Fishka132312/MeowlGui/refs/heads/main/source/library.lua"))()
```

## 🚀 Quick Start

Here's a minimal example to create a window, add a page, and organize content into sections.

```lua
local MainCat = Window:Category("Main")
local MainPage = Window:Page({
		Name = "Main",
		Icon = "7539983773",
		Category = MainCat
})

local MainSection = MainPage:Section({Name = "hello", Side = 1}) -- Side 1 (Left) or 2 (Right)
```

---

## 📚 Documentation & API

### 1. Buttons
Buttons are used to trigger one-time actions such as applying settings, refreshing data, or executing custom functions.

```
Section:Button({
    Name = "Click Me",
    Callback = function()
        print("Button was clicked!")
    end
})
```

### 2. Toggles
Toggles are the core building blocks of the library. They support **nested sub-options** that expand beautifully when activated.

```lua
local MyToggle = Section:Toggle({
    Name = "Show FPS",
    Flag = "ShowFPS", -- Unique identifier for Configs
    Default = false,
    Callback = function(Value)
        print("Show FPS is now:", Value)
    end
})
```

### 3. Dropdown
Perfect for selecting from a list of predefined options such as game modes, difficulties, weapon types, themes, or any other categorical settings.

```lua
local MyDropdown = Section:Dropdown({
    Name = "Select Preset",
    Flag = "SelectPreset",
    Items = {"Default", "Dark", "Light", "Custom"},
    Default = "Default", -- Use {"Default", "Dark"} for Multi
    Multi = false, -- Set to true for multiple selection
    Callback = function(Value)
        print("Selected:", Value)
    end
})
```

### Updating Dropdowns
Refresh items or change selection dynamically from your code.

```lua
-- Refresh the list
MyDropdown:Refresh({"New Item 1", "New Item 2"}, "New Item 1")

-- Set a specific value programmatically
MyDropdown:Set("New Item 2")
```

### 4. Sliders
Perfect for adjusting numeric values like speeds, radii, or delays.

```lua
local MySlider = Section:Slider({
    Name = "Camera FOV",
    Flag = "CameraFOV",
    Min = 16,
    Max = 120,
    Default = 16,
    Decimals = 1, -- 0 for integers, 1 for 0.5, etc.
    Suffix = "", -- Text shown after the number (e.g. 85°)
    Callback = function(Value)
        print("Camera FOV set to:", Value)
    end
})
```

### 5. Textboxes (Inputs)
Perfect for entering custom text values such as usernames, messages, commands, URLs, or any string data.

```lua
local MyInput = MainSection:Textbox({
    Name = "Player Name",           -- Label shown above the textbox
    Flag = "PlayerName",               -- Unique flag to save/load the value
    Default = "",                   -- Default value
    Placeholder = "Enter nickname or text...", -- Hint text when empty
    Numeric = false,                -- true = only allows numbers (with decimals if needed)
    Finished = false,               -- false = callback on every change, true = callback only on Enter / focus lost
    Callback = function(Value)
        print("Entered:", Value)
    end
})
```

### 6. Colorpickers & Keybinds
Can be used standalone or attached directly to toggles and other elements.

```lua
-- Standalone
Section:Colorpicker({
    Name = "Particle Color",
    Flag = "ParticleColor",
    Default = Color3.fromRGB(255, 0, 0),
    Alpha = 1, -- Transparency (0-1)
    Callback = function(Color, Alpha) ... end
})

Section:Keybind({
    Name = "Toggle Menu",
    Flag = "ToggleMenu",
    Default = Enum.KeyCode.E,
    Mode = "Hold", -- "Toggle", "Hold", or "Always"
    Callback = function(State) ... end
})

-- Attached to a Toggle
MyToggle:Colorpicker({ Default = Color3.new(1,1,1) })
MyToggle:Keybind({ Default = Enum.KeyCode.X })
```

---

## 🖥️ Event Logger

Built-in draggable logger that snaps to the center of the screen. Great for debugging and user feedback.

**Enable it first:**
```lua
Library.LogsEnabled = true -- Required to show the panel
```

**Send logs:**
```lua
-- Library:Log(Text, Duration, Color)
Library:Log("Config Loaded", 5, Color3.fromRGB(0, 255, 0))
Library:Log("Target missed", 3, Color3.fromRGB(255, 0, 0))
```

---

## ⚙️ Settings & Configs

Add a ready-made Settings page (Menu Scale, Config Manager, Watermark, etc.) with one line:

```lua
Library:CreateSettingsPage(Window)
```

### Menu Scale
Supports DPI scaling. Best to let users change it in Settings, but you can also set it manually:

```lua
-- Values: 0.5, 0.75, 1, 1.25, 1.5
Library.CurrentScale = 1.25 
-- Recommended: use the built-in Settings Page dropdown instead of setting manually.
```

---

## 🔄 Updating Values Programmatically

Change any element's state from your script using `:Set()`.

**Toggle:**
```lua
local Toggle = Section:Toggle(...)
Toggle:Set(true) -- Turns it on
```

**Slider:**
```lua
local Slider = Section:Slider(...)
Slider:Set(50) -- Sets value to 50
```

**Dropdown:**
```lua
local Dropdown = Section:Dropdown(...)
Dropdown:Set("Option 1") -- Selects Option 1
```

---

## 🛡️ Safe Unloading

Unload cleanly without triggering anti-cheats:

```lua
Library:Unload()
```

---

## 📝 Full Example Script

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Fishka132312/MeowlGui/refs/heads/main/source/library.lua"))()
local Name = "Hello"

Library.Folders = {
    Directory = CheatName,
    Configs = CheatName .. "/Configs",
    Assets = CheatName .. "/Assets",
}

local Accent = Color3.fromRGB(255, 65, 85)
local Gradient = Color3.fromRGB(140, 10, 45)

Library.Theme.Accent = Accent
Library.Theme.AccentGradient = Gradient
Library:ChangeTheme("Accent", Accent)
Library:ChangeTheme("AccentGradient", Gradient)
local Window = Library:Window({
    Name = "Hello",
    SubName = "Meowl Library",
    Logo = "6274377111"
})

-------------------------Main-----------------------

local MainCat = Window:Category("Main")
local MainPage = Window:Page({
		Name = "Main",
		Icon = "7539983773",
		Category = MainCat
})

local MainSection = MainPage:Section({Name = "Test", Side = 1})

local MyDropdown = Section:Dropdown({
    Name = "Select Mode",
    Flag = "SelectMode",
    Items = {"Default", "Dark", "Light", "Custom"},
    Default = "Default", -- Use {"Default", "Dark"} for Multi
    Multi = false, -- Set to true for multiple selection
    Callback = function(Value)
        print("Selected:", Value)
    end
})

local MyToggle = MainSection:Toggle({
    Name = "Apply Mode",
    Flag = "ApplyMode",
    Default = false,
    Callback = function(Value)
        print("Apply Mode is now:", Value)
    end
})

local MainSection = MainPage:Section({Name = "Test1", Side = 2})

local MySlider = MainSection:Slider({
    Name = "Camera FOV",
    Flag = "CameraFOV",
    Min = 16,
    Max = 120,
    Default = 16,
    Decimals = 1,
    Suffix = "",
    Callback = function(Value)
        print("Camera FOV set to:", Value)
    end
})

MainSection:Button({
    Name = "Set Camera Fov",
    Callback = function()
        print("Button was clicked!")
    end
})

local PlayersCat = Window:Category("Players")
local PlayersPage = Window:Page({
		Name = "Players",
		Icon = "7539983773",
		Category = PlayersCat
})

local PlayersSection = PlayersPage:Section({Name = "Ban", Side = 1})

local MyInput = PlayersSection:Textbox({
    Name = "Player Name",
    Flag = "PlayerName",
    Default = "",
    Placeholder = "Enter nickname or text...",
    Numeric = false,
    Finished = false,
    Callback = function(Value)
        print("Entered:", Value)
    end
})

PlayersSection:Button({
    Name = "Ban Player!",
    Callback = function()
        print("Button was clicked!")
    end
})

local PlayersSection = PlayersPage:Section({Name = "Ban Reason", Side = 2})

local MyInput = PlayersSection:Textbox({
    Name = "Ban Reason",
    Flag = "BanReason",
    Default = "",
    Placeholder = "Enter ban reason...",
    Numeric = false,
    Finished = false,
    Callback = function(Value)
        print("Ban reason is:", Value)
    end
})

PlayersSection:Colorpicker({
    Name = "Text Color",
    Flag = "TextColor",
    Default = Color3.fromRGB(255, 0, 0),
    Alpha = 1, -- Transparency (0-1)
    Callback = function(Color, Alpha) ... end
})

PlayersSection:Button({
    Name = "Apply Reason!",
    Callback = function()
        print("Button was clicked!")
    end
})

local GuiCat = Window:Category("Gui")
local PlayersPage = Window:Page({
		Name = "Gui",
		Icon = "7539983773",
		Category = GuiCat
})

local GuiSection = GuiPage:Section({Name = "Gui", Side = 1})

Section:Keybind({
    Name = "Gui Keybind",
    Flag = "GuiKeybind",
    Default = Enum.KeyCode.E,
    Mode = "Hold", -- "Toggle", "Hold", or "Always"
    Callback = function(State) ... end
})

local SettingsCat = Window:Category("Settings")
local SettingsPage = Library:CreateSettingsPage(Window, KeybindList)
table.insert(SettingsCat.Elements, SettingsPage)
Window:Init()
```
