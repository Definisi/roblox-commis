# Architecture

## Instance Tree (Scripts & Key Objects)

```
ReplicatedStorage
├── Configs/
│   ├── Config [ModuleScript]
│   ├── ItemConfig [ModuleScript]
│   ├── ToolConfig [ModuleScript]
│   ├── PetConfig [ModuleScript]
│   ├── AccessoryConfig [ModuleScript]
│   ├── MutationConfig [ModuleScript]
│   ├── DailyRewardConfig [ModuleScript]
│   └── TempaConfig [ModuleScript]
├── Enums/
│   └── GameEnum [ModuleScript]
├── Libraries/
│   └── Utils [ModuleScript]
├── UIUtils/
│   ├── ChangeColor [ModuleScript]
│   └── RarityColors/ (Color3Values: Common, Uncommon, Rare, Epic, Legendary, Mythical)
├── Events/
│   ├── DigEvent [RemoteEvent]
│   ├── RewardDigEvent [RemoteEvent]
│   ├── GetVoxelHP [RemoteFunction]
│   ├── SellEvent [RemoteEvent]
│   ├── CangkulPurchaseEvent [RemoteEvent]
│   ├── CangkulInventoryEvent [RemoteEvent]
│   ├── PetPurchaseEvent [RemoteEvent]
│   ├── PetInventoryEvent [RemoteEvent]
│   ├── PetEvent [RemoteEvent]
│   ├── AccessoryPurchaseEvent [RemoteEvent]
│   ├── AccessoryInventoryEvent [RemoteEvent]
│   ├── DailyRewardEvent [RemoteEvent]
│   ├── BestiaryEvent [RemoteEvent]
│   ├── TreasureInventoryEvent [RemoteEvent]
│   └── TempaEvent [RemoteEvent]
├── DialogModule [ModuleScript]
└── Topbarplus/
    └── Icon [ModuleScript] (TopbarPlus v3 library)

ServerScriptService
├── Handlers/
│   ├── DigServer [Script]
│   ├── VoxelEngine [ModuleScript]
│   ├── VoxelEngineBootstrap [Script]
│   ├── PlayerSetup [Script]
│   ├── CangkulShopServer [Script]
│   ├── SellServer [Script]
│   ├── PetShopServer [Script]
│   ├── PetDisplayBob [Script]
│   ├── PetSystemBootstrap [Script]
│   ├── PlayerLighting [Script]
│   ├── ToolSlotManager [Script]
│   ├── AccessoryShopServer [Script]
│   ├── ChatCommands [Script] (commented out)
│   ├── DailyRewardServer [Script]
│   ├── TreasureInventoryServer [Script]
│   └── TempaServer [Script]
└── Systems/
    ├── DataSave [ModuleScript]
    ├── InventoryTreasure [ModuleScript]
    └── PetSystem [ModuleScript]

ServerStorage
├── ToolTemplates/
│   ├── Cangkul Karatan/ (Tool + DigClient LocalScript)
│   ├── Cangkul Petani/ (Tool + DigClient LocalScript)
│   └── Cangkul Tulang/ (Tool + DigClient LocalScript)
├── RewardTemplates/
│   ├── Mud/ (Reward1, Reward2, Reward3 Models + Register)
│   ├── Sand/ (Reward1, Reward2, Reward3)
│   ├── Limestone/ (Reward1, Reward2, Reward3)
│   ├── Ground/ (Reward1, Reward2, Reward3)
│   ├── Slate/ (Reward1, Reward2, Reward3)
│   ├── Basalt/ (Reward1, Reward2, Reward3)
│   └── Wood/ (3 Models — legacy, unused)
└── PetTemplates/ (Tuyul, 3 Tuyul MeshParts)

StarterPlayer/StarterPlayerScripts
├── PetClient [LocalScript]
├── ToolSlotClient [LocalScript]
├── CursorClient [LocalScript]
├── PurchasePromptHandler [LocalScript]
├── TopbarClient [LocalScript]
└── ProximityPromptScript [LocalScript]

StarterGui
├── ScreenGui/ → UIController [LocalScript]
├── MobileUI/ (Crosshair, TargetInfo/HPBar/ChargeBar)
├── CurrencyGui/ (CoinsDisplay)
├── TipsGui/ → NotificationClient [LocalScript]
├── CangkulPurchasePrompt/ → PurchasePromptController [LocalScript]
├── PetPurchasePrompt/ → PurchasePromptController [LocalScript]
├── AccessoryPurchasePrompt/ → PurchasePromptController [LocalScript]
├── TeleportGui/ → TeleportClient [LocalScript]
├── MenuGui/ → MenuController [LocalScript] + BestiaryFrame/LocalScript + InventarisFrame/InventarisClient
│   ├── InventarisFrame [Frame] (cloned from PetsFrame style)
│   │   ├── InventarisClient [LocalScript]
│   │   ├── Header (Title "Inventaris (X/20)", Close)
│   │   ├── Container [ScrollingFrame] (UIGridLayout, Template: Arrow/UIStroke/ItemName/PriceTag/MutationBadge/Favorited/Equipped)
│   │   ├── Selected [Frame] (Header Name/Rarity, MutationLabel, PriceLabel, LayerLabel, Buttons: HotbarEquip/HotbarUnequip/Favorite/Sell)
│   │   └── SellAllButton [ImageButton] "Jual Semua"
├── DailyRewardGui/ [ScreenGui] Enabled=false, ResetOnSpawn=false
│   ├── DailyRewardClient [LocalScript]
│   ├── Overlay [TextButton]
│   └── MainFrame [Frame] (Title, StreakInfo, RewardGrid/Day1-7, ClaimButton, CloseButton)
├── HotbarGui/ [ScreenGui] DisplayOrder=10, ResetOnSpawn=false
│   ├── HotbarClient [LocalScript]
│   └── HotbarFrame [Frame]
│       ├── Slot1-Slot9 [ImageButton] (Arrow, RarityStroke, UIGradient, ToolName, SelectedHighlight, MutationBadge)
│       └── InventoryButton [ImageButton] (CountLabel "X/20")
├── TempaGui/ [ScreenGui] Enabled=false, ResetOnSpawn=false
│   ├── TempaClient [LocalScript]
│   └── MainFrame [Frame] (Arrow, Header/Title+Close, TabBar, ItemList, UpgradePanel)
├── dialog/ (NPC dialog system)
└── (deleted: CedGoInventory)

workspace
├── Voxel/ [Folder] (720 pre-placed surface Parts at Y=1584, 36x24 grid with gaps)
├── Rewards/ [Folder] (populated at runtime with reward Models)
├── VFX/
│   ├── Racun [ParticleEmitter] (Poison mutation VFX, green)
│   ├── Darah [Attachment] (Blood mutation VFX, red, 2 emitters)
│   └── SihirHitam [ParticleEmitter] (Black Magic mutation VFX, purple)
├── Assets/
│   ├── Cangkul/ (display models with Register + ProximityPrompt "CangkulPurchase")
│   ├── Pets/ (display models with Register + ProximityPrompt "PetPurchase")
│   └── Cursor (3D cursor part)
├── Equip/
│   ├── Tengkorak (rig display, Torso ProximityPrompt "AccessoryPurchase" ObjectText="Baju Tengkorak")
│   ├── Vampire (rig display, Torso ProximityPrompt "AccessoryPurchase" ObjectText="Baju Vampir")
│   ├── 8x floating BaseParts (Kalung/Sabuk/Topi/Topeng per set, each with ProximityPrompt)
│   ├── Set Tengkorak/ (Folder in equip area)
│   └── Set Vampir/ (Folder in equip area)
├── Anvil [Part] (ProximityPrompt "TempaPurchase", ActionText="Tempa", ObjectText="Pandai Besi")
└── Location/Surface/Spawn (teleport spawn part)
```

## Game Flow

```
STARTUP:
  VoxelEngineBootstrap → VoxelEngine.GenerateSurface()
    → Process pre-placed Parts in workspace.Voxel at Y=1584
    → Fill terrain, make Parts invisible, generate depth buffer (+3 below)
  PlayerSetup → DataSave.Load(), create leaderstats → ORDERED LOADER:
    Phase 1: signal OrderLoad_Cangkul → CangkulShopServer gives equipped tool → OrderDone_Cangkul
    Phase 2: signal OrderLoad_Pet → PetShopServer applies buff + spawns pet → OrderDone_Pet
    Phase 3: signal OrderLoad_Accessory → AccessoryShopServer applies buffs + visuals → OrderDone_Accessory
    Phase 4: InventoryTreasure.RestoreTools() → hotbar treasures to backpack
      → PlayerSetup fires TreasureInventoryEvent "SyncBag" (initial sync)
  Respawn: PlayerSetup fires SyncBag again via CharacterAdded
  DailyRewardServer → wait DataSave loaded → compute reward state → FireClient "ShowReward"

DIGGING TERRAIN:
  DigClient (in tool) → Heartbeat raycast → hit invisible Part in Voxel/
    → Hold click → charge bar fills (cycle = voxelSpeed / (toolSpeed * petBuff * (1 + accSpeedBuff)))
    → Full charge → DigEvent:FireServer(partPos, partSize)
  DigServer → validate range, timing → apply damage to hpTracker[key]
    → HP <= 0:
      → clearTerrain (WriteVoxels Air)
      → VoxelEngine.RemovePosition() + Part:Destroy()
      → VoxelEngine.GenerateBelow() (spawn +3 below for ALL columns)
      → spawnChance = 30% * (1 + luckBuff/100) → spawnReward(pos, layerName, player)
        → pickReward() by weighted chance
        → Roll mutations: Racun(5%+buff), Darah(3%+buff), SihirHitam(2%+buff) — first hit wins
    → HP > 0: FireClient(hp, maxHP, speed)

DIGGING REWARDS:
  DigClient → raycast hits Block in reward Model → RewardDigEvent:FireServer(hitPos)
  DigServer → findRewardNear() → apply damage
    → HP <= 0: InventoryTreasure.AddTreasure() → reward:Destroy()
      → Checks bag size (max 20), auto-assigns hotbar slot (2-9)
      → Creates Tool only for hotbar items, fires ItemPickup to client

SELLING:
  SellServer → SellEvent "SellAll"
    → InventoryTreasure.RemoveAllNonFavorited() (preserves favorites)
    → Add coins to data + leaderstats
  TreasureInventoryServer → "SellItem" / "SellBatch" for single/batch sell

CUSTOM INVENTORY + HOTBAR:
  HotbarClient: disables default Backpack, renders slots 1-9 (slot 1=Cangkul, 2-9=treasures)
    → Number keys 1-9 or click to equip/unequip
    → Slots only visible when occupied (empty slots hidden)
    → InventoryButton toggles InventarisFrame in MenuGui
  InventarisClient (in MenuGui.InventarisFrame): grid view of all bag items, sort by rarity/price
    → Sell single/batch, favorite toggle, hotbar assign/remove
    → All actions via TreasureInventoryEvent → TreasureInventoryServer
    → UI cloned from PetsFrame style (same Arrow pattern, UIStroke, colors)

SHOPS:
  ProximityPrompt triggers → PurchasePromptHandler routes to UI
  CangkulShopServer / PetShopServer / AccessoryShopServer handle purchase + equip logic
  MenuController shows inventory frames for cangkul/pet/accessory management

ACCESSORIES:
  ProximityPrompt "AccessoryPurchase" → PurchasePromptHandler → AccessoryPurchasePrompt
  AccessoryShopServer: purchase, equip/unequip (1 per slot, auto-unequip old)
  6 buff attributes: AccessoryBuff_damage/_speed/_luck/_mutasi_racun/_mutasi_darah/_mutasi_sihir_hitam
  Full set bonus: 5 pieces of same set → add SetBonuses (Tengkorak: +30%Dmg +15%Racun, Vampir: +30%Spd +15%Darah)
  Data migration: old slot keys (Outfit/Kalung/etc) auto-migrated to new (Topi/Muka/Leher/Pinggang/Baju+Celana)
  Visuals: clone Shirt/Pants/Accessory from ServerStorage.AccessoryTemplates["Set X"] onto character
  Reapply on respawn via CharacterAdded

TEMPA (FORGING/UPGRADE):
  ProximityPrompt "TempaPurchase" on workspace.Anvil → PurchasePromptHandler → opens TempaGui + RequestSync
  TempaClient: two tabs (Cangkul / Aksesoris), shows owned items with tempa level badges
    → Select item → shows stats preview (current → next), cost (coins + materials), availability
    → Tempa button fires TempaCangkul/TempaAccessory to server
  TempaServer: validates ownership + level + coins + materials → deducts → increments level
    → Cangkul: re-gives equipped tool with bonus stats (baseDmg + perLevel*level, baseSpd + perLevel*level)
    → Accessory: sets TempaChanged attribute → AccessoryShopServer recalculates buffs (baseBuff * (1 + level*0.3))
    → Materials consumed from treasure bag (InventoryTreasure.RemoveTreasure)
  Max levels: Cangkul +10, Accessory +5
  DataSave: TempaLevels_Cangkul = {["toolId"] = level}, TempaLevels_Accessory = {["accId"] = level}
```

## Event Protocol

| Event | Direction | Args |
|-------|-----------|------|
| DigEvent | Client→Server | partPos: Vector3, partSize: Vector3 |
| DigEvent | Server→Client | currentHP, maxHP, speed |
| RewardDigEvent | Client→Server | hitPos: Vector3 |
| RewardDigEvent | Server→Client | currentHP, maxHP, speed |
| GetVoxelHP | Client→Server (invoke) | partPos: Vector3 → returns hp, maxHP, speed |
| SellEvent | Client→Server | action: "SellAll" or "SellEquipped" |
| SellEvent | Server→Client | "SellResult", {success, coins, count, totalCoins} |
| CangkulPurchaseEvent | Client→Server | "Purchase", cangkulName |
| CangkulPurchaseEvent | Server→Client | "PurchaseResult", {success, reason?, toolName?, newCoins?} |
| CangkulInventoryEvent | Both | "RequestSync" / "Sync" / "Equip" |
| PetPurchaseEvent | Client→Server | "Purchase", petName |
| PetPurchaseEvent | Server→Client | "PurchaseResult", {success, reason?, petName?, newCoins?} |
| PetInventoryEvent | Both | "RequestSync" / "Sync" / "Equip" / "Unequip" |
| PetEvent | Both | "PetSpawned" / "EquipPet" / "UnequipPet" / "BuyPet" |
| AccessoryPurchaseEvent | Client→Server | "Purchase", accName |
| AccessoryPurchaseEvent | Server→Client | "PurchaseResult", {success, reason?, accName?, newCoins?} |
| AccessoryInventoryEvent | Both | "RequestSync" / "Sync" / "Equip" / "Unequip" |
| DailyRewardEvent | Server→Client | "ShowReward", {streak, cycleDay, coins, canClaim, currentStreak} |
| DailyRewardEvent | Client→Server | "Claim" |
| DailyRewardEvent | Server→Client | "ClaimResult", {success, coins?, newStreak?} |
| BestiaryEvent | Client→Server | "RequestSync" |
| BestiaryEvent | Server→Client | "Sync", {discoveredItems: {number}} |
| TreasureInventoryEvent | Client→Server | "RequestSync" |
| TreasureInventoryEvent | Client→Server | "SellItem", onlyID |
| TreasureInventoryEvent | Client→Server | "SellBatch", {onlyID, ...} |
| TreasureInventoryEvent | Client→Server | "LockItem", onlyID |
| TreasureInventoryEvent | Client→Server | "SetHotbar", slot, onlyID |
| TreasureInventoryEvent | Client→Server | "RemoveHotbar", onlyID |
| TreasureInventoryEvent | Server→Client | "SyncBag", syncData |
| TreasureInventoryEvent | Server→Client | "ItemPickup", entry |
| TreasureInventoryEvent | Server→Client | "BagFull" |
| TreasureInventoryEvent | Server→Client | "SellResult", {success, coins, count, totalCoins} |
| TreasureInventoryEvent | Server→Client | "LockItem", {onlyID, favorited} |
| TempaEvent | Client→Server | "RequestSync" |
| TempaEvent | Server→Client | "Sync", {cangkulLevels, accessoryLevels, coins, inventoryCangkul, inventoryAccessory, materialCounts} |
| TempaEvent | Client→Server | "TempaCangkul", toolId |
| TempaEvent | Client→Server | "TempaAccessory", accId |
| TempaEvent | Server→Client | "TempaResult", {success, reason?, type?, id?, name?, newLevel?, newCoins?} |

## Grid System
- Cell size: 16 studs
- X range: -192 to 368, Z range: 0 to 368
- Y range: 1584 (surface) to -816 (bottom)
- Surface Y = 1584, Bottom Y = -816
- Depth buffer: 3 rows below lowest destroyed block (generated for ALL columns)

## Layer Config (top → bottom, 6 layers)
| Layer | Y Range | Material | HP | Speed | Rewards |
|-------|---------|----------|----|-------|---------|
| Mud | 1584→1120 | Mud | 50 | 2 | Grass Clump, Flower, Rare Seed |
| Sand | 1104→640 | Sand | 80 | 3 | Stone, Sandstone, Fossil |
| Limestone | 624→160 | Limestone | 120 | 4 | Salt Crystal, Quartz, Amethyst |
| Ground | 144→(-80) | Ground | 150 | 5 | Mud Chunk, Clay, Ancient Bone |
| Slate | (-96)→(-320) | Slate | 200 | 6 | Iron Ore, Silver Ore, Gold Ore |
| Basalt | (-336)→(-816) | Basalt | 250 | 7 | Basalt Shard, Obsidian, Diamond |
