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
Inventory management for Cangkul, Pets, Accessories, Bestiary, and Inventaris.

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
- Syncs via CangkulInventoryEvent "RequestSync"/"Sync"
- Populates slots with name, image, stats (damage/speed), rarity color, equip/equipped buttons

**Pet inventory:**
- Syncs via PetInventoryEvent "RequestSync"/"Sync"
- Populates slots with image, rarity color, equipped indicator
- Selected panel: shows name, rarity, buff text, equip/unequip buttons
- Info: equipped count / maxEquip
- Storage: owned count / maxStorage

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
Treasure collection tracker / encyclopedia. Requires: ItemConfig, GameEnum, ChangeColor, BestiaryEvent.

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

---

## InventarisClient (`StarterGui.MenuGui.InventarisFrame.InventarisClient`) [LocalScript]
Full inventory UI for managing treasure items. Lives inside MenuGui.InventarisFrame (not a separate ScreenGui). UI cloned from PetsFrame style for visual consistency. Requires: ItemConfig, GameEnum, TreasureInventoryEvent.

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

## DialogModule (`ReplicatedStorage.DialogModule`) [ModuleScript]
NPC dialog system with typewriter effect.

- `DialogModule.new(npcName, npc, prompt, animation)` — constructor
- `addDialog(text, responses)` — add dialog option with response choices
- `triggerDialog(player, questionNumber)` — typewriter text, show numbered responses
- `hideGui(exitQuip)` — close with optional exit message
- Uses BillboardGui on NPC Head, response buttons in StarterGui.dialog
- Keyboard number keys (1-9) or click to choose response
- Distance check: auto-close if player walks away (>10 studs)
