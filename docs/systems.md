# Systems (Server ModuleScripts)

## DataSave (`ServerScriptService.Systems.DataSave`)
DataStore-based persistence. Store name: `"PlayerData_v1"`, auto-save every 60s, 3 retries.

**Types:**
```lua
type TreasureEntry = {
    onlyID: number,
    id: number,
    name: string,
    rarity: number,
    sellPrice: number,
    mutation: string?,  -- "Darkness" or nil
}

type PlayerData = {
    onlyID: number,        -- auto-increment counter for unique IDs
    Coins: number,
    EquippedCangkul: number,
    InventoryCangkul: {number},
    EquippedPet: number?,
    InventoryPet: {number},
    MaxPetEquip: number,   -- default 1
    MaxPetStorage: number, -- default 25
    Treasures: {TreasureEntry},
    Favorites: {number},             -- set of onlyIDs that are favorited
    HotbarSlots: {[number]: number}, -- slot (2-9) → onlyID mapping
    DailyReward_LastClaim: number,   -- os.time() of last claim (0 = never)
    DailyReward_Streak: number,      -- consecutive day count
    DailyReward_TotalClaims: number, -- lifetime total claims
    InventoryAccessory: {number},        -- owned accessory IDs
    EquippedAccessories: {[string]: number}, -- slotType → accessory ID
    DiscoveredItems: {number},           -- treasure item IDs player has found (for Bestiary)
}
```

**API:**
- `Load(player)` → bool — load from DataStore, merge with defaults
- `Save(player | userId)` → bool
- `GetData(player)` → PlayerData? — returns mutable reference
- `IsLoaded(player)` → bool
- `SetField(player, field, value)` / `GetField(player, field)`
- `Cleanup(player)` — save + remove from memory
- `StartAutoSave()` — spawns auto-save loop
- BindToClose saves all on shutdown

---

## InventoryTreasure (`ServerScriptService.Systems.InventoryTreasure`)
Requires: DataSave, ItemConfig, MutationConfig.

Manages treasure items in player's bag (max 20 from ItemConfig.DefaultBagSize). Tools are only created for items assigned to hotbar slots.

**API:**
- `AddTreasure(player, itemId, rewardModel)` → entry
  - Checks bag size (max 20) — if full, fires TreasureInventoryEvent "BagFull" and returns nil
  - Generates unique onlyID
  - Gets name/rarity from rewardModel attributes or ItemConfig
  - Computes sellPrice from ItemConfig (random in range min..max)
  - Checks rewardModel Mutation attribute → applies MutationConfig.sellMultiplier
  - Stores entry with mutation field
  - **Marks item as discovered**: adds itemId to `data.DiscoveredItems` if not already present (for Bestiary)
  - **Auto-assigns hotbar**: assigns to first empty hotbar slot (2-9), creates Tool only if assigned
  - Fires TreasureInventoryEvent "ItemPickup" to client
- `RemoveTreasure(player, onlyID)` → bool — removes single entry + destroys Tool if in hotbar
- `RemoveAllNonFavorited(player)` → totalCoins, soldCount — sells all non-favorited items, removes from hotbar
- `RestoreTools(player)` — recreate Tools only for items in HotbarSlots (on spawn/respawn)
- `GetTreasures(player)` → {TreasureEntry}
- `GetSyncData(player)` → {treasures, favorites, hotbarSlots, bagSize} — full state for client sync
- `ToggleFavorite(player, onlyID)` → bool — toggle favorite status in data.Favorites
- `IsFavorited(player, onlyID)` → bool
- `SetHotbarSlot(player, slot, onlyID)` — assign item to hotbar slot, creates Tool
- `RemoveFromHotbar(player, onlyID)` — remove item from hotbar, destroys Tool

**Tool attributes set:** IsTreasure, OnlyID, ItemID, ItemName, Rarity, SellPrice, Mutation

---

## PetSystem (`ServerScriptService.Systems.PetSystem`)
Requires: DataSave, PetConfig. Manages active pet models following players.

**State:**
- `activePets[player]` = { folder: Folder, models: {BasePart} }
- `PET_OFFSETS` = 3 positions (right, left, top) for up to 3 pets
- PetTemplates has "Tuyul" and "3 Tuyul" (cloned from Tuyul). PetSystem looks up by `config.name`.

**API:**
- `SpawnPet(player, petId)` — removes old, creates folder `"PlayerName_Pets"` in workspace, clones from ServerStorage.PetTemplates, sets offset attributes, applies buff, fires PetEvent
- `RemovePet(player)` — destroys folder, clears buff attributes

**Auto-behavior:**
- CharacterAdded: re-spawn equipped pet
- PlayerRemoving: remove pet
- PetEvent.OnServerEvent: handles EquipPet/UnequipPet/BuyPet (legacy path, PetShopServer is primary)
