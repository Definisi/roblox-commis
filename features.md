# Fitur yang Sudah Selesai

## Sistem Digging (Core Gameplay)
- Terrain voxel yang bisa digali secara real-time
- 6 lapisan bumi dengan tingkat kesulitan berbeda (HP dan speed meningkat semakin dalam)
- Sistem dynamic generation — tanah baru muncul otomatis saat pemain menggali lebih dalam
- Visual terrain menggunakan material Roblox yang berbeda per lapisan
- Animasi dan efek partikel saat menggali
- Camera shake untuk feedback yang satisfying

## Sistem Treasure & Reward
- 30% chance mendapat treasure setiap kali menghancurkan blok tanah
- 11 jenis treasure horror-themed tersebar di 6 lapisan (2 per layer, Basalt 1)
- Sistem rarity: Common, Rare, Epic, Legendary
- Semakin dalam lapisan, semakin langka dan mahal treasure-nya
- Treasure muncul tertanam di tanah — terrain dummy mengisi voxel agar terlihat seperti tanah asli, treasure setengah nongol dari permukaan
- Highlight hijau saat menggali treasure
- Invisible hitplate (16x1x16) mencegah klik tembus ke voxel bawah
- Rotasi random pada setiap treasure agar terlihat natural
- Auto-despawn setelah 60 detik (beserta terrain dummy-nya)

## Sistem Mutasi (3 Jenis)
- 3 jenis mutasi: Gold (Uncommon, 8%, 1.5x), Frozen (Uncommon, 6%, 1.3x), Darkness (Rare, 3%, 1.8x)
- Setiap mutasi memiliki efek visual unik: color override, highlight glow, particle effects, VFX template
- Gold: warna emas + sparkle particles + highlight gold
- Frozen: material Ice + warna biru es + highlight biru + Wind VFX
- Darkness: warna gelap + highlight ungu + Kegelapan VFX
- Treasure bermutasi bernilai lebih tinggi sesuai multiplier masing-masing
- Mutasi di-roll secara random saat treasure spawn

## Sistem Tool (Cangkul)
- 3 tier Cangkul dengan damage dan speed berbeda
- Upgrade system: pemain membeli tool lebih baik dengan coins
- Setiap tool mempengaruhi kecepatan dan kekuatan gali
- Visual model berbeda untuk setiap tier

## Sistem Pet
- 2 jenis pet yang memberikan speed buff saat menggali
- Pet mengikuti pemain secara smooth dengan animasi bobbing
- Sistem equip/unequip melalui inventory UI
- Pet display di shop area dengan animasi idle (bobbing + spinning)

## Sistem Ekonomi
- Mata uang: Coins
- Jual treasure untuk mendapat coins (harga bervariasi per item dan rarity)
- Jual semua sekaligus atau satu per satu
- Harga jual memiliki range (min-max) untuk variasi
- Coins display di UI dengan format abbreviated (1.5K, 1M, dll)

## Shop System
- Toko Cangkul: 3 tool dengan harga berbeda
- Toko Pet: 2 pet dengan harga berbeda
- Proximity prompt — pemain mendekati display model dan muncul prompt pembelian
- Konfirmasi pembelian dengan detail stats dan harga
- Validasi server-side (anti-cheat untuk pembelian)

## Inventory & UI
- Menu inventory untuk Cangkul (lihat stats, equip/switch)
- Menu inventory untuk Pet (lihat buff, equip/unequip, storage info)
- Notifikasi popup saat mendapat treasure baru (dengan stacking untuk item sama)
- Notifikasi hasil penjualan
- HP bar dan charge bar saat menggali
- Target info (nama layer/treasure yang sedang ditarget)

## Sistem Teleport
- Tombol "Return to Surface" saat di underground
- Tombol "Return to Last Dig" saat di surface
- Memudahkan navigasi antara permukaan dan area gali terakhir

## NPC Dialog System
- Sistem dialog dengan efek typewriter
- Support multiple dialog options dan response choices
- Interaksi berbasis proximity

## Data Persistence
- Auto-save setiap 60 detik
- Save saat pemain keluar
- Save saat server shutdown
- Data yang tersimpan: coins, tools yang dimiliki, pet yang dimiliki, treasure di inventory, equipment yang sedang dipakai

## Anti-Exploit
- Semua kalkulasi HP dan damage dilakukan di server
- Validasi timing untuk mencegah speed hack
- Validasi jarak (range check) untuk mencegah teleport hack
- Anti-drop tool system
- Server-side purchase validation

## Quality of Life
- Underground lighting otomatis (lampu mengikuti pemain)
- Custom proximity prompt (menggantikan default Roblox)
- 3D cursor yang mengikuti posisi mouse di terrain
- Support keyboard, gamepad, dan touch input
