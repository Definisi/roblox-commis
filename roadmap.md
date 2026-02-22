# Roadmap & TODO

## Status Saat Ini
Core gameplay sudah playable. Pemain bisa menggali, mengumpulkan treasure, membeli tool/pet/accessory, equip accessories untuk buff, mengecek koleksi di Bestiary, meng-upgrade equipment di Pandai Besi, dan menjual loot. Admin panel tersedia untuk manajemen server. Semua sistem utama berjalan dengan data persistence dan ordered loading.

---

## TODO - Prioritas Tinggi

### Konten Tambahan
- [ ] Tambah lebih banyak jenis Cangkul (4-6 tier tambahan untuk mid-late game progression)
- [ ] Tambah lebih banyak Pet dengan buff berbeda (damage buff, luck buff, coins buff)
- [ ] Tambah rarity Mythical untuk end-game treasure ultra-langka

### Progression & Engagement
- [ ] Sistem rebirth/prestige — reset progress untuk permanent multiplier
- [ ] Quest/mission system (harian & mingguan)
- [ ] Achievement/badge system
- [ ] Leaderboard (most coins, deepest dug, rarest treasure found)

### Social Features
- [ ] Trading system antar pemain
- [ ] Party/group digging
- [ ] Chat commands untuk interaksi

---

## TODO - Prioritas Sedang

### Visual & Polish
- [ ] Efek visual untuk setiap rarity saat pickup
- [ ] Background music & sound effects per layer
- [ ] Particle effects yang lebih beragam per layer
- [ ] Animasi karakter saat menggali

### Monetisasi
- [ ] Gamepass: 2x Coins
- [ ] Gamepass: Auto-sell
- [ ] Gamepass: Extra pet slot
- [ ] Gamepass: Exclusive tools
- [ ] Limited-time event items
- [ ] Robux shop untuk premium pets/tools

### World Building
- [ ] Area spawn yang lebih menarik (lobby/hub area)
- [ ] NPC shopkeeper dengan dialog
- [ ] Tutorial/onboarding untuk pemain baru
- [ ] Signage dan petunjuk arah di dunia game

---

## TODO - Prioritas Rendah (Nice to Have)

### Advanced Systems
- [ ] Pet leveling system
- [ ] Crafting system (combine treasures)
- [ ] Private mine/area untuk setiap pemain
- [ ] Boss encounters di kedalaman tertentu
- [ ] Seasonal events (Halloween mine, Christmas treasures, dll)

### Technical
- [ ] Optimize untuk mobile performance
- [ ] Analytics tracking (player retention, engagement metrics)
- [ ] A/B testing untuk balancing ekonomi

---

## Sudah Selesai

### Core Systems
- [x] Core digging mechanic dengan 6 lapisan (Mud, Sand, Limestone, Ground, Slate, Basalt)
- [x] Dynamic voxel generation — tanah baru muncul otomatis saat menggali lebih dalam
- [x] 11 jenis treasure horror-themed dengan 4 rarity levels (Common, Rare, Epic, Legendary)
- [x] 3 tier Cangkul tools dengan damage/speed berbeda
- [x] 2 jenis Pet (Tuyul, 3 Tuyul) dengan speed buff
- [x] Multi-equip pet system — hingga 3 pet sekaligus, slot positioning, additive buff stacking

### Economy & Shops
- [x] Shop system (Cangkul, Pet, Accessory) dengan proximity prompt
- [x] Sell system (jual semua / jual satu / jual batch, preserves favorites)
- [x] Coin display dengan format abbreviated (1.5K, 1M, dll)
- [x] Purchase failure notifications — "Koin tidak cukup!" / "Sudah dimiliki!" di semua shop

### Accessory System
- [x] 10 aksesoris (2 set × 5 item): Tengkorak (Damage focus), Vampir (Speed focus)
- [x] 5 slot equip: Topi, Muka, Leher, Pinggang, Baju+Celana
- [x] Multi-stat buff: damage, speed, luck, mutasi_racun, mutasi_darah, mutasi_sihir_hitam
- [x] Full set bonus: Tengkorak +30%Dmg +15%Racun, Vampir +30%Spd +15%Darah
- [x] Visual equip — outfit mengubah pakaian, aksesoris ditambahkan ke karakter

### Mutation System
- [x] 3 jenis mutasi: Racun (5%, 1.3x, hijau), Darah (3%, 1.5x, merah), SihirHitam (2%, 2.0x, ungu)
- [x] Chance mutasi ditambah oleh accessory buffs (additive per jenis)
- [x] Efek visual unik per mutasi: color override, highlight glow, VFX particles

### Tempa (Forging/Upgrade)
- [x] Upgrade Cangkul (+10 max, flat dmg/spd bonus per level)
- [x] Upgrade Aksesoris (+5 max, 30%/lvl buff multiplier)
- [x] Biaya upgrade: coins + treasure material dari tas
- [x] UI preview stats (sebelum → sesudah), ketersediaan material

### Custom Inventory & Hotbar
- [x] Custom Hotbar: 9 slot (slot 1=Cangkul, 2-9=treasures), number keys 1-9
- [x] Tas (bag): max 20 item, favorites persist saat Jual Semua
- [x] InventarisFrame: grid view, sell single/batch, favorite toggle, hotbar assign
- [x] Bag full protection: treasure tetap spawn, tidak bisa diambil jika penuh, HP di-restore
- [x] ViewportIcon system: 3D model preview di semua inventory UI

### Bestiary
- [x] Treasure encyclopedia dengan 11 item, layer filter, 3D preview
- [x] Discovery tracking — item belum ditemukan tampil "???"
- [x] Data penemuan tersimpan permanen

### Admin Panel
- [x] Group rank authentication (group 35484105, rank >= 2)
- [x] Tab-based UI (Server Luck, Cari Pemain, Ban & Kick)
- [x] Server Luck: ON/OFF toggle, multiplier (x2/x4/x8), duration timer
- [x] Weekend auto-luck: otomatis x2 setiap Sabtu & Minggu (GMT+7)
- [x] Player lookup: online & offline, 6 kategori data (Info/Cangkul/Pets/Aksesoris/Tas/Tempa)
- [x] Ban system: Perma/1 Bulan/1 Tahun/Unban, BanList DataStore
- [x] Kick/TP ke Admin/TP ke Pemain, self-ban/kick prevention
- [x] Server Luck indicator: visible untuk semua pemain (kanan bawah, di atas coins)

### UI & Polish
- [x] Buffs GUI — dynamic buff display (kiri bawah), auto-rebuild saat buff berubah
- [x] Daily rewards / login streak (7-day cycle, 24h cooldown, 48h streak window)
- [x] TopbarPlus — tombol "Hadiah Harian" di topbar
- [x] Notification system — treasure pickup, sell result, bag full, favorite, equip/unequip
- [x] Container sorting — item yang dipakai tampil pertama, lalu urut rarity tertinggi

### Infrastructure
- [x] Data persistence (auto-save 60s, save on leave, save on shutdown)
- [x] Ordered loader — sequential player init (Cangkul → Pet → Accessory → Treasure + SyncBag)
- [x] Anti-exploit (server-side validation, range check, timing check, bag enforcement)
- [x] Teleport surface/underground
- [x] NPC dialog system
- [x] Underground lighting (PointLight mengikuti pemain)
- [x] Custom proximity prompts (semua prompt: Cangkul, Pet, Sell, Accessory, Anvil)
- [x] Lokalisasi Bahasa Indonesia — seluruh teks GUI dalam Bahasa Indonesia
