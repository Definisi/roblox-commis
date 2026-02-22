# Client Scripts

## DigClient (`ServerStorage.ToolTemplates.*.DigClient`) [LocalScript]
Inside each Cangkul tool template. All 3 copies are identical.

**Behavior:**
- On Equip: connect Heartbeat, InputBegan/Ended
- Heartbeat loop:
  - Update raycast filter (include Voxel/ + Rewards/ folders)
  - Raycast from mouse screen position
  - Determine hit type: voxel Part or reward Model
  - If valid target in range:
    - Show TargetInfo (layer name or item name)
    - On new target: InvokeServer GetVoxelHP for initial HP display
    - If mouseDown: accumulate digProgress (dt * toolSpeed * petBuff * (1 + accSpeedBuff/100) / voxelSpeed)
    - Progress >= 1: fire DigEvent or RewardDigEvent, spawn particles, camera shake
  - Track currentVoxelSpeed from server responses
- Reward highlighting: Highlight instance on reward model while digging
- Particle effects: colored debris particles at hit position
- Camera shake: 3-frame random offset

**UI elements used (from StarterGui.MobileUI):**
- Crosshair (hidden, Visible=false always)
- TargetInfo frame: HPBar (BarCurrent + BarDamage + HP text + TargetName) + ChargeBar (Bar fill)

---

## PetClient (`StarterPlayer.StarterPlayerScripts.PetClient`) [LocalScript]
Smooth pet following using RenderStepped.

- Finds `workspace[playerName.."_Pets"]` folder
- For each pet BasePart: reads offset attributes, applies sin-wave bob, dt-based lerp to follow HRP

---

## ToolSlotClient (`StarterPlayer.StarterPlayerScripts.ToolSlotClient`) [LocalScript]
Client-side tool drop prevention.

- Blocks Backspace key (prevent default drop)
- Watches character.ChildRemoved: if Tool goes to workspace, return to Backpack
- Watches workspace.ChildAdded: return tools belonging to local player

---

## CursorClient (`StarterPlayer.StarterPlayerScripts.CursorClient`) [LocalScript]
3D cursor that follows mouse raycast on surfaces. Only visible when Cangkul is equipped.

- Clones `workspace.Assets.Cursor` part
- RenderStepped: raycast from mouse, position cursor at hit + normal offset
- Transparency 0.5 when active, 1 when hidden

---

## PurchasePromptHandler (`StarterPlayer.StarterPlayerScripts.PurchasePromptHandler`) [LocalScript]
Routes ProximityPrompt triggers to purchase UIs.

- "CangkulPurchase" → CangkulPurchasePrompt.Events.Open:Fire(modelName)
- "PetPurchase" → PetPurchasePrompt.Events.Open:Fire(modelName)
- "AccessoryPurchase" → AccessoryPurchasePrompt.Events.Open:Fire(prompt.ObjectText)
- "TempaPurchase" → TempaEvent:FireServer("RequestSync"), TempaGui.Enabled = true

---

## HotbarClient (`StarterGui.HotbarGui.HotbarClient`) [LocalScript]
Custom hotbar replacing the default Roblox Backpack. Requires: ViewportIcon.

**Setup:**
- Disables default Backpack (`StarterGui:SetCoreGuiEnabled(CoreGuiType.Backpack, false)`)
- Re-disables on each respawn via CharacterAdded
- 9 slots: Slot 1 = Cangkul (always visible), Slots 2-9 = treasure items from hotbar data
- Empty slots are hidden (Visible=false), only occupied slots shown
- InventoryButton with CountLabel showing "X/20" bag count

**Slot rendering:**
- Each slot styled with Arrow pattern + UIStroke (RarityStroke) matching menu button style
- Rarity color on Arrow ImageColor3 and RarityStroke
- MutationBadge visible for mutated items
- SelectedHighlight visible on equipped slot
- ViewportFrame per slot: 3D model preview using ViewportIcon.Setup()
  - Slot 1: shows current Cangkul model (tracked via cangkulToolId state variable)
  - Slots 2-9: shows treasure item models or cleared when empty

**Input:**
- Number keys 1-9: equip/unequip tool in that slot (with gameProcessed check)
- Click slot: equip tool (toggle if already equipped)
- InventoryButton click: toggles InventarisFrame in MenuGui

**Equip flow:**
- Calls humanoid:UnequipTools() then task.wait() then humanoid:EquipTool(target)
- Tracks selectedSlot state, synced with Character ChildAdded/ChildRemoved events
- Self-healing: if tool not found for a slot, clears stale slotTools data and hides the slot

**Auto-cleanup (proactive):**
- `cleanupDestroyedTool(onlyID)` — clears slotTools, cachedData.treasures, cachedData.hotbarSlots for destroyed item, resets selectedSlot if needed, calls updateAllSlots()
- `watchContainerForDestroys(container)` — connects ChildRemoved on Backpack and Character; on removal, task.defer checks if tool.Parent == nil (destroyed, not moved) → calls cleanupDestroyedTool
- Hotbar slots auto-disappear immediately when Tools are destroyed (sell, tempa material consume, etc.) — no click needed

**Data sync:**
- Listens to CangkulInventoryEvent for Cangkul slot updates (slot 1, updates cangkulToolId state)
  - "Sync" and "EquipResult" events both update cangkulToolId with toolData.id
- Listens to TreasureInventoryEvent for "SyncBag" (rebuild slots 2-9, reset selectedSlot if invalid) and "ItemPickup" (add new item to first empty slot, sync cachedData.hotbarSlots)
- Initial sync via task.delay(3) to wait for PlayerSetup completion

---

## TopbarClient (`StarterPlayer.StarterPlayerScripts.TopbarClient`) [LocalScript]
Adds "Hadiah Harian" (Daily Reward) button to the Roblox topbar using TopbarPlus v3.

- Requires `ReplicatedStorage.Topbarplus.Icon`
- Creates icon with label "Hadiah Harian" and gift image
- On click: fires DailyRewardEvent "RequestShow" to server, deselects immediately
- Server responds with "ShowReward" → DailyRewardClient handles the popup

---

## AdminPanelClient (`StarterGui.AdminPanel.AdminPanelClient`) [LocalScript]
Admin panel frontend. Auth: `GetRankInGroup(35484105) >= 2`, non-admins get panel destroyed.

**Keybind:** Semicolon (`;`) toggles `AdminPanel.Enabled`

**Tab-based UI** (3 tabs like ImGui):
- **Tab "Server Luck"**: ON/OFF toggle, multiplier selector (x2/x4/x8), duration selector (15m/30m/1h/2h), Apply/Reset buttons. Status shows active multiplier + time remaining.
- **Tab "Cari Pemain"**: search box, player header (image+name+status), 6 category sub-tabs (Info/Cangkul/Pets/Aksesoris/Tas/Tempa). Grid container (left) with clickable items + Selected panel (right) with ViewportFrame 3D preview + stats. Matches InventarisFrame style.
- **Tab "Ban & Kick"**: target display (from lookup), ban buttons (Perma/1 Bulan/1 Tahun/Unban), misc buttons (Kick/TP ke Admin/TP ke Pemain). Feedback label shows action results.

**Self-protection:** Server blocks self-ban and self-kick with error message.

**Periodic sync:** Every 5s (when panel visible) fires "Sync" → updates luck status + player count.

## ServerLuckIndicator (`StarterGui.ServerLuckGui.ServerLuckIndicator`) [LocalScript]
Shows ServerLuck multiplier to ALL players (bottom-right, above coins). Hidden when x1, visible when active. Color-coded: green (x2), purple (x4), gold (x8). Listens to `workspace.ServerLuckMultiplier` attribute.
