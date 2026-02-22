# UI Scripts

## UIController (`StarterGui.ScreenGui.UIController`) [LocalScript]
Simple coins display updater.

- Watches leaderstats.Coins.Changed → updates `CurrencyGui.CoinsDisplay.Amount.Text` with `Utils.FormatNumberAbbreviated()`

---

## NotificationClient (`StarterGui.TipsGui.NotificationClient`) [LocalScript]
Item pickup, sell, favorite & inventory notifications with stacking.

**Item pickup (via TreasureInventoryEvent):**
- Listens to TreasureInventoryEvent "ItemPickup" for treasure pickup notifications
- Shows rich text notification: "+N ItemName (Rarity)" with rarity color
- Stacking: same item name increments count, resets fade timer
- Auto-fade after 2.5s with tween

**Sell result (via TreasureInventoryEvent):**
- Listens TreasureInventoryEvent "SellResult"
- Shows "Terjual N item seharga +X $" notification

**Favorite toggle (via TreasureInventoryEvent):**
- Listens TreasureInventoryEvent "LockItem"
- Shows "Ditambahkan ke Favorit" or "Dihapus dari Favorit"

**Bag full (via TreasureInventoryEvent):**
- Listens TreasureInventoryEvent "BagFull"
- Shows "Inventaris penuh!" notification

**Pet slot full (via PetInventoryEvent):**
- Listens PetInventoryEvent "SlotFull"
- Shows "Slot pet penuh!" notification

**Purchase result notifications:**
- Listens to CangkulPurchaseEvent, PetPurchaseEvent, AccessoryPurchaseEvent "PurchaseResult"
- On success: green "Membeli [name]" notification
- On failure: "Koin tidak cukup!" (red), "Sudah dimiliki!" (yellow), or generic error

**Equip/Unequip notifications:**
- CangkulInventoryEvent "EquipResult": shows "Memakai [name]"
- PetInventoryEvent "EquipResult"/"UnequipResult": shows "Memakai/Melepas [name]"
- AccessoryInventoryEvent "EquipResult"/"UnequipResult": shows "Memakai/Melepas [name]"

**Tempa result (via TempaEvent):**
- Listens TempaEvent "TempaResult"
- On success: gold "Tempa berhasil! [name] +[level]"
- On failure: "Koin tidak cukup!" / "Material tidak cukup!" / "Sudah level maksimal!"

**General notifications (showNotify):**
- Reusable function for any system message (rich text)

Uses template from TipsGui.ItemTipsFrame.Template.

---

## MenuController (`StarterGui.MenuGui.MenuController`) [LocalScript]
Inventory management for Cangkul, Pets, Accessories, Bestiary, and Inventaris. Requires: ViewportIcon.

**UI structure:**
- Buttons: Cangkul btn, Pet btn, Accessory btn, Bestiary btn, Inventaris btn (LayoutOrder=4) — toggle frames, mutual exclusive
- Inventaris button opens/closes InventarisFrame (direct child of MenuGui, not a separate ScreenGui)
- CangkulFrame: header + close + container (slots cloned from Template)
- PetsFrame: header + close + container + Selected panel + Info + Storage
- AccessoriesFrame: header + close + container + Selected panel (Equip/Unequip)
- BestiaryFrame: header + close + container + Selected panel + layer filter buttons (controlled by BestiaryFrame.LocalScript)
- InventarisFrame: header + close + container + Selected panel + SellAllButton (controlled by InventarisClient inside it)
- Watches InventarisFrame.Visible changed signal for external toggling (e.g. HotbarClient's InventoryButton)

**Sorting (all containers):**
- Equipped items always appear first
- Then sorted by rarity descending (higher rarity = rarer = first)
- Then by ID ascending as tiebreaker

**Cangkul inventory:**
- Syncs via CangkulInventoryEvent "RequestSync"/"Sync" (includes tempaLevels)
- Requires TempaConfig for bonus calculation
- Populates slots with name, image, stats (damage/speed with tempa bonus), tempa level label, rarity color, equip/equipped buttons
- Stats show total values with bonus: "Kekuatan: 30 (+20)", "Kecepatan: 1.50d (+0.50)"
- Tempa level label: gold "Tempa: +N" if upgraded, grey "+0" if not
- ViewportFrame per slot: 3D model preview (0.7x0.5 size, centered) via ViewportIcon.Setup(vf, toolData.id)

**Pet inventory:**
- Syncs via PetInventoryEvent "RequestSync"/"Sync" (equipped = {number} array)
- Populates slots with image, rarity color, equipped indicator
- Selected panel: shows name, rarity, buff text, equip/unequip buttons
- Info: equipped count / maxEquip
- Storage: owned count / maxStorage
- **EquipSlots UI**: 3 slot indicators between Header and Container
  - ViewportFrame per slot shows equipped pet 3D model (via ViewportIcon)
  - Click slot to unequip that pet (sends petId to server)
  - Green border when filled, dark when empty
  - Container and Selected panels shifted down to make room
- `isPetEquippedClient(petId)` — helper to check if petId is in equipped array
- `updateEquipSlots()` — refreshes slot visuals on sync/populate
- Unequip button in Selected panel sends petId (not nil) to server

**Accessory inventory:**
- Syncs via AccessoryInventoryEvent "RequestSync"/"Sync"
- Populates slots with rarity color, equipped indicator
- Selected panel: shows name, slot/set info, buff text, equip/unequip buttons
- Equip fires AccessoryInventoryEvent "Equip" with accId
- Unequip fires AccessoryInventoryEvent "Unequip" with accId

---

## CangkulPurchasePrompt (`StarterGui.CangkulPurchasePrompt.PurchasePromptController`) [LocalScript]
Purchase confirmation dialog for cangkul tools.

- Triggered by LocalEvents.Open (from PurchasePromptHandler)
- Shows: title, name (rarity colored), price, speed, damage, image (rarity bordered)
- Purchase button → CangkulPurchaseEvent:FireServer("Purchase", name)
- Closes on success or cancel

---

## PetPurchasePrompt (`StarterGui.PetPurchasePrompt.PurchasePromptController`) [LocalScript]
Purchase confirmation for pets. Same pattern as Cangkul.

- Shows: title ("Beli petName?"), name, price, buff text, rarity text, image
- Purchase button → PetPurchaseEvent:FireServer("Purchase", name)

---

## AccessoryPurchasePrompt (`StarterGui.AccessoryPurchasePrompt.PurchasePromptController`) [LocalScript]
Purchase confirmation for accessories. Same pattern as PetPurchasePrompt.

- Triggered by LocalEvents.Open (from PurchasePromptHandler via prompt.ObjectText)
- Shows: title ("Beli accName?"), name (set-colored), price, buff text (+15% Speed/Damage + set name), slot type
- Purchase button → AccessoryPurchaseEvent:FireServer("Purchase", accName)
- Closes on success or cancel

---

## TeleportClient (`StarterGui.TeleportGui.TeleportClient`) [LocalScript]
Surface/underground teleport buttons.

- Tracks lastDigPos via voxelFolder.ChildRemoved
- Surface threshold: Y >= 1592
- Underground: shows "Return Surface" button → teleport to random spawn point
- Surface: shows "Return Last Dig" button → teleport to lastDigPos + Y offset
- 0.5s cooldown

---

## ProximityPromptScript (`StarterPlayer.StarterPlayerScripts.ProximityPromptScript`) [LocalScript]
Custom ProximityPrompt renderer (replaces default Roblox style).

- Creates BillboardGui with circular progress bar, action/object text
- Supports keyboard, gamepad, touch input types
- Theme system: reads prompt "Theme" attribute for custom UI templates
- Animated with tweens: fade in/out, button hold, scale

---

## DailyRewardClient (`StarterGui.DailyRewardGui.DailyRewardClient`) [LocalScript]
Daily reward popup UI controller.

**UI Structure:**
- DailyRewardGui (ScreenGui, Enabled=false, ResetOnSpawn=false)
  - Overlay (fullscreen dark TextButton, click to close)
  - MainFrame (centered 520x360 Frame)
    - Title "Daily Reward", StreakInfo "Day X / 7"
    - RewardGrid (7 day slots: Day1-Day7, each with DayLabel + CoinIcon + CoinsLabel + Checkmark)
    - ClaimButton, CloseButton

**Behavior:**
- Waits for ordered loader completion (`OrderDone_Accessory` attribute) + 0.5s, then fires "RequestShow" to server
- OnClientEvent "ShowReward" → populate grid, show popup (tween in)
- Day states: claimed (green + checkmark), current (gold border/pulse), future (dim)
- Claim button → FireServer "Claim", shows "Mengklaim..."
- OnClientEvent "ClaimResult" → success: mark day claimed, auto-close after 2s
- If canClaim=false: button shows "Kembali besok!"
- Close via X button or overlay click
- TopbarClient can also trigger popup via "RequestShow" at any time

---

## Bestiary (`StarterGui.MenuGui.BestiaryFrame.LocalScript`) [LocalScript]
Treasure collection tracker / encyclopedia. Requires: ItemConfig, GameEnum, ChangeColor, BestiaryEvent, ViewportIcon.

**UI structure:**
- BestiaryFrame.MainFrame.Container: ScrollingFrame with Template ImageButton slots
- BestiaryFrame.MainFrame.Selected: panel showing TreasureFrame (Name/Rarity/Image) + Stats (Price/Sell/Discovered)
- BestiaryFrame.Buttons.AreaList: filter buttons (All, Mud, Sand, Limestone, Ground, Slate, Basalt)

**Behavior:**
- Builds list from ItemConfig (IDs 3001-3011 = treasure items)
- Clones Template per item, sets rarity color, image, discovered/undiscovered state
- Undiscovered items: dimmed (ImageTransparency=0.6), name shows "???"
- Click item → update Selected panel (name, rarity, price range, discovered status)
- Layer filter buttons → filter displayed items by ItemConfig.layer field
- On frame visible: fires BestiaryEvent "RequestSync" → server responds with discovered item IDs
- Discovery tracked server-side in `data.DiscoveredItems` (added by InventoryTreasure.AddTreasure)
- ViewportFrame rendering:
  - Grid slots: 0.75x0.75 size, centered at 0.5,0.45, ZIndex 4, shows item model via ViewportIcon.Setup(vf, itemData.id)
  - Selected panel: persistent "SelectedViewport" frame in selectedImage, calls Setup for discovered or Clear for undiscovered

---

## InventarisClient (`StarterGui.MenuGui.InventarisFrame.InventarisClient`) [LocalScript]
Full inventory UI for managing treasure items. Lives inside MenuGui.InventarisFrame (not a separate ScreenGui). UI cloned from PetsFrame style for visual consistency. Requires: ItemConfig, GameEnum, TreasureInventoryEvent, ViewportIcon.

**UI structure:**
- InventarisFrame (Frame inside MenuGui, same style as PetsFrame/CangkulFrame)
  - Header: Title "Inventaris (X/20)", Close button
  - Container: ScrollingFrame with UIGridLayout, Template ImageButton slots
  - Selected: detail panel with Name/Rarity header, stats labels, action buttons
  - SellAllButton: "Jual Semua" at bottom

**Grid population:**
- Receives bag data via TreasureInventoryEvent "SyncBag"
- Clones Template per item into Container (ScrollingFrame with UIGridLayout)
- Each slot: Arrow pattern overlay, UIStroke (rarity color), ItemName, PriceTag, Favorited star, MutationBadge, Equipped overlay (hotbar indicator)
- ViewportFrame per slot: 0.75x0.6 size, centered at 0.5,0.42, ZIndex 4, shows item model via ViewportIcon.Setup(vf, entry.id)

**Sorting:**
- Default: rarity descending → sellPrice descending

**Selected panel (cloned from PetsFrame.Selected style):**
- Click item → shows: Name, Rarity (colored), MutationLabel, PriceLabel ("Harga Jual: $X"), LayerLabel ("Lapisan: X")
- Buttons: HotbarEquip ("Pasang Hotbar"), HotbarUnequip ("Lepas Hotbar [N]"), Favorite ("Favorit" / "Hapus Favorit"), Sell ("Jual")

**Actions (all via TreasureInventoryEvent):**
- Sell single: "SellItem", {onlyID}
- Sell batch (SellAllButton): "SellBatch" (non-favorited items)
- Favorite toggle: "LockItem", {onlyID}
- Hotbar assign: "SetHotbar", {slot, onlyID}
- Hotbar remove: "RemoveHotbar", {onlyID}

**Open/close:**
- Opened via HotbarClient InventoryButton or MenuController "Inventaris" button (both toggle InventarisFrame.Visible)
- Listens to Visible changed signal → fires "RequestSync" on open, clears selection on close
- Close button sets InventarisFrame.Visible = false

---

## TempaClient (`StarterGui.TempaGui.TempaClient`) [LocalScript]
Forging/upgrade UI for Cangkul tools and Accessories at the Anvil.

**UI Structure (matches game-wide style: Arrow sliced image, UIGradient, UIStroke):**
- TempaGui [ScreenGui] Enabled=false, ResetOnSpawn=false
  - MainFrame [Frame] BG=black T=0.2, UIStroke(white T=0.75), Arrow overlay, AspectRatio=1.9
    - Header: gold Title frame ("Pandai Besi", Fondamento Bold) + red Close frame ("X")
    - TabBar: 2 ImageButtons (CangkulTab/AksesorisTab) with gradient/stroke/arrow, gold=active gray=inactive
    - ItemList [ScrollingFrame] left ~34%: item buttons with rarity stroke/arrow, Balthazar Heavy, level badge "+X"
    - UpgradePanel [Frame] right ~60%: Arrow overlay, ItemTitle, StatsPreview, CostDisplay, MaterialStatus, TempaButton
      - TempaButton: green ImageButton (BG=89,255,0 T=0.85, gradient, arrow) matching Purchase/Claim style. Gray when can't afford or max.

**Behavior:**
- On open (Enabled=true): switches to Cangkul tab, populates items from sync data
- Tab switching: filter by Cangkul or Accessory inventory, gold highlight active tab
- Select item: shows current → next stats, cost (coins + materials with have/need counts), material availability
- TempaButton: fires "TempaCangkul"/"TempaAccessory" to server, 0.5s debounce
- On "Sync" event: refreshes item list and panel
- On "TempaResult" success: brief green flash on button
- Close: X button in red Close frame

**Stats display:**
- Cangkul: Level, Damage, Speed (current → next in colored rich text)
- Accessory: Level + each non-zero buff key (Dmg, Spd, Luck, Racun, Darah, S.Hitam)
- Material check: green if enough, red if not. Button disabled when can't afford or max level.
- Fonts: Fondamento Bold (titles/buttons), Balthazar Heavy (item names/stats). All TextScaled with black UIStroke.

---

## BuffsClient (`StarterGui.BuffsGui.BuffsClient`) [LocalScript]
Dynamic buff display that only shows active (non-zero) buffs. Positioned bottom-left.

**UI Structure:**
- BuffsGui [ScreenGui] IgnoreGuiInset=true, DisplayOrder=5, ResetOnSpawn=false
  - MainFrame [Frame] AutomaticSize=Y, dark BG (15,15,25 T=0.3), UICorner(8), UIStroke(80,80,120), UIPadding, UIListLayout(Vertical, Padding=3)
    - (Dynamic buff rows created/destroyed by script)

**Buff row layout (per active buff):**
- Frame (Size 1,0 x 0,24) with buff-colored BG at 85% transparency, UICorner(4)
  - Colored circle icon (10x10 round Frame)
  - Label TextLabel (buff name, GothamBold 13, light gray)
  - Value TextLabel ("+X%", GothamBold 13, buff color, right-aligned)

**6 Buff types tracked:**
| Key | Label | Color | Source |
|-----|-------|-------|--------|
| Kekuatan | Kekuatan | 255,100,100 | PetBuff_damage + AccessoryBuff_damage |
| Kecepatan | Kecepatan | 100,200,255 | PetBuff_speed + AccessoryBuff_speed |
| Keberuntungan | Keberuntungan | 255,255,100 | PetBuff_luck + AccessoryBuff_luck |
| Racun | Mutasi Racun | 50,200,50 | AccessoryBuff_mutasi_racun |
| Darah | Mutasi Darah | 200,30,30 | AccessoryBuff_mutasi_darah |
| SihirHitam | Sihir Hitam | 150,50,255 | AccessoryBuff_mutasi_sihir_hitam |

**Behavior:**
- Combines pet buffs (multiplier-1) + accessory buffs (/100) into single percentage per type
- Keberuntungan includes PetBuff_luck (from PetConfig buffLuck field)
- Only creates rows for buffs > 0% (empty MainFrame when no buffs active)
- Listens to 10 attribute change signals: PetBuff_damage/speed/luck, AccessoryBuff_* (6 keys), TempaChanged
- Refreshes all rows on any attribute change (destroy + recreate)
- Initial refresh via task.defer on script start

---

## DialogueUtils (`StarterGui.DialogueGUI.DialogueUtils`) [ModuleScript]
Shared utility module for all dialogue scripts. Provides cinematic, typewriter, camera, and choice functions.

**API:**
- `HideOtherGuis()` / `ShowOtherGuis()` — hides/restores HotbarGui, BuffsGui, CurrencyGui, MobileUI, TeleportGui, TipsGui, ServerLuckGui
- `FreezePlayer()` — gradually reduces WalkSpeed to 0, disables AutoRotate
- `HideCharacter(character)` / `ShowCharacter(character)` — toggles character visibility
- `FocusCamera(targetCFrame)` — Scriptable mode, tween to target (FOV 50)
- `FocusCameraGroup(positions: {CFrame})` — focuses camera to frame multiple targets (calculates midpoint + pull-back distance)
- `ReleaseCamera()` — FOV tweens back to 70, Custom camera mode restored
- `EnterCinematic()` — letterbox bars tween in 0.5s (Quad Out), hides other GUIs
- `ExitCinematic()` — bars tween out, restores GUIs, releases camera, unfreezes player
- `Typewrite(text)` — typewriter effect on DialogueText, sound per character (rbxassetid://147982968)
- `Say(speaker, text, color?)` — sets SpeakerName (colored), runs Typewrite, waits for skip
- `ShowChoices(choices: {{text, callback}})` — creates response buttons in ResponseFrame, supports number keys 1-3 + mouse click, auto-cleanup after selection
- `BindSkip()` / `UnbindSkip()` — binds/unbinds Space key for skip functionality
- `StartDialogue()` — checks DialogueActive attribute (mutual exclusion), sets it true, calls EnterCinematic
- `EndDialogue()` — calls ExitCinematic, clears DialogueActive attribute

**Colors table:**
- `Colors.npc` — gold (NPC speaker name)
- `Colors.player` — blue (player speaker name)
- `Colors.special` — used for special NPC names (e.g. Jamet, Driver)

**DialogueActive mutual exclusion:**
- `StartDialogue()` checks `player:GetAttribute("DialogueActive")` — if true, aborts (prevents overlap with SellDialogue or other dialogues)
- Sets `DialogueActive = true` on enter, clears to `false` on exit
- All dialogue scripts (TemplateProximity, WargaDialogue, PetSellerDialogue, GojekJametDialogue, PociDialogue) use this system

---

## TemplateProximity (`StarterGui.DialogueGUI.TemplateProximity`) [LocalScript]
Petani NPC cinematic dialogue. Uses DialogueUtils for all cinematic/typewriter/camera functions.

**UI Structure (DialogueGUI, ScreenGui, DisplayOrder=100, ResetOnSpawn=false):**
- CinematicTop (Frame) — black letterbox bar at top, slides in from Y=-0.1 to Y=0
- CinematicBottom (Frame) — black letterbox bar at bottom, slides in from Y=1.0 to Y=0.9
- DialogueBox (Frame) — dark semi-transparent box (15,15,15 T=0.3) at bottom center, UICorner(12), UIPadding
  - SpeakerName [TextLabel] — gold for NPC, blue for player
  - DialogueText [TextLabel] — white text, typewriter effect
  - SkipHint [TextLabel] — "[Spasi] Lewati >"
- ResponseFrame (Frame) — vertical list of choice buttons, centered, UIListLayout

**Dialogue tree (Pak Tani / Petani):**
- Branch 1: "Tempat apa ini?" (lore path) / "Langsung mulai" (skip path)
  - Lore path → Branch 2: "Ada tips?" (tips path) / "Aku siap" (skip tips)
- Always ends with closing warning about the depths

**Trigger:** ProximityPrompt on Petani.Torso.DialoguePrompt (E key, Custom style)

---

## WargaDialogue (`StarterGui.DialogueGUI.WargaDialogue`) [LocalScript]
Warga NPC dialogue about the graveyard's history, terrain layers, and dangers. Uses DialogueUtils.

**Dialogue tree:**
- Branch: "Ceritakan tentang tempat ini" (lore — history of the graveyard and layers) / "Ada sesuatu yang berbahaya?" (tips — warnings about deeper layers and mutations) / "Tidak jadi" (exit)

**Trigger:** ProximityPrompt on Warga.Torso.DialoguePrompt (E key, Custom style)

---

## PetSellerDialogue (`StarterGui.DialogueGUI.PetSellerDialogue`) [LocalScript]
Penjual Peliharaan NPC dialogue about pets. Uses DialogueUtils.

**Dialogue tree:**
- Branch: "Peliharaan apa yang kamu jual?" (lists all 3 pets with stats) / "Apa gunanya peliharaan?" (explains buff system — speed, damage, luck) / "Tidak jadi" (exit)

**Trigger:** ProximityPrompt on Penjual Peliharaan.Torso.DialoguePrompt (E key, Custom style)

---

## GojekJametDialogue (`StarterGui.DialogueGUI.GojekJametDialogue`) [LocalScript]
3-way dialogue between Player, Driver Gojek, and Jamet. Uses DialogueUtils. They discuss rumors of another land (reruntuhan kerajaan kuno / ancient kingdom ruins) beyond the mountains with even more treasure. Nobody knows the exact location yet. Two branching points. Camera uses `FocusCameraGroup` to frame both NPCs.

**Trigger:** ProximityPrompt on Driver gojek.Torso.DialoguePrompt (E key, Custom style)

---

## PociDialogue (`StarterGui.DialogueGUI.PociDialogue`) [LocalScript]
Simple cat (Poci) interaction. Uses DialogueUtils. Shows random meow sound + reaction text. Every 3rd interaction shows an extra happy response.

**Trigger:** ProximityPrompt on Poci.Meshes.poci.DialoguePrompt (E key, Custom style)

---

## DialogModule (`ReplicatedStorage.DialogModule`) [ModuleScript]
Legacy NPC dialog system (unused by current NPCs, kept for reference).

- `DialogModule.new(npcName, npc, prompt, animation)` — constructor
- `addDialog(text, responses)` — add dialog option with response choices
- Uses BillboardGui on NPC Head, response buttons (formerly in deleted StarterGui.dialog)
