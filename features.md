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

## Sistem Accessory
- 2 set aksesoris: Tengkorak (Damage +15% per item), Vampir (Speed +15% per item)
- 5 slot per set: Outfit, Kalung, Aksesoris, Headwear, Eyewear
- Visual equip — outfit mengubah pakaian karakter, aksesoris lain ditambahkan ke karakter
- Buff stack secara aditif (contoh: 3 item damage = +45% damage)
- Equip/unequip melalui inventory UI

## Sistem Bestiary
- Ensiklopedia treasure yang melacak item yang sudah pernah ditemukan pemain
- 11 slot treasure tersusun berdasarkan layer
- Item yang belum ditemukan tampil redup dengan nama "???"
- Filter berdasarkan layer (All, Mud, Sand, Limestone, Ground, Slate, Basalt)
- Panel detail: nama, rarity, harga jual, status penemuan
- Data penemuan tersimpan permanen (DiscoveredItems)

## Daily Reward
- Sistem reward harian dengan siklus 7 hari
- Streak system: bonus meningkat setiap hari berturut-turut (50/75/100/150/200/300/500 coins)
- Cooldown 24 jam antar klaim, streak reset jika lebih dari 48 jam tidak klaim
- UI popup otomatis saat login
- Tombol "Hadiah Harian" di topbar (TopbarPlus v3) untuk membuka popup kapan saja

## Inventory & UI
- Menu inventory untuk Cangkul (lihat stats, equip/switch)
- Menu inventory untuk Pet (lihat buff, equip/unequip, storage info)
- Menu inventory untuk Accessory (lihat buff, equip/unequip per slot)
- Menu Bestiary (koleksi treasure, filter layer, detail panel)
- Sorting otomatis di semua container: item yang dipakai tampil pertama, lalu urut rarity tertinggi
- Notifikasi popup saat mendapat treasure baru (dengan stacking untuk item sama)
- Notifikasi hasil penjualan
- Notifikasi gagal beli (uang tidak cukup / sudah dimiliki) untuk semua jenis shop
- Notifikasi saat ganti Cangkul, equip/unequip Pet, equip/unequip Accessory
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

## Ordered Loader
- Sistem loading terurut saat pemain join: Cangkul → Pet → Accessory → Treasure
- Setiap fase menunggu fase sebelumnya selesai (menggunakan attribute signals)
- Timeout 15 detik per fase untuk mencegah hang
- Memastikan semua equipment terpasang sebelum pemain mulai bermain

## Data Persistence
- Auto-save setiap 60 detik
- Save saat pemain keluar
- Save saat server shutdown
- Data yang tersimpan: coins, tools, pets, accessories, treasure, equipped items, discovered items, daily reward streak

## Anti-Exploit
- Semua kalkulasi HP dan damage dilakukan di server
- Validasi timing untuk mencegah speed hack
- Validasi jarak (range check) untuk mencegah teleport hack
- Anti-drop tool system
- Server-side purchase validation

## Lokalisasi
- Seluruh teks GUI menggunakan Bahasa Indonesia
- Tombol, notifikasi, prompt pembelian, dan info panel semua dalam Bahasa Indonesia

## Quality of Life
- Underground lighting otomatis (lampu mengikuti pemain)
- Custom proximity prompt (menggantikan default Roblox)
- 3D cursor yang mengikuti posisi mouse di terrain
- Support keyboard, gamepad, dan touch input
