# Konten Game

## Lapisan Bumi (6 Layers)

| # | Layer | Material | HP | Speed | Kedalaman Y | Keterangan |
|---|-------|----------|-----|-------|-------------|------------|
| 1 | Mud | Mud | 50 | 2 | 1584–1120 | Layer awal, treasure murah tapi cepat digali |
| 2 | Sand | Sand | 80 | 3 | 1104–640 | Mulai ada treasure yang lebih berharga |
| 3 | Limestone | Limestone | 120 | 4 | 624–160 | Treasure uncommon-epic, harga naik signifikan |
| 4 | Ground | Ground | 150 | 5 | 144–(-80) | Treasure langka mulai muncul |
| 5 | Slate | Slate | 200 | 6 | (-96)–(-320) | Kertas Judi, Gigi Emas |
| 6 | Basalt | Basalt | 250 | 7 | (-336)–(-816) | End-game content, Keranda |

Semakin dalam = HP lebih tinggi + speed lebih lambat = butuh tool dan pet yang lebih baik.

---

## Treasure (11 Items — Horror Theme)

### Layer 1 — Mud (Mudah)
| Treasure | ID | Rarity | Chance | Harga Jual |
|----------|----|--------|--------|------------|
| Paku Karatan | 3001 | Common | 60% | 3-8 coins |
| Tulang Tangan | 3002 | Common | 40% | 3-8 coins |

### Layer 2 — Sand
| Treasure | ID | Rarity | Chance | Harga Jual |
|----------|----|--------|--------|------------|
| Tulang Iga | 3003 | Common | 55% | 5-12 coins |
| Tengkorak | 3004 | Common | 45% | 5-12 coins |

### Layer 3 — Limestone
| Treasure | ID | Rarity | Chance | Harga Jual |
|----------|----|--------|--------|------------|
| Boneka Sihir Hitam | 3005 | Rare | 50% | 25-50 coins |
| Telur Dipaku | 3006 | Rare | 50% | 25-50 coins |

### Layer 4 — Ground
| Treasure | ID | Rarity | Chance | Harga Jual |
|----------|----|--------|--------|------------|
| Foto Buram | 3007 | Rare | 55% | 30-60 coins |
| Kertas Ilmu Hitam | 3008 | Epic | 45% | 75-140 coins |

### Layer 5 — Slate
| Treasure | ID | Rarity | Chance | Harga Jual |
|----------|----|--------|--------|------------|
| Kertas Judi | 3009 | Epic | 55% | 80-150 coins |
| Gigi Emas | 3010 | Legendary | 45% | 250-450 coins |

### Layer 6 — Basalt (End-game)
| Treasure | ID | Rarity | Chance | Harga Jual |
|----------|----|--------|--------|------------|
| Keranda | 3011 | Legendary | 100% | 350-650 coins |

- **Spawn chance:** 30% per blok dihancurkan, dipengaruhi luck buff dan Server Luck multiplier
- **Treasure spawn:** tertanam di tanah (terrain dummy mengisi voxel), highlight hijau saat digali
- **Auto-despawn:** 60 detik setelah spawn
- **Bag full protection:** treasure tetap spawn tapi tidak bisa diambil jika tas penuh (20/20), HP di-restore

---

## Sistem Mutasi (3 Jenis)

| Mutasi | Rarity | Chance | Multiplier | Efek Visual |
|--------|--------|--------|------------|-------------|
| Racun (Poison) | Uncommon | 5% | 1.3x | Warna hijau, highlight glow, poison VFX |
| Darah (Blood) | Rare | 3% | 1.5x | Warna merah, highlight glow, blood VFX (2 emitters) |
| Sihir Hitam (Black Magic) | Epic | 2% | 2.0x | Warna ungu gelap, highlight glow, dark magic VFX |

- Mutasi di-roll saat treasure spawn (first hit wins — dicek berurutan)
- Chance mutasi bisa ditambah dengan buff aksesoris (additive per jenis: base + AccessoryBuff/100)
- Treasure bermutasi memiliki efek visual unik dan harga jual dikalikan multiplier
- Harga jual formula: `math.floor(baseSellPrice * sellMultiplier)`

---

## Tools (Cangkul)

| Tool | ID | Damage | Speed | Harga | Rarity |
|------|----|--------|-------|-------|--------|
| Cangkul Karatan | 1 | 10 | 1.0x | Gratis (starter) | Common |
| Cangkul Petani | 2 | 15 | 1.2x | 100 coins | Uncommon |
| Cangkul Tulang | 3 | 25 | 1.5x | 500 coins | Rare |

---

## Pets

| Pet | ID | Buff | Harga | Rarity |
|-----|----|------|-------|--------|
| Tuyul | 1 | +20% speed gali | 200 coins | Common |
| 3 Tuyul | 2 | +50% speed gali | 1,000 coins | Rare |

- **Multi-equip:** hingga 3 pet sekaligus
- **Slot positioning:** right, left, behind (posisi berbeda per slot)
- **Buff stacking aditif:** semua buff pet yang diequip dijumlahkan
- Pet mengikuti pemain secara smooth dengan animasi bobbing

---

## Accessories (10 Items — 2 Set)

### Set Tengkorak (Damage Focus)
| Accessory | ID | Slot | Harga | Rarity |
|-----------|----|------|-------|--------|
| Baju Tengkorak | 1 | Baju+Celana | 300 coins | Uncommon |
| Kalung Tengkorak | 2 | Leher | 200 coins | Common |
| Sabuk Tengkorak | 3 | Pinggang | 250 coins | Common |
| Topi Tengkorak | 4 | Topi | 400 coins | Rare |
| Topeng Tengkorak | 5 | Muka | 350 coins | Uncommon |

**Buff per piece:** Damage +10%, Speed +5%, Luck +5%, Racun +15%, Darah +5%, Sihir Hitam +10%

### Set Vampir (Speed Focus)
| Accessory | ID | Slot | Harga | Rarity |
|-----------|----|------|-------|--------|
| Baju Vampir | 6 | Baju+Celana | 300 coins | Uncommon |
| Kalung Vampir | 7 | Leher | 200 coins | Common |
| Sabuk Vampir | 8 | Pinggang | 250 coins | Common |
| Topi Vampir | 9 | Topi | 400 coins | Rare |
| Topeng Vampir | 10 | Muka | 350 coins | Uncommon |

**Buff per piece:** Damage +5%, Speed +10%, Luck +5%, Racun +5%, Darah +15%, Sihir Hitam +10%

### Slot & Bonus
- **5 slot equip:** Topi, Muka, Leher, Pinggang, Baju+Celana
- **6 jenis buff:** damage, speed, luck, mutasi_racun, mutasi_darah, mutasi_sihir_hitam
- **Full set bonus (5 pieces same set):**
  - Tengkorak: +30% Damage, +15% Racun
  - Vampir: +30% Speed, +15% Darah
- Baju+Celana mengubah pakaian karakter, aksesoris lain ditambahkan sebagai Accessory ke karakter

---

## Daily Reward (7-Day Cycle)

| Hari | Reward |
|------|--------|
| Day 1 | 50 coins |
| Day 2 | 75 coins |
| Day 3 | 100 coins |
| Day 4 | 150 coins |
| Day 5 | 200 coins |
| Day 6 | 300 coins |
| Day 7 | 500 coins (bonus) |

- Cooldown 24 jam antar klaim
- Streak reset jika tidak klaim lebih dari 48 jam
- Siklus berulang setelah hari ke-7
- Popup otomatis saat login, bisa dibuka ulang via tombol "Hadiah Harian" di topbar

---

## Bestiary (Treasure Encyclopedia)

Koleksi 11 treasure yang melacak penemuan pemain:
- Item yang belum ditemukan tampil redup dengan nama "???"
- Filter berdasarkan layer: All, Mud, Sand, Limestone, Ground, Slate, Basalt
- Panel detail menampilkan: nama, rarity, range harga jual, 3D preview, status penemuan
- Data penemuan tersimpan permanen di DataStore

---

## Tempa (Upgrade/Forging System)

Pemain bisa meng-upgrade Cangkul dan Aksesoris di Pandai Besi (Anvil) menggunakan coins + treasure material.

### Tempa Cangkul (Max +10)

| Tool | Base Dmg | Base Spd | +Dmg/Lvl | +Spd/Lvl | Max Dmg @+10 | Max Spd @+10 |
|------|----------|----------|----------|----------|-------------|-------------|
| Cangkul Karatan | 10 | 1.0 | +2 | +0.05 | 30 | 1.50 |
| Cangkul Petani | 15 | 1.2 | +3 | +0.06 | 45 | 1.80 |
| Cangkul Tulang | 25 | 1.5 | +5 | +0.08 | 75 | 2.30 |

### Biaya Tempa Cangkul

| Level | Coins | Material |
|-------|-------|----------|
| +1 | 50 | 2x Paku Karatan |
| +2 | 100 | 3x Paku Karatan |
| +3 | 200 | 2x Tulang Iga |
| +4 | 350 | 3x Tulang Iga |
| +5 | 600 | 2x Boneka Sihir Hitam |
| +6 | 1,000 | 2x Telur Dipaku |
| +7 | 1,500 | 2x Foto Buram |
| +8 | 2,200 | 1x Kertas Ilmu Hitam |
| +9 | 3,000 | 2x Kertas Judi |
| +10 | 4,000 | 1x Gigi Emas |

### Tempa Aksesoris (Max +5)

Setiap level menambah +30% dari base buff. Formula: `totalBuff = baseBuff × (1 + level × 0.3)`

| Level | Multiplier | Coins | Material |
|-------|-----------|-------|----------|
| +1 | 1.3x | 100 | 2x Tulang Tangan |
| +2 | 1.6x | 250 | 2x Tengkorak |
| +3 | 1.9x | 500 | 1x Boneka Sihir Hitam |
| +4 | 2.2x | 1,000 | 1x Kertas Ilmu Hitam |
| +5 | 2.5x | 2,000 | 1x Kertas Judi |

Material dikonsumsi dari tas pemain (inventory treasure). Prioritas: non-favorited dulu.

---

## Custom Inventory & Hotbar

### Hotbar (9 Slot)
- Slot 1 = Cangkul (selalu visible)
- Slot 2-9 = treasure items dari data hotbar
- Number keys 1-9 atau klik untuk equip/unequip
- Slot kosong otomatis tersembunyi
- 3D model preview per slot via ViewportFrame

### Tas (Bag)
- Max 20 item
- Favorites persist saat "Jual Semua"
- Grid view dengan sorting (equipped first, lalu rarity tertinggi)
- Aksi: sell single/batch, favorite toggle, hotbar assign/remove
- InventoryButton di hotbar menampilkan "X/20" bag count

---

## Server Luck System

### Admin Server Luck
- Admin bisa set multiplier luck server: x2, x4, atau x8
- Durasi: 15 menit, 30 menit, 1 jam, atau 2 jam
- Mengalikan treasure spawn chance untuk SEMUA pemain

### Weekend Auto-Luck
- Otomatis x2 setiap Sabtu & Minggu (GMT+7 / WIB)
- Admin override: luck admin > weekend luck
- Saat admin luck expire, fallback ke weekend jika masih weekend

### Indikator
- Semua pemain bisa lihat indikator Server Luck di kanan bawah (di atas coins)
- Color-coded: hijau (x2), ungu (x4), emas (x8)
- Menampilkan persentase dan sumber (Admin/Weekend)

---

## Admin Panel

Panel admin untuk manajemen server, hanya bisa diakses oleh admin (group rank >= 2).

### Tab Server Luck
- ON/OFF toggle, multiplier selector (x2/x4/x8)
- Duration selector (15m/30m/1h/2h)
- Apply/Reset buttons
- Status display (aktif/tidak aktif, sisa waktu)

### Tab Cari Pemain
- Search box untuk cari pemain (online & offline)
- Player header (avatar + nama + status online/offline)
- 6 sub-tab kategori: Info, Cangkul, Pets, Aksesoris, Tas, Tempa
- Grid view dengan item clickable + detail panel dengan 3D preview

### Tab Ban & Kick
- Target display dari lookup
- Ban options: Perma, 1 Bulan, 1 Tahun, Unban
- Kontrol: Kick, TP ke Admin, TP ke Pemain
- Feedback label untuk hasil aksi
- Self-ban/kick prevention

---

## Rarity System

| Rarity | Warna | Frekuensi |
|--------|-------|-----------|
| Common | Abu-abu | Paling sering |
| Uncommon | Hijau | Sering |
| Rare | Biru | Jarang |
| Epic | Ungu | Sangat jarang |
| Legendary | Oranye | Paling langka |
