# Library

# PAYOMBØYZ UI Library

ไลบรารี UI แบบ single-file สำหรับ Luau/Roblox ที่ออกแบบให้เป็นฐานต่อยอดของ Script Hub ได้หลายเกมและหลายสไตล์ ภาพหลักได้แรงบันดาลใจจากแบรนด์ PAYOMBØYZ: ดำหมึก แดงแรง ขอบคม แสงนีออน และรายละเอียดแบบหน้า manga โดยคุม contrast ให้อ่านง่ายและรักษาพื้นที่กดบนมือถือ

ไฟล์ที่โหลดเป็นโมดูลคือ [`Obsidain_Glassmophic2.txt`](Obsidain_Glassmophic2.txt) ใช้ raw URL ที่ระบุได้โดยตรง ไฟล์นี้ไม่ต้องโหลดไอคอนหรือแพ็กเกจ UI เพิ่ม ส่วนคู่มือและตัวอย่างในโฟลเดอร์นี้ใช้ตอนพัฒนา/ปรับธีม

## เริ่มใช้งาน

```lua
local Library = loadstring(game:HttpGet(
    "https://raw.githubusercontent.com/payomboyz333/Library/refs/heads/main/PayomboyZ-UI"
))()

local Window = Library:CreateWindow({
    Title = "PAYOMBØYZ HUB",
    Name = "GAME HUB",
    Subtitle = "MANGA EDITION",
    DefaultTab = "Home",
    Theme = "แดงเปลวเพลิง",
    Logo = "https://raw.githubusercontent.com/payomboyz333/Library/refs/heads/main/LogoPayomboyZ.png",
    Motion = "Full",
    -- Uses the repository PAYOMBØYZ logo by default; supply an rbxassetid or raw image URL to replace it.
    Effects = {
        Lightning = true,
        Sparks = true,
        CapsuleSparks = true,
        Sound = true,
        Intro = true,
        SparkCount = 3,
        CapsuleSparkCount = 1,
    },
})

local farm = Window:AddTab({ Title = "Farm", Icon = "⚔" })
local section = farm:CreateSection({ Name = "Automation", Expanded = true })

section:CreateToggle({
    Name = "Auto Farm",
    Note = "Description shown under the control title",
    Default = false,
    Callback = function(enabled)
        print("Auto Farm:", enabled)
    end,
})

section:CreateDropdown({
    Name = "Target",
    Options = { "Nearby", "Selected", "All" },
    Default = "Nearby",
    Callback = function(value) print(value) end,
})

section:CreateButton({ Name = "Refresh", Callback = function() print("Refresh") end })
Window:CreateAppearanceTab() -- Adds theme, motion, effects, and density controls
```

`CreateWindow` also accepts `Title`, `Name`, `Subtitle`/`SubTitle`, `VersionTag`, `Logo`, `DefaultTab`, `Theme`, `Motion`, and `Effects`. `Logo` accepts an `rbxassetid://...` asset, raw image URL, or local file path; it defaults to `LogoPayomboyZ.png` in this repository. Raw URLs require local asset support (`writefile` and `getcustomasset` or `getsynasset`); local paths need `getcustomasset` or `getsynasset`. The existing loader shape (`CreateWindow`, `GetDefaultTab`, `CreateTab`, `Finalize`) remains available.

The published module is stored in the repository root at `PayomboyZ-UI` (without a file extension). Use `https://raw.githubusercontent.com/payomboyz333/Library/refs/heads/main/PayomboyZ-UI` exactly as shown above; do not append the local filename. The README and Example can remain local and are not required for the module download. `Window:SetLogo(source)` changes the intro, main header, capsule, and profile mark while the window is open.

The sidebar utility buttons can be replaced or extended with `QuickActions = { { Name = "My action", Callback = function(window) ... end } }`. Each callback receives the window object. Optional `OnLogout` and `OnUnload` callbacks let the host script respond to the matching lifecycle event. `window:Unload()` disconnects tracked input/render listeners, stops active toggles, and destroys the UI.

## Theme presets

The seven built-in themes keep the dark ink surfaces, white readable text, and sharp anime framing. Each changes its particle color pair, travel direction, drift, spin, lifetime, and glow strength along with the accent. Pooled GUI spark sprites fade in and out near the outer edges, keeping the content area clear. The default is `แดงเปลวเพลิง`.

| Preset | Accent | Mood |
| --- | --- | --- |
| `ดำทมิฬ` | เงินหม่น | เศษเถ้าลอยช้าและสัญลักษณ์คม |
| `แดงเปลวเพลิง` | แดงชาด | ประกายไฟลอยขึ้นและแกว่งแรง |
| `เขียวมรกต` | เขียวมรกต | กลีบ/ใบไม้หมุนลงอย่างนุ่มนวล |
| `ฟ้าคลื่นทะเล` | ฟ้าคราม | คลื่นไหลด้านข้างพร้อมจังหวะขึ้นลง |
| `ม่วงแห่งความลึกลับ` | ม่วงอเมทิสต์ | อักขระเรืองแสงลอยขึ้นและเต้นแสง |
| `ทองแห่งเกียรติยศ` | ทองอำพัน | กลีบดอกสีทองหมุนช้า |
| `ขาวแห่งความบริสุทธิ์` | ขาว/ฟ้าเงิน | เกล็ดหิมะตกช้า |

Switch a running window with `Window:SetTheme("เขียวมรกต")`; query it with `Window:GetTheme()`. The old names `Crimson`, `Jade`, `Azure`, `Amethyst`, `Sakura`, `Amber`, and `Violet` remain accepted as compatibility aliases. The module also exposes `Library:GetThemes()` and `Library:SetTheme(nameOrPalette)`. A custom palette can provide `Accent`, `Bright`, `Deep`, and `Tint` as `Color3` values, plus an optional `Particle` table (`Glyph`, `Style`, `Direction`, `Speed`, `Drift`, `Sway`, `Spin`, `Lifetime`, `Opacity`, `Colors`). Existing controls, gradient accents, and particle movement update when a preset changes.

## Motion and effects

Set `Motion` to `Full`, `Subtle`, or `Off`. `Subtle` shortens transition time; `Off` applies transition end states immediately. These modes cover window intro, tab changes, section expansion, dropdowns, toggles, hover states, and text input focus.

Effects are optional and can be selected per window in `Effects`, or changed live with `Window:SetEffects(name, value)`:

- `Lightning`: procedural crimson arcs and the ambient flash
- `Sparks`: low-cost, theme-specific particles that softly fade and respawn
- `CapsuleSparks`: particles in the draggable open/close capsule
- `Sound`: interface click sound
- `Intro`: startup morph sequence (startup only)
- `SparkCount`: 0–7 pooled main particles, each with its own color and shape; default is 2 on touch devices and 3 otherwise
- `CapsuleSparkCount`: 0–2 pooled capsule sprites; default is 0 on touch devices and 1 otherwise

`Window:CreateAppearanceTab()` adds a ready-to-use Appearance tab with controls for theme, custom accent color, motion, lightning, both spark layers, click sound, and capped spark density. Main sparks use a small pooled image-sprite set and one throttled updater; lightning and capsule sparks start off unless explicitly enabled. The custom color updates the live UI palette. `Window:SetAccent(color3)` is also available to scripts. `Window:CreateSettingsTab()` is an alias. Applications can also compose their own settings pages using the same controls.

Tabs slide a short distance into view with a moving accent edge marker. Changing tabs closes expanded dropdowns so menus do not stay open off-screen. Set `Motion = "Off"` to disable transitions. A semantic icon set keeps script hubs visually consistent: `Icon = "Farm"`, `"Combat"`, `"Quest"`, `"Settings"`, `"Travel"`, `"Visuals"`, `"Trade"`, `"Crown"`, or `"Spark"` resolves to a compact glyph; existing emoji and glyph strings still work. Add or override an alias with `Library:RegisterIcon("Raid", "☠")` before creating the tab.

## Public API

### Window

- `AddTab({ Title, Icon })` or `CreateTab({ Name, Title, Icon })`; icon aliases are optional
- `GetDefaultTab()` creates/returns the default tab named by `DefaultTab`
- `CreateAppearanceTab(title)` / `CreateSettingsTab(title)` builds reusable appearance settings
- `SetTheme(nameOrPalette)`, `SetAccent(color3)`, `GetTheme()`, `SetMotion("Full" | "Subtle" | "Off")`, `SetLogo(source)`
- `SetEffects(name, value)`, `Toggle()`, `Close()`, `Unload()`
- `CreateState({ Name, Default })`, `GetState(name)`, `CreateExclusiveGroup({ Name })`
- `Gui`, `Shell`, `Tabs`, `CurrentTab`, `Options`, and `States` are exposed for extension

### Tabs and sections

Tabs support `CreateSection({ Name, Description, Icon, Expanded })` and direct control calls. Sections can be collapsed with `section:SetExpanded(boolean)`. Section contents are placed in cards, distributed into responsive columns, and returned controls expose `Get`, `GetValue`, `Set`, `SetValue`, `Subscribe`, and `SetOptions` where relevant.

Controls: `CreateToggle`, `CreateSlider`, `CreateDropdown`, `CreateMultiDropdown`, `CreateInput`, `CreateKeybind`, `CreateColorPicker`, `CreateRadioGroup`, `CreateProgressBar`, `CreateStatCard`, `CreateBarChart`, `CreateCallout`, `CreateButton`, `CreateText`, and `CreateLabel`. Most configs accept `Name`/`Title`, `Tooltip`/`Note`/`Description`, and `Callback`. Buttons accept an optional semantic `Icon`. Dropdowns accept `Options` or `Values`; sliders accept `Min`, `Max`, `Default`, and `Rounding`.

### Control guide

All controls created with `section:Create...()` return a handle with `Get()`, `GetValue()`, `Set(value, fireCallback)`, `SetValue(value, fireCallback)`, and `Subscribe(callback)`. Component-specific methods such as `SetColor`, `SetCaption`, `SetData`, and `Push` are forwarded to the component.

```lua
local mode = section:CreateRadioGroup({
    Name = "Mode",
    Values = { "Balanced", "Fast", "Quiet" },
    Default = "Balanced",
    Callback = function(value) print("Mode:", value) end,
})

local tint = section:CreateColorPicker({
    Name = "Accent",
    Default = Color3.fromRGB(255, 45, 65),
    Callback = function(color) print(color:ToHex()) end,
})

local progress = section:CreateProgressBar({ Name = "Download", Min = 0, Max = 100 })
progress:SetValue(42)
progress:SetColor(Color3.fromRGB(255, 180, 60))

local total = section:CreateStatCard({
    Name = "Session time",
    Default = "00:12:08",
    Caption = "Current session",
})
total:SetValue("00:12:09")
total:SetCaption("Updated just now")

local graph = section:CreateBarChart({
    Name = "Activity",
    Values = { 2, 4, 3, 8, 6, 10 },
    MaxPoints = 16,
})
graph:Push(12)
graph:SetData({ 3, 5, 7, 4, 9 })
```

`CreateKeybind` accepts `Default = "K"` or an `Enum.KeyCode`, and `Mode = "Toggle"` or `"Hold"`. Its callback receives `(keyCode, active)`. Click its key capsule to capture a new key; Escape clears it.

`CreateColorPicker` opens a swatch palette and a hex field. `SetValue` accepts a `Color3` or six digit hex string. `CreateRadioGroup` provides a single-choice list. `CreateProgressBar`, `CreateStatCard`, and `CreateBarChart` expose update methods for live status pages.

`CreateDropdown` includes an option filter. `CreateMultiDropdown` supports selected-value dictionaries, `SetOptions(options, default, preserve)`, and `SetValue`. `CreateInput` supports `Multiline`, `Numeric`, `MaxLength`, `ReadOnly`, and `Validator`; a validator returns `true` to accept input or `false, message` to reject it and show the reason. Buttons can request a second click with `Confirm = true` or a custom confirmation label, and `ConfirmWindow` sets its timeout in seconds.

`CreateCallout` accepts `Kind = "Info" | "Success" | "Warning" | "Danger"`. `CreateDivider("Section label")` adds a labeled separator. Put `Tooltip`, `Note`, or `Description` on a control to show a hover help card (or a short tap card on touch devices).

### Search and keyboard commands

```lua
local palette = Window:CreateCommandPalette({
    {
        Name = "Open appearance",
        Description = "Themes and motion",
        Category = "Navigation",
        Callback = function(window) window:SelectTab("Appearance") end,
    },
})

palette:Register({
    Name = "Recenter window",
    Description = "Move the hub back to the center",
    Callback = function(window) window:Center() end,
})

palette:Open() -- Ctrl+K opens and closes the palette
```

Palette rows support `Name`, `Description`, `Category`, `Keywords`, and `Callback(window)`. Arrow keys move selection, Enter runs the highlighted command, and Escape closes the palette. Use `palette:Remove(name)`, `palette:List()`, and `palette:Destroy()` to manage commands.

`Window:Search(query)` filters controls that were built through the modern `Create...` API and returns the number of matches. An empty query restores all those controls. `Window:FindControl(nameOrKey)` looks up a handle, while `Window:GetControls()` returns the registered handles. Tabs expose `SetBadge(text, color)`, `SetVisible(boolean)`, and `Activate()`; the window has `SelectTab(nameOrTab)` and `RemoveTab(nameOrTab)`.

### Configuration and persistence

```lua
local profiles = Window:CreateConfigManager()
profiles:Save("Farm")
local json = profiles:Encode("Farm") -- Store this with your host's persistence layer

profiles:Save("Combat")
profiles:Load("Farm")
profiles:List()
profiles:Delete("Combat")
profiles:Import(json, "Imported", true) -- Store and apply
```

`Save` and `Load` operate on the manager's in-memory profiles. `Encode` returns a JSON string and `Import` reads one. The manager deliberately leaves file storage to the caller, so the library does not assume executor-only file APIs. `Window:ExportConfig()` and `Window:ApplyConfig(config)` are also available directly. Values include toggles, sliders, inputs, dropdowns, color picks, radio selections, and keybinds.

The config envelope is `{ Version, Theme, Motion, Controls }`. Color values and enum values are converted to tagged JSON-safe tables and restored on import.

### Reusable utilities

- `Library:RegisterTheme(name, { Accent, Bright, Deep, Tint })`, `RemoveTheme(name)`, `GetThemePalette(name)`
- `Library:SafeCall(callback, ...)` returns the `pcall` result
- `Library:DeepCopy(value)` copies tables while preserving cycles
- `Library:CreateSignal()` returns `Connect`, `Once`, `Fire`, `Wait`, and `Destroy`
- `Library:CreateJanitor()` returns `Add`, `Remove`, `Cleanup`, and `Destroy`
- `Library:DismissNotifications()` and `Library:GetNotificationCount()` manage toasts

The Janitor can clean up functions, `RBXScriptConnection`s, instances, and task threads. Pass a method name as the second argument for custom cleanup (`janitor:Add(object, "Disconnect", "input")`).

### Notifications

```lua
Library:Notify({ Title = "Saved", Content = "Your settings were saved.", Duration = 3 })

local toast = Library:Notify({
    Type = "Success", -- Info, Success, Warning, Error
    Title = "Profile exported",
    Content = "The JSON is ready to copy.",
    Duration = 0, -- stay open until dismissed when duration is zero
    Action = {
        Name = "Open settings",
        Callback = function() Window:SelectTab("Settings") end,
    },
})
toast:Update({ Content = "Copied to clipboard." })
toast:Dismiss()
```

Notifications stack at the top right and cap the visible queue at five. `Action` may include `Dismiss = false` to keep the toast open after the callback. `Library:DismissNotifications()` clears active toasts, and `Library:GetNotificationCount()` reports their count.

## Design and extension notes

- Theme and animation code stays in the library; game actions belong in the calling script's callbacks.
- Keep surfaces dark in accent themes so text stays readable. Use accent colors for actions, selection, and status highlights.
- Use short labels with a one-line `Note`; long explanations belong in `CreateText` or a separate help section.
- Prefer `CreateAppearanceTab` as a starting point, then add game-specific settings in separate tabs/sections.
- `SixlyUI.txt`, `uwuwareUI.txt`, and `scootUI.txt` informed the restrained particle lifecycle, clear selected-tab affordance, and popup-closing behavior. Their code is not loaded or copied at runtime; the library stays self-contained.

## Version notes

`4.9.2` gives each of the seven pooled comic particles a permanent color and silhouette identity, replaces simple dots/rings with sharper manga sigils and impact marks, keeps each particle in a stable lane when it respawns, and shifts the fixed seven-color spectrum with the selected theme. Example exposes all seven theme moods.

`4.9.0` adds seven distinct pooled particle silhouettes in a seven-color spectrum, moves Log Out to the lower sidebar with the live theme accent, and adds a detected executor/runtime status card. The Example enables all seven particles. It also retains the section container scope and auto-sizing fixes from 4.8.1.

`4.8.1` fixes section controls not being created: divider and callout builders now capture the tab's local section container correctly. Expanded section bodies resize with their contents, and Example requires this version instead of silently running a stale module.

`4.8.0` replaces unsupported decorative icon glyphs with font-safe marks, removes the masthead's rectangular fill, and adds a Thai/English text switch to the showcase. The masthead and intro use a Gothic display face while control text stays in readable sans-serif fonts.

`4.7.0` refines the cut shell silhouette and corner masks, adds the PAYOMBØYZ logo image to the project, and holds the intro reveal briefly for the same logo used by the rest of the window. Example first looks for the local module and logo so it cannot silently run a stale published library; its right column is labeled and filled as a Quick Command Deck.

`4.6.0` replaces the rectangular shell stroke with an asymmetric perimeter that projects past the window, cuts a side notch, and leaves a break for the oversized PAYOMBØYZ masthead. The center burst and repeated slash decorations are gone; a dark ink gradient and sparse halftone marks set the background. Control cards now use readable Gotham text and restrained borders. Particle effects reuse a maximum of four image sprites from one throttled update loop, with no per-sprite slash trails or frame-by-frame FPS counter.

`4.5.0` added a live custom accent picker to Appearance, a branded logo badge in the main header, and mobile scaling for narrow viewports. Display headlines now use Gotham Black/Bold for legibility. The Example provides Thai navigation, live appearance controls, and separate mission, farming, arsenal, and style pages.

`4.4.0` rebuilt the visual surface around chamfered comic panels, double ink/neon borders, diagonal impact marks, alternating panel slant, and quick hover reactions. The content canvas gained a PAYOMBØYZ stamp and impact rays, while the ember particle profile uses faster diagonal slash trails. The Example loader targets the repository's extensionless `PayomboyZ-UI` raw file path.

### Curated icon direction

The visual vocabulary is grouped by job so an anime/action accent does not make navigation harder to scan:

- **Navigation and settings:** `home`, `search`, `settings`, `player`, `visuals`, `help`, `back`, `next`, `close`.
- **Game and action:** `farm`, `combat`, `sword`, `katana`, `shuriken`, `flame`, `quest`, `raid`, `shield`.
- **Hub and status:** `inventory`, `package`, `server`, `trade`, `shop`, `terminal`, `clock`, `chart`, `bell`, `lock`.
- **Brand accents:** `crown`, `spark`, `egg`, `event`, `scroll`, `trophy`.

Use the semantic names anywhere a control accepts `Icon`, or call `Library:RegisterIcon("custom-name", "glyph")` to add a project-specific glyph. This build uses compact text glyphs so the single-file Luau module has no icon download or runtime dependency. The shortlist follows the clean functional shapes in [Lucide](https://icones.js.org/collection/lucide) and [Tabler](https://icones.js.org/collection/tabler), with game motifs checked against [Game Icons](https://icones.js.org/collection/game-icons). Icônes is an icon-set explorer powered by Iconify; each set has its own license, so check the source set before redistributing its actual SVG artwork: [Icônes collections](https://icones.js.org/collection).

`4.2.0` builds on the 4.1 library with seven distinct particle motion profiles, fade-and-respawn lifecycle, animated tab entrance and selection edge, dropdown cleanup on navigation, and an extensible semantic icon set inspired by compact icon affordances in the local UI references. Existing emoji strings, control APIs, theme aliases, search, appearance controls, and single-file loading remain supported.

`4.1.0` adds seven Thai-named theme identities, the PAYOMBØYZ GitHub logo by default, persistent top-bar control search, live gradient recoloring, and compatibility aliases for the former English theme names.
