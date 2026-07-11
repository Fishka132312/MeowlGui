# MeowlSploit

> A clean, modern, and powerful UI library for Roblox scripts.  
> Built for developers who want premium-looking menus without the hassle.

## ✨ Installation

Simply load the library into your script using `loadstring`:

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Fishka132312/MeowlGui/refs/heads/main/source/library.lua"))()
```

## 🚀 Quick Start

Here's a minimal example to create a window, add a page, and organize content into sections.

```lua
local Window = Library:Window({
    Name = "Cheat Name",
    SubName = "Best script ever",
    Logo = "123456789", -- Roblox Asset ID
    MenuKeybind = Enum.KeyCode.End
})

local Page = Window:Page({Name = "Ragebot", Icon = "rbxassetid://..."})
local Section = Page:Section({Name = "Main Settings", Side = 1}) -- Side 1 (Left) or 2 (Right)
```

---

## 📚 Documentation & API

### 1. Toggles
Toggles are the core building blocks of the library. They support **nested sub-options** that expand beautifully when activated.

```lua
local MyToggle = Section:Toggle({
    Name = "Silent Aim",
    Flag = "SilentAim", -- Unique identifier for Configs
    Default = false,
    Callback = function(Value)
        print("Silent Aim is now:", Value)
    end
})
```

#### Nested Elements (Sub-Options)
You can place other elements *inside* a toggle — they will appear only when the toggle is expanded.

```lua
-- Add a Slider inside the Toggle
MyToggle:Slider({
    Name = "FOV Radius",
    Min = 0, Max = 360, Default = 180
})

-- Add a Dropdown inside the Toggle
MyToggle:Dropdown({
    Name = "Hitbox",
    Items = {"Head", "Torso"},
    Default = "Head"
})
```

### 2. Sliders
Perfect for adjusting numeric values like speeds, radii, or delays.

```lua
local MySlider = Section:Slider({
    Name = "Walkspeed",
    Flag = "WS_Slider",
    Min = 16,
    Max = 200,
    Default = 16,
    Decimals = 1, -- 0 for integers, 1 for 0.5, etc.
    Suffix = " studs",
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
    end
})
```

### 3. Dropdowns
Support single & multi-selection + built-in search.

```lua
local MyDropdown = Section:Dropdown({
    Name = "Target Selection",
    Flag = "Target_Drop",
    Items = {"Player", "NPC", "Boss", "Ally"},
    Default = "Player", -- Use {"Player", "NPC"} for Multi
    Multi = false, -- Set to true for multiple selection
    Callback = function(Value)
        print("Selected:", Value)
    end
})
```

#### Updating Dropdowns
Refresh items or change selection dynamically from your code.

```lua
-- Refresh the list
MyDropdown:Refresh({"New Item 1", "New Item 2"}, "New Item 1")

-- Set a specific value programmatically
MyDropdown:Set("New Item 2")
```

### 4. Colorpickers & Keybinds
Can be used standalone or attached directly to toggles and other elements.

```lua
-- Standalone
Section:Colorpicker({
    Name = "ESP Color",
    Flag = "ESP_Color",
    Default = Color3.fromRGB(255, 0, 0),
    Alpha = 1, -- Transparency (0-1)
    Callback = function(Color, Alpha) ... end
})

Section:Keybind({
    Name = "Triggerbot Key",
    Flag = "Trigger_Bind",
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
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/ImInsane-1337/neverlose-ui/refs/heads/main/source/library.lua"))()

-- Enable Logs
Library.LogsEnabled = true

local Window = Library:Window({
    Name = "Project Nova",
    SubName = "Release Build",
    Logo = "123456789",
    MenuKeybind = Enum.KeyCode.RightShift
})

local MainTab = Window:Page({Name = "Combat", Icon = "rbxassetid://123..."})
local MainSection = MainTab:Section({Name = "Aimbot", Side = 1})

local AimToggle = MainSection:Toggle({
    Name = "Enabled",
    Flag = "AimEnabled",
    Default = false,
    Callback = function(v)
        if v then 
            Library:Log("Aimbot Enabled", 3)
        end
    end
})

-- Nested Slider
AimToggle:Slider({
    Name = "FOV",
    Min = 0, Max = 180, Default = 90
})

-- Nested Dropdown
AimToggle:Dropdown({
    Name = "Hitbox",
    Items = {"Head", "Torso", "Legs"},
    Default = "Head"
})

-- Create Settings Page (Configs, Scale, etc.)
Library:CreateSettingsPage(Window)
```
