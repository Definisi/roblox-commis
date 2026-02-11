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

## Rarity System

| Rarity | Warna | Frekuensi |
|--------|-------|-----------|
| Common | Abu-abu | Paling sering |
| Uncommon | Hijau | Sering |
| Rare | Biru | Jarang |
| Epic | Ungu | Sangat jarang |
| Legendary | Oranye | Paling langka |
