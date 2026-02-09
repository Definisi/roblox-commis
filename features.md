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
- 18 jenis treasure unik tersebar di 6 lapisan
- Sistem rarity: Common, Uncommon, Rare, Epic, Legendary
- Semakin dalam lapisan, semakin langka dan mahal treasure-nya
- Treasure muncul sebagai objek 3D yang harus digali lagi untuk diklaim
- Auto-despawn setelah 60 detik jika tidak diambil

## Sistem Mutasi "Darkness"
- 5% chance treasure yang spawn memiliki mutasi spesial
- Efek visual gelap (particle effect) pada treasure yang termutasi
- Treasure bermutasi bernilai 2x lipat harga jual normal
- Menambah excitement dan element of surprise

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
