# Roblox Digging Game - Project Index

## Quick Reference
Terrain-based digging game. Players dig voxel terrain with Cangkul tools, collect treasure rewards, equip pets for buffs, equip accessories for speed/damage buffs, sell loot for coins. Dynamic voxel generation - only surface layer at startup, neighbors spawn on destroy.

## Documentation Files
| File | Contents |
|------|----------|
| [architecture.md](architecture.md) | Game flow, grid system, events, instances tree |
| [configs.md](configs.md) | Config, ItemConfig, ToolConfig, PetConfig, AccessoryConfig, MutationConfig, DailyRewardConfig, TempaConfig, GameEnum |
| [server-scripts.md](server-scripts.md) | DigServer, VoxelEngine, VoxelEngineBootstrap, PlayerSetup, CangkulShopServer, SellServer, PetShopServer, AccessoryShopServer, PetDisplayBob, PlayerLighting, ToolSlotManager, PetSystemBootstrap, ChatCommands, DailyRewardServer, TreasureInventoryServer, TempaServer, AdminPanelServer |
| [systems.md](systems.md) | DataSave, InventoryTreasure, PetSystem |
| [client-scripts.md](client-scripts.md) | DigClient, PetClient, ToolSlotClient, CursorClient, PurchasePromptHandler, TopbarClient, HotbarClient, AdminPanelClient |
| [ui-scripts.md](ui-scripts.md) | UIController, NotificationClient, MenuController, CangkulPurchasePrompt, PetPurchasePrompt, AccessoryPurchasePrompt, TeleportClient, ProximityPromptScript, DailyRewardClient, Bestiary, InventarisClient, TempaClient, BuffsClient, DialogueUtils, TemplateProximity, WargaDialogue, PetSellerDialogue, GojekJametDialogue, PociDialogue, DialogModule |
| [rewards.md](rewards.md) | Reward templates per layer, Register data, loot tables |
| [utils.md](utils.md) | Utils library, ChangeColor module, ViewportIcon module |

## Key Architecture Facts
- Grid: 16 studs/cell, X range -192..368, Z range 0..368, Y range 1584..-816
- Voxel = invisible Part guide (Transparency=1, CanQuery=true) + terrain fill
- Client raycasts hit Parts -> sends Position+Size to server -> server validates + applies damage
- HP tracked server-side in dictionary. When HP<=0: clear terrain, destroy Part, GenerateBelow (depth buffer for all columns), maybe spawn reward
- 6 terrain layers (top→bottom): Mud, Sand, Limestone, Ground, Slate, Basalt
- 6 reward layers with 11 horror-themed treasures (2 per layer except Basalt with 1)
- Mutation system: 3 mutations — Racun (5%, 1.3x, green), Darah (3%, 1.5x, red), SihirHitam (2%, 2.0x, purple). Chances boosted by accessory buffs (additive)
- Pet system: multi-equip slots (max 3), slot-based positioning (right/left/behind), additive buff stacking (sum of all equipped pet buffs). Migration from old single-equip data. EquipSlots UI in PetsFrame shows 3D previews, click to unequip.
- Accessory system: 2 sets (Tengkorak=Damage focus, Vampir=Speed focus), 5 items each, multi-stat buffs (damage/speed/luck/mutasi_racun/mutasi_darah/mutasi_sihir_hitam), 5 slots (Topi/Muka/Leher/Pinggang/Baju+Celana). Full set bonus (5 pieces): Tengkorak +30%Dmg +15%Racun, Vampir +30%Spd +15%Darah
- Daily Reward: 7-day cycle (50/75/100/150/200/300/500 coins), 24h cooldown, 48h streak window
- Custom Inventory + Hotbar: replaces default Backpack (deleted CedGo + Satchel + Purse systems)
  - Bag size: max 20 items (ItemConfig.DefaultBagSize), favorites persist through SellAll
  - HotbarGui: 9 slots (slot 1=Cangkul, 2-9=treasures), empty slots hidden, number keys 1-9
  - InventarisFrame: inside MenuGui (cloned from PetsFrame style), grid view, sort by rarity/price, sell single/batch, favorite toggle, hotbar assign
  - TreasureInventoryServer handles all actions via TreasureInventoryEvent
  - Tools only created for hotbar items (not all bag items)
  - DataSave: Favorites={} and HotbarSlots={} in player data
- Bestiary system: tracks discovered treasure items (persisted in DiscoveredItems), UI in MenuGui.BestiaryFrame with layer filters and selected panel
- Ordered loader: PlayerSetup orchestrates join sequence → Cangkul → Pet → Accessory → Treasure + SyncBag (attribute signals with 15s timeout)
- Purchase failure notifications: all 3 shops (Cangkul/Pet/Accessory) show "Koin tidak cukup!" / "Sudah dimiliki!" via NotificationClient
- Equip/unequip notifications: Cangkul switch, Pet equip/unequip, Accessory equip/unequip all show notification
- TopbarPlus: "Hadiah Harian" button in topbar (TopbarClient → DailyRewardServer "RequestShow")
- NotificationClient: listens TreasureInventoryEvent for ItemPickup, SellResult, LockItem (favorite), BagFull
- Tempa (forging/upgrade): Anvil ProximityPrompt → TempaGui, upgrade Cangkul (+10 max, flat dmg/spd bonus) or Accessories (+5 max, 30%/lvl multiplier). Costs coins + treasure materials from bag. TempaServer validates + deducts + increments. DataSave: TempaLevels_Cangkul, TempaLevels_Accessory
- ViewportIcon system: 3D model previews in all inventory UIs (Hotbar, Inventaris, Bestiary, MenuController). Uses stripped models from ReplicatedStorage.IconModels (Treasures/Cangkuls folders). Auto-fits camera based on bounding box, consistent angled view
- Admin Panel: group rank auth (group 35484105, rank >= 2), semicolon (;) toggle, ServerLuck multiplier (x2/x4/x8 with timer), player lookup (online/offline), ban system (BanList_v1 DataStore), kick/TP
- Dialogue system: DialogueUtils shared module in DialogueGUI provides cinematic bars, typewriter, camera focus, choices, mutual exclusion (DialogueActive attribute). 5 dialogue scripts: TemplateProximity (Petani), WargaDialogue (graveyard lore/tips), PetSellerDialogue (pet info), GojekJametDialogue (3-way, kingdom rumors, FocusCameraGroup), PociDialogue (cat meow). All use ProximityPrompt triggers on NPC torsos.
- VFX folder renamed from "VFX" to "Efek Mutasi" in workspace; VFX cloning now clones individual ParticleEmitters instead of whole Part templates
- All GUI text is in Indonesian (Bahasa Indonesia)

## Key Lessons
- Never send Part instances through RemoteEvents (unreliable with StreamingEnabled). Send Vector3.
- Terrain FillBlock(Air) leaves remnants. Use WriteVoxels for clean clearing.
- Transparent Parts (CanQuery=true) as raycast guides solve terrain grid precision issues.
- Always check for multiple Roblox Studio instances - MCP may connect to wrong one.
- VoxelEngine is ModuleScript (required by Bootstrap + DigServer). Bootstrap Script calls GenerateSurface().
- Roblox `require` cache persists in Studio edit mode - changing Source won't affect cached modules until playtest.
- When modifying script Source via MCP, string replacement with gsub can fail on special chars or line endings. Use plain `find` + `sub` for reliable edits.
