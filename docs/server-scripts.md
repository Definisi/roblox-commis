# Server Scripts

## DigServer (`ServerScriptService.Handlers.DigServer`) [Script]
Main digging handler. Requires: Config, ItemConfig, MutationConfig, VoxelEngine, InventoryTreasure.

**Key data:**
- `hpTracker[posKey]` = {hp, maxHP, speed} — server-side voxel HP
- `lastDigTime[player][posKey]` — timing verification
- `lastRewardDigTime[player][rewardKey]` — reward timing verification
- `rewardsFolder` = workspace.Rewards
- `rewardTemplates[layerName]` = array of {model, id, name, rarity, chance}

**Key functions:**
- `getLayerHP(material)` / `getLayerSpeed(material)` / `getLayerName(y)` — layer lookups
- `pickReward(layerName)` — weighted random from templates (chance out of 100)
- `clearTerrain(center, size)` — WriteVoxels Air
- `spawnReward(position, layerName, player)` — clone reward at destroyed voxel position, set attributes (HP/MaxHP/Speed/IsReward/ItemID/ItemName/Rarity), roll mutations (Racun 5%/Darah 3%/SihirHitam 2% + player accessory buffs → VFX from workspace["Efek Mutasi"] folder, clones individual ParticleEmitters + color/highlight/sparkle), auto-despawn after DespawnTime. Luck buff increases spawn chance multiplicatively.
- `erodeTerrain(position, size)` — erodes terrain edges around destroyed voxel (directional, fixed startFromEnd logic for correct erosion direction)
- `findRewardNear(hitPos)` — finds reward model within GRID distance
- `findVoxelPart(pos)` — finds closest Part in workspace.Voxel
- `getToolSpeed(character)` — reads tool Speed attribute

**DigEvent handler (terrain):**
1. Validate: character, range, Vector3 types
2. Find voxel Part, init hpTracker if needed
3. Timing check: elapsed >= expectedCycle * 0.8
4. Apply damage (DigDamage * PetBuff_damage * (1 + AccessoryBuff_damage/100), or tool Damage * PetBuff_damage * (1 + AccessoryBuff_damage/100))
5. HP <= 0: clearTerrain, erodeTerrain, VoxelEngine.RemovePosition, Part:Destroy, GenerateBelow, spawnChance = `Config.Reward.SpawnChance * PetBuff_luck * (1 + AccessoryBuff_luck/100) * ServerLuckMultiplier` → spawnReward(pos, layerName, player). PetBuff_luck comes from pet buffLuck field (set by PetShopServer.applyPetBuff).
6. HP > 0: FireClient(hp, maxHP, speed)

**RewardDigEvent handler:**
1. Validate, find reward near hitPos
2. Timing check same as terrain
3. Apply damage
4. HP <= 0: InventoryTreasure.AddTreasure, reward:Destroy
5. HP > 0: update HP attribute, FireClient

**GetVoxelHP (RemoteFunction):**
Returns (hp, maxHP, speed) for reward or voxel at position.

---

## VoxelEngine (`ServerScriptService.Handlers.VoxelEngine`) [ModuleScript]
Requires: Config. Manages voxel grid generation.

**State:**
- `generated[posKey]` = true or "destroyed" — tracks all generated positions
- `surfaceColumns[colKey]` = true — which X,Z columns exist
- `allColumns` = array of {x, z} — all column positions
- `alivePerY[flooredY]` = count — alive voxels per Y-level (for cleanup)
- `DEPTH_BUFFER = 3` — rows to pre-generate below
- `CLEANUP_INTERVAL = 120` — seconds between destroyed entry cleanup

**Functions:**
- `GenerateSurface()` — Process pre-placed Parts in workspace.Voxel: fill terrain, make invisible guides, build column list, generate depth buffer (3 rows below surface for each column)
- `GenerateBelow(position)` — When block destroyed at Y: generate +3 below for ALL columns (not just the destroyed one)
- `RemovePosition(position)` — Mark as "destroyed", decrement alivePerY
- `createVoxel(x, y, z)` — Internal: create Part + terrain fill if not already generated, increment alivePerY

**Cleanup system (task.spawn loop every 120s):**
- Find highest alive Y-level (`maxAliveY`)
- Remove "destroyed" entries from `generated` where Y > maxAliveY (safe because blocks only generate downward)

---

## VoxelEngineBootstrap (`ServerScriptService.Handlers.VoxelEngineBootstrap`) [Script]
```lua
local VoxelEngine = require(script.Parent.VoxelEngine)
VoxelEngine.GenerateSurface()
```

---

## PlayerSetup (`ServerScriptService.Handlers.PlayerSetup`) [Script]
Requires: DataSave, InventoryTreasure.

**Ordered Loader** — orchestrates player initialization in sequence:
1. Create leaderstats/Coins, DataSave.Load()
2. Phase 1 (Cangkul): set `OrderLoad_Cangkul` attr → wait for `OrderDone_Cangkul`
3. Phase 2 (Pet): set `OrderLoad_Pet` attr → wait for `OrderDone_Pet`
4. Phase 3 (Accessory): set `OrderLoad_Accessory` attr → wait for `OrderDone_Accessory`
5. Phase 4 (Treasure): wait for character, InventoryTreasure.RestoreTools(), fire TreasureInventoryEvent "SyncBag" (initial sync)
- Each phase has 15s timeout protection
- Subsequent respawns: restore treasures + fire SyncBag via CharacterAdded
- PlayerRemoving: DataSave.Cleanup()
- Starts DataSave.StartAutoSave()
- **BestiaryEvent handler**: on "RequestSync" → returns `{discoveredItems}` from player's `data.DiscoveredItems`

---

## CangkulShopServer (`ServerScriptService.Handlers.CangkulShopServer`) [Script]
Requires: DataSave, ToolConfig, GameEnum, TempaConfig.

- Loads Register ModuleScripts from `workspace.Assets.Cangkul` display models
- `ownsCangkul(data, id)` — check InventoryCangkul
- `giveTool(player, toolData)` — clone from ServerStorage.ToolTemplates, remove old cangkul, set attributes (ToolId, ToolType, Damage, Speed, TempaLevel, Owner). **Applies tempa bonus**: reads `TempaLevels_Cangkul[toolId]`, adds `TempaConfig.GetCangkulBonus()` to base damage/speed.
- CangkulPurchaseEvent: validate, deduct coins, add to InventoryCangkul, auto-equip, sync
- CangkulInventoryEvent: "RequestSync" → send inventory, "Equip" → switch tool + FireClient "EquipResult" {name}
- On join: waits for `OrderLoad_Cangkul` signal → give equipped cangkul → sets `OrderDone_Cangkul`. Re-equip on respawn

---

## SellServer (`ServerScriptService.Handlers.SellServer`) [Script]
Requires: DataSave, InventoryTreasure, ItemConfig.

- SellEvent "SellAll": calls `RemoveAllNonFavorited` (preserves favorited items), add coins, FireClient result, fires TreasureInventoryEvent "SyncBag" to update hotbar
- SellEvent "SellEquipped": removes equipped treasure tool, add coins, FireClient result, fires TreasureInventoryEvent "SyncBag"
- 1s cooldown per player

---

## PetShopServer (`ServerScriptService.Handlers.PetShopServer`) [Script]
Requires: DataSave, PetConfig, GameEnum, PetSystem.

Multi-pet equip system. Supports up to 3 equipped pets with additive buff stacking.

**Key functions:**
- `migrateEquippedPet(data)` — migration: converts old `data.EquippedPet` (single number) to `data.EquippedPets` (array)
- `applyPetBuff(player, equippedPets)` — reads `buffSpeed`, `buffDamage`, `buffLuck` fields from PetConfig for each equipped pet (each field is a multiplier, e.g. 1.2 = +20%). Additive stacking: sums (value - 1) for each pet, final buff = 1 + sum. Sets `PetBuff_damage`, `PetBuff_speed`, and `PetBuff_luck` attributes on the player. PetBuff_luck is used by DigServer in spawnChance calculation.
- `syncInventory(player)` — sends {inventory, equipped, maxEquip, maxStorage} to client via PetInventoryEvent "Sync". equipped is an array of pet IDs.

**Handlers:**
- PetPurchaseEvent "Purchase": validate coins, deduct, add to InventoryPet, auto-equip if slots available
- PetInventoryEvent "RequestSync": call syncInventory
- PetInventoryEvent "Equip": validate no duplicates, check slot count < maxEquip, append to EquippedPets, call applyPetBuff, spawn pets, FireClient "EquipResult" {name}
- PetInventoryEvent "Unequip": takes petId parameter, removes from EquippedPets array, call applyPetBuff, remove pets, FireClient "UnequipResult" {name}
- PetInventoryEvent "SlotFull": sent to client when all slots occupied (during equip attempt)
- On join: waits for `OrderLoad_Pet` signal → migrate old data → apply buffs + spawn pets → sets `OrderDone_Pet`
- Loads Register from workspace.Assets.Pets display models

---

## AccessoryShopServer (`ServerScriptService.Handlers.AccessoryShopServer`) [Script]
Requires: DataSave, AccessoryConfig, GameEnum, TempaConfig.

- `BUFF_KEYS` = {"damage", "speed", "luck", "mutasi_racun", "mutasi_darah", "mutasi_sihir_hitam"}
- `ownsAccessory(data, accId)` — check InventoryAccessory
- `applyAccessoryBuffs(player)` — sum all 6 buff types from equipped accessories (in %), **applies tempa multiplier** per piece: `buffValue = baseBuff * (1 + tempaLevel * 0.3)`. Check full set bonus (5 pieces of same set → add SetBonuses), set 6 player attributes:
  - `AccessoryBuff_damage`, `AccessoryBuff_speed`, `AccessoryBuff_luck`
  - `AccessoryBuff_mutasi_racun`, `AccessoryBuff_mutasi_darah`, `AccessoryBuff_mutasi_sihir_hitam`
- `migrateEquippedSlots(data)` — on join, if old slot keys found (Outfit/Kalung/etc), rebuild EquippedAccessories by looking up each ID's current slotType from config
- `applyAccessoryVisuals(player)` — clear old visuals (tagged `IsAccessoryEquip`), clone from `ServerStorage.AccessoryTemplates["Set X"]`:
  - Baju+Celana slot: apply Shirt/Pants templates to character's existing clothing
  - Other slots: clone Accessory, Humanoid:AddAccessory()
- `clearAccessoryVisuals(character)` — remove all items tagged `IsAccessoryEquip`
- `syncInventory(player)` — send {inventory, equipped} to client

**Handlers:**
- AccessoryPurchaseEvent "Purchase": validate, deduct coins, add to InventoryAccessory, auto-equip if slot empty
- AccessoryInventoryEvent "RequestSync"/"Equip"/"Unequip": manage equipped slots (1 per slot, auto-unequip old)
- PlayerAdded: waits for `OrderLoad_Accessory` signal → migrate old slots → apply buffs + visuals → sets `OrderDone_Accessory`. CharacterAdded → reapply visuals on respawn. **TempaChanged listener**: recalculates buffs when `player:GetAttribute("TempaChanged")` changes (set by TempaServer on accessory tempa)

---

## PetDisplayBob (`ServerScriptService.Handlers.PetDisplayBob`) [Script]
Animates pet display models in workspace.Assets.Pets with bobbing (sin wave) + spinning (continuous rotation). Uses RunService.Heartbeat.

---

## PetSystemBootstrap (`ServerScriptService.Handlers.PetSystemBootstrap`) [Script]
```lua
require(ServerScriptService.Systems.PetSystem)
```

---

## PlayerLighting (`ServerScriptService.Handlers.PlayerLighting`) [Script]
Adds PointLight to HumanoidRootPart on spawn. Range=16, Brightness=2, warm white, shadows enabled.

---

## ToolSlotManager (`ServerScriptService.Handlers.ToolSlotManager`) [Script]
Prevents tool dropping. Marks tools with Owner attribute, CanBeDropped=false. Monitors workspace.ChildAdded to return dropped tools to owner's Backpack.

---

## ChatCommands (`ServerScriptService.Handlers.ChatCommands`) [Script]
**Currently commented out** (entire script wrapped in `--[[ ]]`). Contains chat-based commands for /inv, /sell-all, /shop, /buy-tool, /pets, etc.

---

## DailyRewardServer (`ServerScriptService.Handlers.DailyRewardServer`) [Script]
Requires: DataSave, DailyRewardConfig.

Daily login reward system with 7-day cycling streak.

**Key functions:**
- `getRewardState(data)` → {canClaim, streak, cycleDay, coins, currentStreak}
  - elapsed = os.time() - lastClaim
  - If elapsed > 48h (STREAK_WINDOW): streak resets to 0
  - canClaim = (lastClaim == 0) or (elapsed >= 24h CLAIM_COOLDOWN)
  - nextStreak = streak + 1, cycleDay = ((nextStreak-1) % 7) + 1

**PlayerAdded flow:**
1. Wait for DataSave.IsLoaded
2. Compute reward state
3. FireClient "ShowReward" with {streak, cycleDay, coins, canClaim, currentStreak}

**OnServerEvent "RequestShow":**
- Re-computes reward state, FireClient "ShowReward" (allows topbar button to reopen popup)

**OnServerEvent "Claim":**
1. Re-check getRewardState (anti-exploit)
2. If canClaim: update DailyReward_LastClaim/Streak/TotalClaims, add coins, update leaderstats
3. FireClient "ClaimResult" {success, coins, newStreak}

---

## TreasureInventoryServer (`ServerScriptService.Handlers.TreasureInventoryServer`) [Script]
Requires: DataSave, InventoryTreasure, ItemConfig.

Handles all custom inventory + hotbar actions via TreasureInventoryEvent.

**Actions (OnServerEvent):**
- `"RequestSync"` → calls `InventoryTreasure.GetSyncData(player)`, fires "SyncBag" to client
- `"SellItem"`, onlyID → `InventoryTreasure.RemoveTreasure()`, add coins, fire "SellResult"
- `"SellBatch"`, {onlyID, ...} → remove multiple items, sum coins, fire "SellResult"
- `"LockItem"`, onlyID → `InventoryTreasure.ToggleFavorite()`, fire "LockItem" {onlyID, favorited}
- `"SetHotbar"`, slot, onlyID → `InventoryTreasure.SetHotbarSlot()`, creates Tool if needed, fire "SyncBag"
- `"RemoveHotbar"`, onlyID → `InventoryTreasure.RemoveFromHotbar()`, destroys Tool, fire "SyncBag"

---

## TempaServer (`ServerScriptService.Handlers.TempaServer`) [Script]
Requires: DataSave, InventoryTreasure, TempaConfig, ToolConfig, AccessoryConfig, ItemConfig, GameEnum.

Forging/upgrade system. Players upgrade Cangkul tools (max +10) and Accessories (max +5) at the Anvil. Each upgrade costs coins + treasure materials from bag.

**Key functions:**
- `hasRequiredMaterials(data, materials)` — check bag has enough of each {id, count}
- `consumeMaterials(player, data, materials)` — remove matching entries from bag (prefers non-favorited first)
- `getTempaLevel(data, type, id)` → number — read TempaLevels_Cangkul/Accessory
- `setTempaLevel(data, type, id, level)` — write tempa level
- `buildSyncData(player)` → {cangkulLevels, accessoryLevels, coins, inventoryCangkul, inventoryAccessory, materialCounts} — materialCounts uses string keys (tostring(itemId)) to avoid RemoteEvent numeric key serialization issues
- `regiveCangkul(player, toolData, tempaLevel)` — destroy old tool, clone new with tempa bonus applied

**Handlers (TempaEvent OnServerEvent):**
- `"RequestSync"` → fire "Sync" with buildSyncData
- `"TempaCangkul"`, toolId → validate ownership + level < max + coins + materials → deduct → increment → re-give if equipped → fire "TempaResult" + "Sync" + TreasureInventoryEvent "SyncBag" (hotbar update)
- `"TempaAccessory"`, accId → validate ownership + level < max + coins + materials → deduct → increment → set TempaChanged attribute (triggers AccessoryShopServer recalc) → fire "TempaResult" + "Sync" + TreasureInventoryEvent "SyncBag" (hotbar update)

---

## AdminPanelServer (`ServerScriptService.Handlers.AdminPanelServer`) [Script]
Admin panel backend. Requires: DataSave. Uses AdminEvent RemoteEvent.

**Auth:** GROUP_ID=35484105, MIN_RANK=2. Every request verified with `player:GetRankInGroup()`.

**Server Luck System:**
- `serverLuckMultiplier` (default 1), `luckExpireTime` (default 0), `adminLuckActive` flag
- Sets `workspace:SetAttribute("ServerLuckMultiplier", mult)` — read by DigServer in spawn chance calc
- Heartbeat checks: auto-reset when admin luck expires, then falls back to weekend if applicable
- **Weekend auto-luck**: every Saturday+Sunday (GMT+7, `os.time() + 25200`), auto x2 if no admin override. Checked every 30s. Admin luck takes priority; on admin reset, weekend re-applies if still weekend.

**Ban System:**
- DataStore `BanList_v1`, key `"Ban_" .. userId`
- Ban data: `{bannedBy, bannedAt, expireTime, reason, duration}`
- `expireTime = 0` means permanent, otherwise `os.time()` based
- Checked on `PlayerAdded` — kicks if active ban, removes if expired

**Handlers (AdminEvent OnServerEvent):**
- `"SetLuck"`, {multiplier, duration} → set workspace attr + timer (duration in minutes, max 480)
- `"ResetLuck"` → reset multiplier to 1 immediately
- `"LookupPlayer"`, {name} → search online players (partial match) → DataSave.GetData; offline → GetUserIdFromNameAsync + DataStore GetAsync. Returns player data summary.
- `"Kick"`, {name} → `player:Kick("Ditendang oleh admin")`
- `"TpToAdmin"`, {name} → PivotTo admin position + offset
- `"TpToPlayer"`, {name} → PivotTo target position + offset
- `"Ban"`, {name, duration} → GetUserIdFromNameAsync → banStore:SetAsync; kick if online
- `"Unban"`, {name} → banStore:RemoveAsync
- `"Sync"` → returns {multiplier, timeRemaining, playerCount}
