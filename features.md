# Fitur yang Sudah Selesai

## Sistem Digging (Core Gameplay)
- Terrain voxel yang bisa digali secara real-time
- 6 lapisan bumi dengan tingkat kesulitan berbeda (HP dan speed meningkat semakin dalam)
- Sistem dynamic generation — tanah baru muncul otomatis saat pemain menggali lebih dalam
- Visual terrain menggunakan material Roblox yang berbeda per lapisan
- Animasi dan efek partikel saat menggali
- Camera shake untuk feedback yang satisfying

## Sistem Treasure & Reward
- 30% chance mendapat treasure setiap kali menghancurkan blok tanah (dipengaruhi luck buff & Server Luck)
- 11 jenis treasure horror-themed tersebar di 6 lapisan (2 per layer, Basalt 1)
- Sistem rarity: Common, Rare, Epic, Legendary
- Semakin dalam lapisan, semakin langka dan mahal treasure-nya
- Treasure muncul tertanam di tanah — terrain dummy mengisi voxel agar terlihat seperti tanah asli
- Highlight hijau saat menggali treasure
- Invisible hitplate (16x1x16) mencegah klik tembus ke voxel bawah
- Rotasi random pada setiap treasure agar terlihat natural
- Auto-despawn setelah 60 detik (beserta terrain dummy-nya)
- Proteksi bag full: treasure tetap spawn, tapi tidak bisa diambil jika tas penuh (20/20). HP reward di-restore agar bisa diambil setelah tas dikosongkan

## Sistem Mutasi (3 Jenis)
- 3 jenis mutasi: Racun (5%, 1.3x, hijau), Darah (3%, 1.5x, merah), SihirHitam (2%, 2.0x, ungu)
- Chance mutasi bisa ditambah dengan buff aksesoris (additive per jenis mutasi)
- Setiap mutasi memiliki efek visual unik: color override, highlight glow, particle effects, VFX template
- Racun: warna hijau + highlight + poison VFX
- Darah: warna merah + highlight + blood VFX (2 emitters)
- SihirHitam: warna ungu gelap + highlight + dark magic VFX
- Treasure bermutasi bernilai lebih tinggi sesuai multiplier masing-masing
- Mutasi di-roll secara random saat treasure spawn (first hit wins)

## Sistem Tool (Cangkul)
- 3 tier Cangkul dengan damage dan speed berbeda
- Upgrade system: pemain membeli tool lebih baik dengan coins
- Setiap tool mempengaruhi kecepatan dan kekuatan gali
- Visual model berbeda untuk setiap tier

## Sistem Pet
- Multi-equip: hingga 3 pet sekaligus
- Slot positioning: right, left, behind (posisi berbeda per slot)
- Buff stacking aditif: semua buff pet yang diequip dijumlahkan
- Pet mengikuti pemain secara smooth dengan animasi bobbing
- Sistem equip/unequip melalui inventory UI dengan 3D preview di EquipSlots
- Pet display di shop area dengan animasi idle (bobbing + spinning)
- Migrasi otomatis dari sistem single-equip lama ke multi-equip

## Sistem Ekonomi
- Mata uang: Coins
- Jual treasure untuk mendapat coins (harga bervariasi per item dan rarity)
- Jual semua sekaligus (preserves favorites) atau jual satu/batch
- Harga jual memiliki range (min-max) untuk variasi
- Coins display di UI dengan format abbreviated (1.5K, 1M, dll)

## Shop System
- Toko Cangkul: 3 tool dengan harga berbeda
- Toko Pet: 2 pet dengan harga berbeda
- Toko Aksesoris: 10 aksesoris (2 set × 5 item)
- Proximity prompt — pemain mendekati display model dan muncul prompt pembelian
- Konfirmasi pembelian dengan detail stats dan harga
- Validasi server-side (anti-cheat untuk pembelian)
- Notifikasi gagal beli: "Koin tidak cukup!" / "Sudah dimiliki!" di semua shop

## Sistem Accessory
- 2 set aksesoris: Tengkorak (Damage focus), Vampir (Speed focus)
- 5 slot: Topi, Muka, Leher, Pinggang, Baju+Celana
- 5 item per set, masing-masing memberi multi-stat buff (damage/speed/luck/mutasi_racun/mutasi_darah/mutasi_sihir_hitam)
- Full set bonus (5 pieces same set): Tengkorak +30% Damage +15% Racun, Vampir +30% Speed +15% Darah
- Visual equip — outfit mengubah pakaian karakter, aksesoris ditambahkan ke karakter
- Buff dihitung sebagai persentase dan diterapkan via player attributes
- Reapply otomatis saat respawn

## Sistem Tempa (Upgrade/Forging)
- Pandai Besi (Anvil) di dunia game — mendekat untuk membuka UI Tempa via ProximityPrompt
- Upgrade Cangkul: max +10 level, setiap level menambah damage dan speed flat
- Upgrade Aksesoris: max +5 level, setiap level menambah 30% dari base buff
- Biaya upgrade: coins + treasure material dari tas pemain
- UI menampilkan preview stats (sebelum → sesudah), biaya, ketersediaan material
- Notifikasi berhasil/gagal tempa
- Data tempa tersimpan permanen

## Sistem Bestiary
- Ensiklopedia treasure yang melacak item yang sudah pernah ditemukan pemain
- 11 slot treasure tersusun berdasarkan layer
- Item yang belum ditemukan tampil redup dengan nama "???"
- Filter berdasarkan layer (All, Mud, Sand, Limestone, Ground, Slate, Basalt)
- Panel detail: nama, rarity, harga jual, 3D preview, status penemuan
- Data penemuan tersimpan permanen (DiscoveredItems)

## Custom Inventory & Hotbar
- Custom Hotbar: 9 slot (slot 1 = Cangkul, slot 2-9 = treasure)
- Number keys 1-9 atau klik untuk equip/unequip
- Slot kosong otomatis tersembunyi
- Tas (bag): max 20 item, favorites persist saat Jual Semua
- InventarisFrame: grid view dengan sorting (equipped first, lalu rarity tertinggi)
- Sell single/batch, favorite toggle, hotbar assign/remove
- InventoryButton di hotbar untuk toggle buka tas
- Semua aksi via TreasureInventoryEvent → server validation

## Daily Reward
- Sistem reward harian dengan siklus 7 hari
- Streak system: bonus meningkat setiap hari berturut-turut (50/75/100/150/200/300/500 coins)
- Cooldown 24 jam antar klaim, streak reset jika lebih dari 48 jam tidak klaim
- UI popup otomatis saat login
- Tombol "Hadiah Harian" di topbar (TopbarPlus v3) untuk membuka popup kapan saja

## Buffs GUI
- Display dinamis di kiri bawah layar menampilkan buff aktif
- Hanya menampilkan buff yang > 0%
- Menggabungkan PetBuff + AccessoryBuff attributes
- Auto-rebuild saat buff berubah (9 attribute change signals)
- Buff types: damage, speed, luck, mutasi_racun, mutasi_darah, mutasi_sihir_hitam

## ViewportIcon System
- 3D model preview di semua inventory UI (Hotbar, Inventaris, Bestiary, MenuController)
- Menggunakan IconModels dari ReplicatedStorage (Treasures & Cangkuls)
- Auto-fit camera berdasarkan bounding box, sudut pandang konsisten

## Admin Panel
- Autentikasi: group rank (group 35484105, rank >= 2)
- Keybind: Semicolon (;) toggle panel
- UI tab-based (3 tab):
  - **Server Luck**: ON/OFF toggle, multiplier selector (x2/x4/x8), durasi (15m/30m/1h/2h), Apply/Reset
  - **Cari Pemain**: search box, header pemain (avatar+nama+status), 6 sub-tab kategori (Info/Cangkul/Pets/Aksesoris/Tas/Tempa), grid container + detail panel dengan ViewportFrame 3D preview
  - **Ban & Kick**: target display, ban (Perma/1 Bulan/1 Tahun/Unban), kontrol (Kick/TP ke Admin/TP ke Pemain)
- Self-ban/kick prevention
- Player lookup: online (DataSave cache) dan offline (DataStore GetAsync)
- Ban system: BanList_v1 DataStore, dicek saat PlayerAdded

## Server Luck
- Admin bisa set multiplier luck server (x2/x4/x8) dengan durasi
- Weekend otomatis x2 setiap Sabtu & Minggu (GMT+7 / WIB)
- Admin override: luck admin > weekend luck, saat expire fallback ke weekend jika masih weekend
- Semua pemain bisa lihat indikator Server Luck di kanan bawah (di atas coins)
- Color-coded: hijau (x2), ungu (x4), emas (x8)
- workspace.ServerLuckMultiplier attribute — dibaca oleh DigServer di perhitungan spawn chance

## Inventory & UI
- Menu inventory untuk Cangkul (lihat stats, equip/switch)
- Menu inventory untuk Pet (lihat buff, equip/unequip, 3D preview, storage info, max 3 equip slots)
- Menu inventory untuk Accessory (lihat buff, equip/unequip per slot)
- Menu Bestiary (koleksi treasure, filter layer, detail panel)
- Menu Inventaris/Tas (grid view, sell, favorite, hotbar assign)
- Sorting otomatis di semua container: item yang dipakai tampil pertama, lalu urut rarity tertinggi
- Notifikasi popup saat mendapat treasure baru (dengan stacking untuk item sama)
- Notifikasi hasil penjualan, bag full, favorite lock/unlock
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
- Sistem loading terurut saat pemain join: Cangkul → Pet → Accessory → Treasure + SyncBag
- Setiap fase menunggu fase sebelumnya selesai (menggunakan attribute signals)
- Timeout 15 detik per fase untuk mencegah hang
- Memastikan semua equipment terpasang sebelum pemain mulai bermain
- Respawn: SyncBag fired ulang via CharacterAdded

## Data Persistence
- Auto-save setiap 60 detik
- Save saat pemain keluar
- Save saat server shutdown
- Data yang tersimpan: coins, tools, pets (multi-equip), accessories (5 slot), treasure bag, equipped items, discovered items, daily reward streak, favorites, hotbar slots, tempa levels (cangkul + accessory)

## Anti-Exploit
- Semua kalkulasi HP dan damage dilakukan di server
- Validasi timing untuk mencegah speed hack
- Validasi jarak (range check) untuk mencegah teleport hack
- Anti-drop tool system
- Server-side purchase validation
- Bag size enforcement (server-side, double safety check)
- Admin panel: server-side auth check on every request

## Lokalisasi
- Seluruh teks GUI menggunakan Bahasa Indonesia
- Tombol, notifikasi, prompt pembelian, dan info panel semua dalam Bahasa Indonesia

## Quality of Life
- Underground lighting otomatis (lampu mengikuti pemain)
- Custom proximity prompt (menggantikan default Roblox) — semua prompt: Cangkul, Pet, Sell, Accessory, Anvil
- 3D cursor yang mengikuti posisi mouse di terrain
- Support keyboard, gamepad, dan touch input
- Server Luck indicator visible untuk semua pemain
