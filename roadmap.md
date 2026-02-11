# Roadmap & TODO

## Status Saat Ini
Core gameplay sudah playable. Pemain bisa menggali, mengumpulkan treasure, membeli tool/pet/accessory, equip accessories untuk buff, mengecek koleksi di Bestiary, dan menjual loot. Semua sistem utama berjalan dengan data persistence dan ordered loading.

---

## TODO - Prioritas Tinggi

### Konten Tambahan
- [ ] Tambah lebih banyak jenis Cangkul (4-6 tier tambahan untuk mid-late game progression)
- [ ] Tambah lebih banyak Pet dengan buff berbeda (damage buff, luck buff, coins buff)
- [ ] Tambah rarity Mythical untuk end-game treasure ultra-langka
- [x] Tambah lebih banyak jenis mutasi (Gold 1.5x, Frozen 1.3x, Darkness 1.8x)

### Progression & Engagement
- [ ] Sistem rebirth/prestige — reset progress untuk permanent multiplier
- [x] Daily rewards / login streak
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
- [x] Tambah variasi visual treasure (model 3D horror-themed dari Workspace.Items)
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
- [x] ~~Enchantment/upgrade system untuk tools~~ → Sudah ada: Tempa system (Cangkul +10, Accessory +5)
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

- [x] Core digging mechanic dengan 6 lapisan
- [x] 11 jenis treasure horror-themed dengan 4 rarity levels (Common, Rare, Epic, Legendary)
- [x] 3 tier Cangkul tools
- [x] 2 jenis Pet dengan speed buff
- [x] Shop system (Cangkul & Pet)
- [x] Inventory management UI
- [x] Sell system (jual semua / jual satu)
- [x] Mutation system — 3 jenis: Gold (8%, 1.5x), Frozen (6%, 1.3x), Darkness (3%, 1.8x)
- [x] Data persistence (auto-save, load on join)
- [x] Anti-exploit (server-side validation)
- [x] Notification system
- [x] Teleport surface/underground
- [x] NPC dialog system
- [x] Underground lighting
- [x] Custom proximity prompts
- [x] Coin display dengan format abbreviated
- [x] Daily rewards / login streak (7-day cycle, 24h cooldown, 48h streak window)
- [x] Accessory system — 2 set (Tengkorak/Vampir), 5 slot, visual equip + buff
- [x] Bestiary — treasure encyclopedia dengan layer filter, discovery tracking
- [x] Ordered loader — sequential player init (Cangkul → Pet → Accessory → Treasure)
- [x] Purchase failure notifications — semua shop menampilkan notifikasi "Koin tidak cukup" / "Sudah dimiliki"
- [x] Equip/unequip notifications — Cangkul, Pet, dan Accessory semua menampilkan notifikasi saat dipakai/dilepas
- [x] TopbarPlus — tombol "Hadiah Harian" di topbar untuk buka daily reward kapan saja
- [x] Lokalisasi Bahasa Indonesia — seluruh teks GUI dalam Bahasa Indonesia
- [x] Container sorting — item yang dipakai tampil pertama, lalu urut rarity tertinggi
- [x] Tempa (forging/upgrade) system — upgrade Cangkul (+10 max) dan Accessory (+5 max) di Pandai Besi dengan coins + treasure material
