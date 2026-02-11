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
