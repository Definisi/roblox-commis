# Konten Game

## Lapisan Bumi (6 Layers)

| # | Layer | Kedalaman | Kesulitan | Keterangan |
|---|-------|-----------|-----------|------------|
| 1 | Mud | Paling atas | Mudah | Layer awal, treasure murah tapi cepat digali |
| 2 | Sand | Menengah atas | Sedang | Mulai ada treasure yang lebih berharga |
| 3 | Limestone | Tengah | Menengah | Treasure uncommon-epic, harga naik signifikan |
| 4 | Ground | Menengah bawah | Sulit | Treasure langka mulai muncul |
| 5 | Slate | Dalam | Sangat Sulit | Kertas Judi, Gigi Emas |
| 6 | Basalt | Terdalam | Paling Sulit | End-game content, Keranda |

Semakin dalam = HP lebih tinggi + speed lebih lambat = butuh tool dan pet yang lebih baik.

---

## Treasure (11 Items — Horror Theme)

### Layer 1 — Mud (Mudah)
| Treasure | Rarity | Chance | Harga Jual |
|----------|--------|--------|------------|
| Paku Karatan | Common | 60% | 3-8 coins |
| Tulang Tangan | Common | 40% | 3-8 coins |

### Layer 2 — Sand
| Treasure | Rarity | Chance | Harga Jual |
|----------|--------|--------|------------|
| Tulang Iga | Common | 55% | 5-12 coins |
| Tengkorak | Common | 45% | 5-12 coins |

### Layer 3 — Limestone
| Treasure | Rarity | Chance | Harga Jual |
|----------|--------|--------|------------|
| Boneka Sihir Hitam | Rare | 50% | 25-50 coins |
| Telur Dipaku | Rare | 50% | 25-50 coins |

### Layer 4 — Ground
| Treasure | Rarity | Chance | Harga Jual |
|----------|--------|--------|------------|
| Foto Buram | Rare | 55% | 30-60 coins |
| Kertas Ilmu Hitam | Epic | 45% | 75-140 coins |

### Layer 5 — Slate
| Treasure | Rarity | Chance | Harga Jual |
|----------|--------|--------|------------|
| Kertas Judi | Epic | 55% | 80-150 coins |
| Gigi Emas | Legendary | 45% | 250-450 coins |

### Layer 6 — Basalt (End-game)
| Treasure | Rarity | Chance | Harga Jual |
|----------|--------|--------|------------|
| Keranda | Legendary | 100% | 350-650 coins |

**Bonus:** Setiap treasure punya chance menjadi versi bermutasi dengan efek visual dan harga jual lebih tinggi.

---

## Sistem Mutasi (3 Jenis)

| Mutasi | Rarity | Chance | Multiplier | Efek Visual |
|--------|--------|--------|------------|-------------|
| Gold | Uncommon | 8% | 1.5x | Warna emas, highlight gold, sparkle particles, VFX Emas |
| Frozen | Uncommon | 6% | 1.3x | Warna biru es, material Ice, highlight biru, VFX Wind |
| Darkness | Rare | 3% | 1.8x | Warna gelap, highlight ungu, VFX Kegelapan |

Mutasi di-roll saat treasure spawn. Treasure bermutasi memiliki efek visual unik dan harga jual dikalikan multiplier.

---

## Tools (Cangkul)

| Tool | Damage | Speed | Harga | Rarity |
|------|--------|-------|-------|--------|
| Cangkul Karatan | Rendah | 1.0x | Gratis (starter) | Common |
| Cangkul Petani | Sedang | 1.2x | 100 coins | Uncommon |
| Cangkul Tulang | Tinggi | 1.5x | 500 coins | Rare |

---

## Pets

| Pet | Buff | Harga | Rarity |
|-----|------|-------|--------|
| Tuyul | +20% speed gali | 200 coins | Common |
| 3 Tuyul | +50% speed gali | 1,000 coins | Rare |

Pet mengikuti pemain dan memberikan buff otomatis saat diequip.

---

## Accessories (10 Items — 2 Set)

### Set Tengkorak (Damage +15% per item)
| Accessory | Slot | Harga | Rarity |
|-----------|------|-------|--------|
| Outfit Tengkorak | Outfit | 300 coins | Uncommon |
| Kalung Tengkorak | Kalung | 200 coins | Common |
| Aksesoris Tengkorak | Aksesoris | 250 coins | Common |
| Topeng Tengkorak | Headwear | 400 coins | Rare |
| Kain Kepala Tengkorak | Eyewear | 350 coins | Uncommon |

### Set Vampir (Speed +15% per item)
| Accessory | Slot | Harga | Rarity |
|-----------|------|-------|--------|
| Outfit Vampir | Outfit | 300 coins | Uncommon |
| Kalung Vampir | Kalung | 200 coins | Common |
| Aksesoris Vampir | Aksesoris | 250 coins | Common |
| Topi Vampir | Headwear | 400 coins | Rare |
| Kacamata Vampir | Eyewear | 350 coins | Uncommon |

**5 slot equip:** Outfit, Kalung, Aksesoris, Headwear, Eyewear. Buff stack aditif (5 item = +75%).
Outfit mengubah pakaian karakter, aksesoris lain ditambahkan sebagai Accessory ke karakter.

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

---

## Bestiary (Treasure Encyclopedia)

Koleksi 11 treasure yang melacak penemuan pemain:
- Item yang belum ditemukan tampil redup dengan nama "???"
- Filter berdasarkan layer: All, Mud, Sand, Limestone, Ground, Slate, Basalt
- Panel detail menampilkan: nama, rarity, range harga jual, status penemuan
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

## Rarity System

| Rarity | Warna | Frekuensi |
|--------|-------|-----------|
| Common | Abu-abu | Paling sering |
| Uncommon | Hijau | Sering |
| Rare | Biru | Jarang |
| Epic | Ungu | Sangat jarang |
| Legendary | Oranye | Paling langka |
