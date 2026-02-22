# Config Scripts

## Config (`ReplicatedStorage.Configs.Config`)
```lua
local Config = {
    GridSize = 16,
    DigDamage = 15,
    DigRange = 24,
    GridMinX = -192, GridMaxX = 368,
    GridMinZ = 0, GridMaxZ = 368,
    SurfaceY = 1584, BottomY = -816,
    Layers = {
        {name="Mud",       minY=1120, maxY=1584, material=Enum.Material.Mud,       hp=50,  speed=2, color=Color3.fromRGB(139,90,43)},
        {name="Sand",      minY=640,  maxY=1104, material=Enum.Material.Sand,      hp=80,  speed=3, color=Color3.fromRGB(194,178,128)},
        {name="Limestone", minY=160,  maxY=624,  material=Enum.Material.Limestone, hp=120, speed=4, color=Color3.fromRGB(200,190,160)},
        {name="Ground",    minY=-80,  maxY=144,  material=Enum.Material.Ground,    hp=150, speed=5, color=Color3.fromRGB(115,75,45)},
        {name="Slate",     minY=-320, maxY=-96,  material=Enum.Material.Slate,     hp=200, speed=6, color=Color3.fromRGB(100,100,100)},
        {name="Basalt",    minY=-816, maxY=-336, material=Enum.Material.Basalt,    hp=250, speed=7, color=Color3.fromRGB(60,60,60)},
    },
    Reward = { SpawnChance=0.3, HP=30, Speed=1, DespawnTime=60 },
}
```

## GameEnum (`ReplicatedStorage.Enums.GameEnum`)
```lua
GameEnum.ItemType = { Material=1, Tool=2, Consumable=3, Pet=4 }
GameEnum.Rarity = { Common=1, Uncommon=2, Rare=3, Epic=4, Legendary=5 }
GameEnum.RarityColor = { [1]=Gray, [2]=Green, [3]=Blue, [4]=Purple, [5]=Orange }  -- UIUtils.RarityColors also has Mythical
GameEnum.RarityName = { [1]="Common", [2]="Uncommon", [3]="Rare", [4]="Epic", [5]="Legendary" }
GameEnum.ToolType = { Cangkul="cangkul", Pickaxe="pickaxe", Bag="bag" }
GameEnum.BuffType = { Damage="damage", Speed="speed", Coins="coins", Luck="luck" }
GameEnum.Layer = { Mud, Sand, Limestone, Ground, Slate, Basalt }
GameEnum.BagAction = { SyncBag, ItemPickup, BagFull, SellResult, RequestSync, SellItem, LockItem, SellBatch }
GameEnum.MutationColor = { Racun=Color3(50,200,50), Darah=Color3(200,30,30), SihirHitam=Color3(100,0,180) }
GameEnum.MutationName = { Racun="Racun", Darah="Darah", SihirHitam="Sihir Hitam" }
```

## ItemConfig (`ReplicatedStorage.Configs.ItemConfig`)
```lua
-- type ItemData = { id, name, tp, stackable, rarity, sellPrice:{min,max}, image:string, layer:string }
-- ItemConfig.GetItem(id) -> ItemData?
-- ItemConfig.DefaultBagSize = 20
-- Items (Horror-themed treasures) — each has image="" (placeholder) and layer field:
-- Mud:       3001 Paku Karatan (C, 3-8), 3002 Tulang Tangan (C, 3-8)
-- Sand:      3003 Tulang Iga (C, 5-12), 3004 Tengkorak (C, 5-12)
-- Limestone: 3005 Boneka Sihir Hitam (R, 25-50), 3006 Telur Dipaku (R, 25-50)
-- Ground:    3007 Foto Buram (R, 30-60), 3008 Kertas Ilmu Hitam (E, 75-140)
-- Slate:     3009 Kertas Judi (E, 80-150), 3010 Gigi Emas (L, 250-450)
-- Basalt:    3011 Keranda (L, 350-650)
-- Voxel blocks (image="", layer="N/A"):
-- [2001] Grass Block (C, 1-2), [2002] Sand Block (C, 1-2), [2003] Salt Block (C, 1-3)
-- [2004] Mud Block (C, 1-3), [2005] Slate Block (C, 2-4), [2006] Basalt Block (C, 3-6)
```

## ToolConfig (`ReplicatedStorage.Configs.ToolConfig`)
```lua
-- type ToolData = { id, name, damage, speed, price, toolType, description, rarity, image }
-- ToolConfig.GetTool(id), GetAllTools(), GetToolsByType(type)
-- Tools:
-- [1] Cangkul Karatan: dmg=10, spd=1.0, price=0, Common
-- [2] Cangkul Petani:  dmg=15, spd=1.2, price=100, Uncommon
-- [3] Cangkul Tulang:  dmg=25, spd=1.5, price=500, Rare
```

## PetConfig (`ReplicatedStorage.Configs.PetConfig`)
```lua
-- type PetData = { id, name, buffType, buffValue, price, rarity, description, color, scale, count }
-- PetConfig.GetPet(id), GetAllPets()
-- Pets:
-- [1] Tuyul:   Speed buff 1.2 (+20%), price=200, Common, count=1
-- [2] 3 Tuyul: Speed buff 1.5 (+50%), price=1000, Rare, count=3
```

## AccessoryConfig (`ReplicatedStorage.Configs.AccessoryConfig`)
```lua
-- type AccessoryData = { id, name, setName, slotType, buffs:{damage,speed,luck,mutasi_racun,mutasi_darah,mutasi_sihir_hitam}, price, rarity, description }
-- AccessoryConfig.GetAccessory(id), GetAllAccessories(), GetAccessoryByName(name)
-- AccessoryConfig.SlotTypes = {"Topi", "Muka", "Leher", "Pinggang", "Baju+Celana"}
-- AccessoryConfig.SetBonuses = { Tengkorak={dmg30,racun15}, Vampir={spd30,darah15} }
-- 2 Sets: Tengkorak (multi-stat + Damage focus), Vampir (multi-stat + Speed focus)
-- Each piece buffs: damage, speed, luck, mutasi_racun, mutasi_darah, mutasi_sihir_hitam (in %)
-- Tengkorak per piece: Dmg10, Spd5, Luck5, Racun15, Darah5, SihirHitam10
-- Vampir per piece:    Dmg5, Spd10, Luck5, Racun5, Darah15, SihirHitam10
-- Full set bonus (5 pieces): Tengkorak +30%Dmg +15%Racun | Vampir +30%Spd +15%Darah
-- Items:
-- [1] Baju Tengkorak:    Tengkorak, Baju+Celana, price=300, Uncommon
-- [2] Kalung Tengkorak:  Tengkorak, Leher,       price=200, Common
-- [3] Sabuk Tengkorak:   Tengkorak, Pinggang,    price=250, Common
-- [4] Topi Tengkorak:    Tengkorak, Topi,        price=400, Rare
-- [5] Topeng Tengkorak:  Tengkorak, Muka,        price=350, Uncommon
-- [6] Baju Vampir:       Vampir,    Baju+Celana, price=300, Uncommon
-- [7] Kalung Vampir:     Vampir,    Leher,       price=200, Common
-- [8] Sabuk Vampir:      Vampir,    Pinggang,    price=250, Common
-- [9] Topi Vampir:       Vampir,    Topi,        price=400, Rare
-- [10] Topeng Vampir:    Vampir,    Muka,        price=350, Uncommon
```

---

## DailyRewardConfig (`ReplicatedStorage.Configs.DailyRewardConfig`)
```lua
local DailyRewardConfig = {
    CLAIM_COOLDOWN = 24 * 60 * 60,  -- 24h minimum between claims
    STREAK_WINDOW  = 48 * 60 * 60,  -- >48h = streak resets
    Rewards = {
        { day = 1, coins = 50 },
        { day = 2, coins = 75 },
        { day = 3, coins = 100 },
        { day = 4, coins = 150 },
        { day = 5, coins = 200 },
        { day = 6, coins = 300 },
        { day = 7, coins = 500 },  -- bonus day
    },
}
-- DailyRewardConfig.GetReward(streak) → {day, coins} (cycles 1-7)
```

---

## MutationConfig (`ReplicatedStorage.Configs.MutationConfig`)
```lua
-- MutationConfig.GetMutation(name) -> mutation data or nil
-- 3 mutations, each with: id, name, rarity, chance, sellMultiplier, vfxTemplate, colorOverride, material, reflectance, highlight, sparkle
MutationConfig.Mutations = {
    Racun = { id=1, chance=0.05, sellMultiplier=1.3, vfxTemplate="Racun", color=Green(50,200,50), Uncommon },
    Darah = { id=2, chance=0.03, sellMultiplier=1.5, vfxTemplate="Darah", color=Red(200,30,30), Rare },
    SihirHitam = { id=3, chance=0.02, sellMultiplier=2.0, vfxTemplate="Sihir Hitam", color=Purple(100,0,180), Epic },
}
-- Mutation chance is additive with player accessory buffs (base + AccessoryBuff_mutasi_X / 100)
```

---

## TempaConfig (`ReplicatedStorage.Configs.TempaConfig`)
```lua
-- Forging/upgrade system config
TempaConfig.MaxLevel_Cangkul = 10
TempaConfig.MaxLevel_Accessory = 5
TempaConfig.AccessoryMultiplierPerLevel = 0.3  -- totalBuff = baseBuff * (1 + level * 0.3)

-- Per-level bonus keyed by tool ID: {damage=X, speed=X}
TempaConfig.CangkulBonusPerLevel = {
    [1] = {damage=2, speed=0.05},   -- Cangkul Karatan: max +20dmg, +0.50spd
    [2] = {damage=3, speed=0.06},   -- Cangkul Petani:  max +30dmg, +0.60spd
    [3] = {damage=5, speed=0.08},   -- Cangkul Tulang:  max +50dmg, +0.80spd
}

-- Cangkul tempa costs (keyed by target level 1-10):
-- Lvl1: 50c + 2x Paku Karatan(3001)
-- Lvl2: 100c + 3x Paku Karatan(3001)
-- Lvl3: 200c + 2x Tulang Iga(3003)
-- Lvl4: 350c + 3x Tulang Iga(3003)
-- Lvl5: 600c + 2x Boneka Sihir Hitam(3005)
-- Lvl6: 1000c + 2x Telur Dipaku(3006)
-- Lvl7: 1500c + 2x Foto Buram(3007)
-- Lvl8: 2200c + 1x Kertas Ilmu Hitam(3008)
-- Lvl9: 3000c + 2x Kertas Judi(3009)
-- Lvl10: 4000c + 1x Gigi Emas(3010)

-- Accessory tempa costs (keyed by target level 1-5):
-- Lvl1: 100c + 2x Tulang Tangan(3002)
-- Lvl2: 250c + 2x Tengkorak(3004)
-- Lvl3: 500c + 1x Boneka Sihir Hitam(3005)
-- Lvl4: 1000c + 1x Kertas Ilmu Hitam(3008)
-- Lvl5: 2000c + 1x Kertas Judi(3009)

-- Helper functions:
-- TempaConfig.GetCangkulBonus(toolId, level) -> {damage, speed} (total bonus)
-- TempaConfig.GetAccessoryMultiplier(level) -> number (1.0 at lvl0, 2.5 at lvl5)
-- TempaConfig.GetCangkulCost(level) -> {coins, materials={{id,count},...}}
-- TempaConfig.GetAccessoryCost(level) -> {coins, materials={{id,count},...}}
```
