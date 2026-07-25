# MeowlLibrary

> A clean, modern, and powerful UI library for Roblox.
> Built for developers who want premium-looking interfaces with minimal effort.
> Fully adapted for both PC and mobile.

## Installation

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Fishka132312/MeowlGui/refs/heads/main/source/library.lua"))()
```

## Quick Start

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Fishka132312/MeowlGui/refs/heads/main/source/library.lua"))()

local Window = Library:Window({
    Name = "My Script",
    SubName = "Meowl Library",
    Logo = "6274377111"          -- Roblox ID or a direct image link
})

local MainCat  = Window:Category("Main")
local MainPage = Window:Page({
    Name = "Main",
    Icon = "7539983773",         -- Roblox ID or a direct image link
    Category = MainCat
})

local MainSection = MainPage:Section({Name = "Hello", Side = 1})

MainSection:Button({
    Name = "Click me",
    Callback = function()
        print("clicked")
    end
})

Window:Init()                     -- always call this last
```

---

## Structure

```
Window
  |- Category            vertical tab group on the left
     |- Page             Name / Icon / Category / Columns (default 2)
        |- Section       Side = 1 (left) or 2 (right)
           |- elements
```

### Window

| Field | Default | Description |
|---|---|---|
| `Name` | `"Window"` | Title |
| `SubName` | `"Fine-tuning for sure wins"` | Subtitle under the title |
| `Logo` | - | Roblox ID or image link |

Methods: `Window:Category(Name)`, `Window:Page(Data)`, `Window:Init()`, `Window:SetOpen(Bool)`.

### Page

| Field | Default |
|---|---|
| `Name` | `"Page"` |
| `Icon` | `"100050851789190"` |
| `Category` | - |
| `Columns` | `2` |

### Section

| Field | Default | Description |
|---|---|---|
| `Name` | `"Section"` | Header text |
| `Description` | `""` | Small text under the header |
| `Icon` | `"123944728972740"` | Roblox ID or image link |
| `Side` | `1` | `1` = left column, `2` = right column |
| `EnableToggle` | `false` | Adds a checkbox to the section header that enables/disables all its elements |

---

## Tooltips

`Toggle`, `Button`, `Slider`, `Dropdown` and `Textbox` accept a `Tooltip` field.
The hint follows the cursor on PC and appears on tap-and-hold on mobile.

```lua
Section:Toggle({
    Name = "Show FPS",
    Tooltip = "Draws an FPS counter in the corner of the screen",
    Flag = "ShowFPS",
    Callback = function(Value) end
})
```

Other elements (Label, Paragraph, Keybind, Listbox, ProgressBar, Image) do **not** support `Tooltip`.

---

## Elements

### Button

```lua
Section:Button({
    Name = "Click Me",
    Icon = nil,                   -- optional, Roblox ID or image link
    Tooltip = "Runs the action",
    Callback = function()
        print("Button was clicked!")
    end
})
```

### Toggle

```lua
local MyToggle = Section:Toggle({
    Name = "Show FPS",
    Tooltip = "Shows a hint on hover",
    Flag = "ShowFPS",             -- unique identifier used by Configs
    Default = false,
    Callback = function(Value)
        print("Show FPS is now:", Value)
    end
})

MyToggle:Set(true)
print(MyToggle:Get())
```

**Attached elements.** A toggle can carry its own colorpicker and keybind:

```lua
MyToggle:Colorpicker({ Flag = "FpsColor", Default = Color3.new(1, 1, 1) })
MyToggle:Keybind({ Flag = "FpsKey", Default = Enum.KeyCode.X })
```

**Settings panel.** `Toggle:Settings(Size)` adds a gear icon that opens a small popup panel; you can put extra elements inside it:

```lua
local Settings = MyToggle:Settings(120)
Settings:Slider({ Name = "Size", Min = 1, Max = 30, Default = 14 })
```

### Slider

```lua
local MySlider = Section:Slider({
    Name = "Camera FOV",
    Tooltip = "Field of view",
    Flag = "CameraFOV",
    Min = 16,
    Max = 120,
    Default = 70,
    Decimals = 1,                 -- step: 1 = integers, 0.1, 0.01 ...
    Suffix = "",                  -- text after the number, e.g. " deg"
    Callback = function(Value)
        print("Camera FOV set to:", Value)
    end
})

MySlider:Set(90)
```

### Dropdown

```lua
local MyDropdown = Section:Dropdown({
    Name = "Select Preset",
    Tooltip = "Pick a preset",
    Flag = "SelectPreset",
    Items = {"Default", "Dark", "Light", "Custom"},
    Default = "Default",          -- use {"Default", "Dark"} when Multi = true
    Multi = false,
    Size = 125,                   -- width of the dropdown
    OptionHolderSize = 125,       -- max height of the open list
    Callback = function(Value)
        print("Selected:", Value)
    end
})

MyDropdown:Refresh({"New 1", "New 2"}, "New 1")   -- rebuild the list
MyDropdown:Set("New 2")                           -- select programmatically
```

### Listbox

A permanently expanded list. Used by the built-in config manager.

```lua
local MyList = Section:Listbox({
    Flag = "ConfigsList",
    Items = {"one", "two"},
    Default = nil,
    Multi = false,
    Size = 125,
    Callback = function(Value) end
})
```

### Textbox

```lua
local MyInput = Section:Textbox({
    Name = "Player Name",
    Tooltip = "Who to target",
    Flag = "PlayerName",
    Default = "",
    Placeholder = "Enter nickname...",
    Numeric = false,              -- true = numbers only
    Finished = false,             -- true = fire only on Enter / focus lost
    Callback = function(Value)
        print("Entered:", Value)
    end
})
```

### Label & Colorpicker

There is no standalone `Section:Colorpicker`. A colorpicker is always attached to a **Label** or a **Toggle**.

```lua
local MyLabel = Section:Label("Box color")

MyLabel:Colorpicker({
    Flag = "BoxColor",
    Default = Color3.fromRGB(255, 0, 0),
    Alpha = 1,                    -- optional transparency channel (0-1)
    Callback = function(Color, Alpha) end
})

MyLabel:SetText("New text")
```

### Keybind

```lua
Section:Keybind({
    Name = "Toggle Menu",
    Flag = "ToggleMenu",
    Default = Enum.KeyCode.E,
    Mode = "Toggle",              -- "Toggle", "Hold" or "Always"
    Callback = function(State) end
})
```

The mode can also be changed at runtime by right-clicking the keybind in the menu.

---

## Decorative & informational elements

### Divider

A thin separator line. Takes no arguments.

```lua
Section:Divider()
```

### Paragraph

A title plus a block of wrapped text. Useful for instructions and warnings.

```lua
local Info = Section:Paragraph({
    Name = "How it works",
    Text = "Enable the toggle and stand near the spawn point. The script does the rest."
})

Info:SetText("Updated description")
```

### ProgressBar

A read-only bar. Good for FPS, ping, cooldowns or loading progress.

```lua
local Bar = Section:ProgressBar({
    Name = "FPS",
    Min = 0,
    Max = 240,
    Suffix = "",                  -- default is "%"
    Default = 0
})

Bar:Set(144)
```

### Image

```lua
local Logo = Section:Image({
    Id = "129442179713871",       -- Roblox ID or a direct image link
    Height = 110,
    Rounded = true
})

Logo:Set("7539983773")
```

---

## Notifications

```lua
Library:Notification({
    Title = "Config",
    Description = "Loaded successfully",
    Icon = "7539983773",          -- optional
    Duration = 5,                 -- seconds, required
    IconColor = {                 -- optional gradient for the icon
        Start = Color3.fromRGB(0, 255, 0),
        End   = Color3.fromRGB(0, 120, 0)
    }
})
```

---

## Keybind list

An on-screen panel showing active keybinds.

```lua
local KeybindList = Library:KeybindList("Keybinds")

KeybindList:Add("Aimbot", "E")
KeybindList:SetVisibility(true)
```

---

## Themes

Built-in presets: `Preset`, `Midnight`, `Ocean`, `Rose`, `Mono`, `Ember`.

```lua
Library:SetThemePreset("Midnight")
```

Manual color changes:

```lua
local Accent   = Color3.fromRGB(255, 65, 85)
local Gradient = Color3.fromRGB(140, 10, 45)

Library.Theme.Accent = Accent
Library.Theme.AccentGradient = Gradient
Library:ChangeTheme("Accent", Accent)
Library:ChangeTheme("AccentGradient", Gradient)
```

Available theme keys: `Accent`, `AccentGradient`, `Background`, `Background 2`, `Section Background`, `Section Background 2`, `Section Top`, `Element`, `Outline`, `Text`.

---

## Ready-made pages

### Settings page

Config manager (create / load / save / delete / auto-load) plus Watermark and Keybind List toggles.

```lua
local SettingsCat  = Window:Category("Settings")
local SettingsPage = Library:CreateSettingsPage(Window, KeybindList)
table.insert(SettingsCat.Elements, SettingsPage)
```

The second argument is optional and is only needed for the "Keybind List" toggle.

### UI page

Theme presets, background presets, manual gradient colors, interface recoloring and custom background images.

```lua
local UiPage = Library:CreateUiPage(Window)
table.insert(SettingsCat.Elements, UiPage)
```

Both functions return the page, so you can append your own sections afterwards:

```lua
local InfoSection = UiPage:Section({Name = "Info", Side = 2})
InfoSection:Image({ Id = "129442179713871", Height = 110 })
```

---

## Configs

Set the folders **before** creating the window:

```lua
local CheatName = "MyScript"

Library.Folders = {
    Directory = CheatName,
    Configs   = CheatName .. "/Configs",
    Assets    = CheatName .. "/Assets"
}
```

Every element with a `Flag` is saved automatically. Current values live in `Library.Flags`:

```lua
if Library.Flags["ShowFPS"] then
    -- ...
end
```

---

## Updating values programmatically

```lua
MyToggle:Set(true)
MySlider:Set(50)
MyDropdown:Set("Option 1")
MyLabel:SetText("Text")
Paragraph:SetText("Text")
ProgressBar:Set(75)
Image:Set("7539983773")
```

---

## Safe unloading

```lua
Library:Unload()
```

---

## Mobile support

- A floating button opens and closes the menu when no keyboard is available.
- Buttons and toggles are enlarged to comfortable touch sizes.
- Dragging the window, scrolling the tab list and scrolling sections never trigger the element underneath: a press only counts if the finger did not move.
- Tooltips appear on tap-and-hold.
- The camera does not rotate while the window is being dragged.

---

## Full example

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Fishka132312/MeowlGui/refs/heads/main/source/library.lua"))()
local CheatName = "Hello"

Library.Folders = {
    Directory = CheatName,
    Configs   = CheatName .. "/Configs",
    Assets    = CheatName .. "/Assets"
}

local Accent   = Color3.fromRGB(255, 65, 85)
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

local KeybindList = Library:KeybindList("Keybinds")

-- ============ MAIN ============
local MainCat  = Window:Category("Main")
local MainPage = Window:Page({ Name = "Main", Icon = "7539983773", Category = MainCat })

local TestSection = MainPage:Section({Name = "Test", Side = 1})

TestSection:Paragraph({
    Name = "About",
    Text = "Pick a mode below and press Apply. Hover any element to see a hint."
})

TestSection:Divider()

TestSection:Dropdown({
    Name = "Select Mode",
    Tooltip = "Behaviour preset",
    Flag = "SelectMode",
    Items = {"Default", "Dark", "Light", "Custom"},
    Default = "Default",
    Callback = function(Value) print("Selected:", Value) end
})

TestSection:Toggle({
    Name = "Apply Mode",
    Tooltip = "Applies the selected mode",
    Flag = "ApplyMode",
    Default = false,
    Callback = function(Value) print("Apply Mode:", Value) end
})

local StatsSection = MainPage:Section({Name = "Stats", Side = 2})

local FpsBar = StatsSection:ProgressBar({ Name = "FPS", Min = 0, Max = 240, Suffix = "" })

task.spawn(function()
    local RunService = game:GetService("RunService")
    local Frames, Clock = 0, os.clock()

    RunService.RenderStepped:Connect(function()
        Frames += 1

        if os.clock() - Clock >= 1 then
            FpsBar:Set(Frames)
            Frames, Clock = 0, os.clock()
        end
    end)
end)

StatsSection:Slider({
    Name = "Camera FOV",
    Tooltip = "Field of view",
    Flag = "CameraFOV",
    Min = 16, Max = 120, Default = 70, Decimals = 1,
    Callback = function(Value) workspace.CurrentCamera.FieldOfView = Value end
})

-- ============ VISUALS ============
local VisualCat  = Window:Category("Visuals")
local VisualPage = Window:Page({ Name = "ESP", Icon = "7539983773", Category = VisualCat })

local EspSection = VisualPage:Section({Name = "Players", Side = 1})

local EspToggle = EspSection:Toggle({
    Name = "Box ESP",
    Tooltip = "Draws a box around every player",
    Flag = "BoxEsp",
    Default = false,
    Callback = function(Value) print("ESP:", Value) end
})

EspToggle:Colorpicker({ Flag = "BoxEspColor", Default = Color3.fromRGB(255, 255, 255) })
EspToggle:Keybind({ Flag = "BoxEspKey", Default = Enum.KeyCode.X })

-- ============ SETTINGS ============
local SettingsCat = Window:Category("Settings")

local UiPage = Library:CreateUiPage(Window)
table.insert(SettingsCat.Elements, UiPage)

local SettingsPage = Library:CreateSettingsPage(Window, KeybindList)
table.insert(SettingsCat.Elements, SettingsPage)

Window:Init()

Library:Notification({
    Title = CheatName,
    Description = "Loaded successfully",
    Duration = 5
})
```
