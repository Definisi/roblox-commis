# Roblox Digging Game - Claude Code Instructions

## Project Overview
Terrain-based digging game built in Roblox Studio. Players dig voxel terrain with Cangkul tools, collect treasure rewards, equip pets for speed buffs, and sell loot for coins.

## How to Work with This Project
- All game code lives **inside Roblox Studio** (not on disk). Use `mcp__Roblox_Studio__run_code` to read/write script Source.
- Documentation lives in `docs/` folder on disk. Always keep docs in sync after code changes.
- Memory files at `C:\Users\Admin\.claude\projects\D--Desktop-RobloxGame-DIg\memory\` mirror `docs/` and are auto-loaded.

## Documentation Structure
All docs are in `D:\Desktop\RobloxGame\DIg\docs\`:

| File | Contents |
|------|----------|
| `main.md` | Project index, quick reference, key facts |
| `architecture.md` | Instance tree, game flow, events, grid system, layer config |
| `configs.md` | Config, ItemConfig, ToolConfig, PetConfig, MutationConfig, GameEnum |
| `server-scripts.md` | All server Scripts documented |
| `systems.md` | DataSave, InventoryTreasure, PetSystem |
| `client-scripts.md` | DigClient, PetClient, ToolSlotClient, CursorClient, PurchasePromptHandler |
| `ui-scripts.md` | UIController, NotificationClient, MenuController, purchase prompts, teleport, dialog |
| `rewards.md` | Loot tables per layer, mutation system, sell price formula |
| `utils.md` | Utils library, ChangeColor module |

## Auto-Update Documentation Rules
After ANY code change in Roblox Studio, update the relevant docs:

1. **Identify affected docs** — check which doc files describe the modified script/system
2. **Update the doc file** in `docs/` with the new behavior, parameters, or flow
3. **Update `docs/main.md`** if the change affects key architecture facts or adds new scripts/systems
4. **Sync to memory** — copy updated docs to memory folder:
   - `docs/main.md` content goes into `memory/MEMORY.md` (keep under 200 lines)
   - Other doc files: copy to `memory/<filename>.md`

### What to Document
- New scripts or modules: add to relevant doc file + architecture.md instance tree
- New RemoteEvents/Functions: add to architecture.md event protocol table
- Config changes: update configs.md
- New reward items/layers: update rewards.md + configs.md + architecture.md layer table
- New UI elements: update ui-scripts.md
- Bug fixes that change behavior: update the affected system's doc
- New features: document the full flow (server + client + UI)

### What NOT to Document
- Internal variable renames or minor refactors that don't change behavior
- Temporary debug code

## Code Editing via MCP
When modifying Roblox scripts through MCP:

```lua
-- Reading a script's source:
local src = game.ServerScriptService.Handlers.DigServer.Source
print(src)

-- Writing/replacing source (use string.find + string.sub, NOT gsub):
local src = script_path.Source
local pos = string.find(src, "target_string", 1, true)  -- plain find
if pos then
    src = string.sub(src, 1, pos - 1) .. "replacement" .. string.sub(src, pos + #"target_string")
    script_path.Source = src
end
```

**Key rules:**
- Use `string.find(src, pattern, 1, true)` with plain=true for reliable matching
- Never use `gsub` for script Source edits (special chars and line endings break it)
- `require` cache persists in edit mode — changes only take effect on playtest
- Always verify edits by reading back the Source after modification

## Key Technical Constraints
- Never send Part instances through RemoteEvents (unreliable with StreamingEnabled). Send Vector3.
- Terrain `FillBlock(CFrame, Size, Air)` leaves remnants. Use `WriteVoxels` for clean clearing.
- Always check for multiple Roblox Studio instances — MCP may connect to wrong one.
- VoxelEngine is a ModuleScript required by both VoxelEngineBootstrap and DigServer.

## Grid System
- Cell size: 16 studs
- X range: -192 to 368, Z range: 0 to 368
- Y range: 1584 (surface) to -816 (bottom)
- 6 layers: Mud, Sand, Limestone, Ground, Slate, Basalt (layer name = material name)
