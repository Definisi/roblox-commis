# Reward System

## Overview
- 30% chance to spawn reward when terrain block destroyed (`Config.Reward.SpawnChance = 0.3`)
- Rewards have HP=30, Speed=1, DespawnTime=60s
- Each reward model: Visual (visible mesh) + Block (invisible, CanQuery=true for raycast)
- 3 mutations: Racun (5%, 1.3x, green), Darah (3%, 1.5x, red), SihirHitam (2%, 2.0x, purple)
- Mutation chances boosted by player accessory buffs (additive: base + AccessoryBuff/100)
- Legacy `Wood/` folder exists (3 models) but is unused — no layer maps to it

## Reward Templates (`ServerStorage.RewardTemplates`)
Each layer folder has 1-2 Models with Register ModuleScript (Basalt has 1, others have 2).
Register returns: `{ id, name, rarity, chance, layer }` — all Registers now include `layer` field (standardized).

Chance is weighted (out of 100 cumulative). All new items are horror-themed treasures from Workspace.Items.

### Mud Layer (top)
| Model | ID | Name | Rarity | Chance | Sell Price |
|-------|----|------|--------|--------|------------|
| Reward1 | 3001 | Paku Karatan | Common (1) | 60% | 3-8 |
| Reward2 | 3002 | Tulang Tangan | Common (1) | 40% | 3-8 |

### Sand Layer
| Model | ID | Name | Rarity | Chance | Sell Price |
|-------|----|------|--------|--------|------------|
| Reward1 | 3003 | Tulang Iga | Common (1) | 55% | 5-12 |
| Reward2 | 3004 | Tengkorak | Common (1) | 45% | 5-12 |

### Limestone Layer
| Model | ID | Name | Rarity | Chance | Sell Price |
|-------|----|------|--------|--------|------------|
| Reward1 | 3005 | Boneka Sihir Hitam | Rare (3) | 50% | 25-50 |
| Reward2 | 3006 | Telur Dipaku | Rare (3) | 50% | 25-50 |

### Ground Layer
| Model | ID | Name | Rarity | Chance | Sell Price |
|-------|----|------|--------|--------|------------|
| Reward1 | 3007 | Foto Buram | Rare (3) | 55% | 30-60 |
| Reward2 | 3008 | Kertas Ilmu Hitam | Epic (4) | 45% | 75-140 |

### Slate Layer
| Model | ID | Name | Rarity | Chance | Sell Price |
|-------|----|------|--------|--------|------------|
| Reward1 | 3009 | Kertas Judi | Epic (4) | 55% | 80-150 |
| Reward2 | 3010 | Gigi Emas | Legendary (5) | 45% | 250-450 |

### Basalt Layer
| Model | ID | Name | Rarity | Chance | Sell Price |
|-------|----|------|--------|--------|------------|
| Reward1 | 3011 | Keranda | Legendary (5) | 100% | 350-650 |

## Mutation System
- 3 mutations (only 1 per reward, checked in order):
  - **Racun** (Poison): 5% base + AccessoryBuff_mutasi_racun/100, 1.3x sell, green VFX, Uncommon
  - **Darah** (Blood): 3% base + AccessoryBuff_mutasi_darah/100, 1.5x sell, red VFX, Rare
  - **SihirHitam** (Black Magic): 2% base + AccessoryBuff_mutasi_sihir_hitam/100, 2.0x sell, purple VFX, Epic
- On spawn: DigServer.spawnReward(pos, layer, player) rolls each mutation with player buffs
- If triggered: clone `workspace.VFX.[Racun|Darah|SihirHitam]` → parent to reward parts, apply color/material/highlight/sparkle, set `Mutation` attribute
- On pickup: InventoryTreasure.AddTreasure() reads Mutation attribute → applies sellMultiplier
- Stored in TreasureEntry.mutation field for persistence
- Luck buff (AccessoryBuff_luck) increases treasure spawn chance multiplicatively: spawnChance * (1 + luck/100)

## Sell Price Calculation
```
baseSellPrice = math.random(itemConfig.sellPrice.min, itemConfig.sellPrice.max)
if mutation:
    finalSellPrice = math.floor(baseSellPrice * mutationConfig.sellMultiplier)
else:
    finalSellPrice = baseSellPrice
```
