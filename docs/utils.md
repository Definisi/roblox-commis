# Utility Modules

## Utils (`ReplicatedStorage.Libraries.Utils`)
General utility functions used across client and server.

```lua
Utils.DeepCopy(original)              -- recursive table deep copy
Utils.ShallowCopy(original)           -- single-level table copy
Utils.FormatNumber(num)               -- 1000 → "1,000" (comma separated)
Utils.FormatNumberAbbreviated(number, decimals)  -- 1500 → "1.5K", 1000000 → "1M"
Utils.Clamp(value, min, max)          -- math.max(min, math.min(max, value))
Utils.Lerp(a, b, t)                   -- a + (b - a) * t
Utils.IsInRange(pos1, pos2, range)    -- magnitude check
Utils.GetPosKey(position)             -- "X,Y,Z" string key for dictionaries
Utils.SafeRequire(module)             -- pcall-wrapped require
Utils.WaitForChildTimeout(parent, name, timeout)  -- polling WaitForChild
```

Suffixes: "", K, M, B, T, Qa, Qi, Sx, Sp, Oc, No, Dc... (up to Tvg)

---

## ChangeColor (`ReplicatedStorage.UIUtils.ChangeColor`)
UI rarity color helper. Uses `RarityColors` folder (sibling Color3Values).

```lua
ChangeColor.ChangeFrameColor(frame, rarityName)    -- set BackgroundColor3, UIStroke, Arrow color
ChangeColor.ChangeRarityColor(frame, rarityName)    -- set bg, stroke, child TextLabel color
ChangeColor.GetRarityColor(rarityName) → Color3?    -- lookup color value
```

RarityColors folder contains Color3Value instances named: Common, Uncommon, Rare, Epic, Legendary, Mythical.

---

## ViewportIcon (`ReplicatedStorage.UIUtils.ViewportIcon`)
3D model preview renderer for ViewportFrames. Clones models from ReplicatedStorage.IconModels into ViewportFrame with auto-fitted camera.

```lua
ViewportIcon.Setup(viewportFrame, modelName)  -- Clone model, create Camera, auto-fit view
ViewportIcon.Clear(viewportFrame)             -- Remove all children from ViewportFrame
```

**Setup behavior:**
- Searches IconModels/Treasures/ first (item IDs "3001"-"3011"), then IconModels/Cangkuls/ (tool IDs "1"-"3")
- Clones model into ViewportFrame (removes existing children first)
- Creates WorldModel container + Camera with 50 FOV
- Auto-fits camera distance based on model bounding box extents: `distance = maxExtent * 1.8`
- Camera angle: -25deg pitch (looking down), 225deg yaw (angled view from front-left)
- LightDirection: (-1,-1,-1).Unit for consistent shading
- If model not found, warns to console

**Model preparation (IconModels):**
- Treasure models: stripped of Register scripts, Terrain, Block parts (only visual MeshParts)
- Cangkul models: stripped of DigClient LocalScript (only MeshParts + welds)
- Models named by ID string (not display name): "3001" not "Grass Clump", "1" not "Cangkul Karatan"

**Usage locations:**
- HotbarClient: Slot 1 (Cangkul) + Slots 2-9 (treasures)
- InventarisClient: Grid slots for all bag items
- Bestiary: Grid slots + Selected panel
- MenuController: Cangkul inventory slots
