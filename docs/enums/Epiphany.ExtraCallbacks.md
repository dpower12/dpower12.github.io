# Enum "ExtraCallbacks"

This is a list of all new callbacks added by Epiphany. Instead of using your mod reference, call the function `Epiphany:AddExtraCallback` or `Epiphany:AddExtraPriorityCallback` with the same parameters. The enums themselves are accessible through `Epiphany.ExtraCallbacks`.

## Callbacks

### TURNOVER_GET_PICKUP_POOL {: .copyable }

Called when getting the pickup pool of a Turnover shop, which defines what pickups can be chosen to spawn as a shop item. Accepts returning a new pool to override it.

See [this guide on Turnover shops](../guides/turnover_shops.md) for more information.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|TURNOVER_GET_PICKUP_POOL {: .copyable } | (table Pool, integer currentTier, [RoomType](https://wofsauge.github.io/IsaacDocs/rep/enums/RoomType.html)) | - | table PickupPool |

### TURNOVER_GET_LAYOUT_INFO {: .copyable }

Called when getting the layout of a Turnover shop, which defines positions of shop items and what kind of item will be sold. Accepts returning a new layout to override it.

See [this guide on Turnover shops](../guides/turnover_shops.md) for more information.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|TURNOVER_GET_LAYOUT_INFO {: .copyable } | (table ShopLayout, integer CurrentTier, [RoomType](https://wofsauge.github.io/IsaacDocs/rep/enums/RoomType.html)) | - | table ShopLayout |

### TURNOVER_GET_PRICE {: .copyable }

Called for each player with Turnover every game update to refresh the price of setting up a shop.

Accepts returning a new price, or `false` to indicate that a shop cannot be set up and prevent later callback functions from running.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|TURNOVER_GET_PRICE {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), integer ShopPrice, integer OriginalPrice) | - | integer or boolean |

### TURNOVER_POST_CREATE_SHOP {: .copyable }

Called after all the shop items are spawned for a Turnover shop. `SpawnedItems` passes an array of all the entities spawned by Turnover, including slot machines and the shopkeeper. Pickups and Collectibles are already cast to [EntityPickup](https://repentogon.com/EntityPickup.html).

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|TURNOVER_POST_CREATE_SHOP {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [Entity](https://repentogon.com/Entity.html)[] SpawnedItems) | - | void |

### PRE_UNLOCK_CACHE {: .copyable }

Called on MC_POST_GAME_STARTED on LATE priority. Passes a table with `Items` and `Cards` as variables. Used to define if modded items/cards are defined or not.

???- note "REPENTOGON achievements"
	If your mod uses REPENTOGON achievements, this callback isn't necessary, as it will properly lock the item inside the vanilla game. If not cache'd already and there is an achievement ID tied to it, Epiphany will check `ItemConfig[Item/Card]:IsAvailable()`, then cache the result.

The table can be modified to have your [CollectibleType](https://wofsauge.github.io/IsaacDocs/rep/enums/CollectibleType.html) or Card ID as the key, and a boolean for setting if they're unlocked or not.

???+ example "Example Code"
	```Lua
		local FIRST_ITEM = Isaac.GetItemIdByName("My First Item")
		local LAST_ITEM = Isaac.GetItemIdByName("My Last Item")
		local FIRST_CARD = Isaac.GetCardIdByName("My First Card")
		local LAST_CARD = Isaac.GetCardIdByName("My Last Card")
		Epiphany:AddExtraCallbacks(Epiphany.ExtraCallbacks.PRE_UNLOCK_CACHE, function(_, cacheUnlocks)
			for i = FIRST_ITEM, LAST_ITEM do
				cachedUnlocks.Items[i] = true
			end
			for i = FIRST_CARD, LAST_CARD do
				cachedUnlocks.Cards[i] = true
			end
		end)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PRE_UNLOCK_CACHE {: .copyable } | (table Unlocks) | - | void |

### POST_ACHIEVEMENT_GET {: .copyable }

Called after an achievement from Epiphany is obtained. `Value` returns `1` for Normal Mode, `2` for Hard Mode. Does not run for achievements obtained through cheats. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_ACHIEVEMENT_GET {: .copyable } | (string AchievementName, integer Value) | string AchievementName | void |

### CAIN_POST_BAG_ITEM {: .copyable }

Called after an item is bagged with Throwing Bag. Accepts no return parameters.

???+ warning "Save data"
	The passed [BagsInfo](../classes/BagsInfo.md) object is mutable and will reflect inside Epiphany's save data.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CAIN_POST_BAG_ITEM {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [BagsInfo](../classes/BagsInfo.md), [CollectibleType](https://wofsauge.github.io/IsaacDocs/rep/enums/CollectibleType.html) ID, boolean IsGolden) | [CollectibleType](https://wofsauge.github.io/IsaacDocs/rep/enums/CollectibleType.html) | void |

### CAIN_POST_CREATE_BAG {: .copyable }

Called after a new Throwing Bag is created. Accepts no return parameters.

???+ warning "Save data"
	The passed [BagsInfo](../classes/BagsInfo.md) object is mutable and will reflect inside Epiphany's save data.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CAIN_POST_CREATE_BAG {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [BagsInfo](../classes/BagsInfo.md), [BagData](../classes/BagData.md), string NewBagID) | - | void |

### CAIN_POST_SWING_HIT {: .copyable }

Called after a throwing bag currently being swung hits an enemy. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CAIN_POST_SWING_HIT {: .copyable } | ([EntityEffect](https://repentogon.com/EntityEffect.html) SwingBag, [Entity](https://repentogon.com/Entity.html) Enemy, [EntityPlayer](https://repentogon.com/EntityPlayer.html), SwingingBagData, float FinalDamage) | - | void |

### CAIN_POST_BAG_THROW {: .copyable }

Called after a throwing bag is thrown. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CAIN_POST_BAG_THROW {: .copyable } | ([EntityEffect](https://repentogon.com/EntityEffect.html) ThrownBag, [BagData](../classes/BagData.md)) | - | void |

### CAIN_POST_BAG_HIT {: .copyable }

Called after a throwing bag that was thrown hits an enemy.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CAIN_POST_BAG_HIT {: .copyable } | ([EntityEffect](https://repentogon.com/EntityEffect.html) ThrownBag, [Entity](https://repentogon.com/Entity.html) Enemy, ThrownBagData, float FinalDamage) | - | void |

### SAMSON_PUNCH_ENTITY {: .copyable }

Called after Tarnished Samson hits an enemy with a punch or a slam. Accepts no return parameters.

- `Incubus` passes [EntityFamiliar](https://repentogon.com/EntityFamiliar.html) if the attacker is a weapon-copying familiar like Incubus. Otherwise, passes `nil`
- `Hand`: Returns `"Left"` or `"Right"` depending on the hand hit the enemy. If `isSlam` is `true`, passes `nil` instead.
- `Point1`: The first point of collision between the enemy and player attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PUNCH_ENTITY {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [EntityNPC](https://repentogon.com/EntityNPC.html) Enemy, boolean isSlam, [Vector](https://repentogon.com/Vector.html) point1, [EntityFamiliar](https://repentogon.com/EntityFamiliar.html)? Incubus, string? Hand, number DmgDealt) | - | void |

### SAMSON_PUNCH_GRID {: .copyable }

Called after Tarnished Samson hits an enemy with a punch or a slam. Only gets called for doors, poop, rocks and moveable TNT. Accepts no return parameters.

- `Incubus` passes [EntityFamiliar](https://repentogon.com/EntityFamiliar.html) if the attacker is a weapon-copying familiar like Incubus. Otherwise, passes `nil`
- `Hand`: Returns `"Left"` or `"Right"` depending on the hand hit the enemy. If `isSlam` is `true`, passes `nil` instead.
- `Point1`: The first point of collision between the grid and player attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PUNCH_GRID {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [GridEntity](https://repentogon.com/GridEntity.html), boolean isSlam, [Vector](https://repentogon.com/Vector.html) point1, [EntityFamiliar](https://repentogon.com/EntityFamiliar.html)? Incubus, string? Hand) | - | void |

### SAMSON_SLAM {: .copyable }

Called after Tarnished Samson performs a slam attack and after boulders are dropped, but before grids and entities are damaged. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_SLAM {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), number SlamRange) | - | void |

### SAMSON_DASH_HIT_ENEMY {: .copyable }

Called after a player performs a dash with Killer Instinct and hits an enemy. `Point1` is the first point of collision between grid and player attack. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_DASH_HIT_ENEMY {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [EntityNPC](https://repentogon.com/EntityNPC.html) Enemy, [Vector](https://repentogon.com/Vector.html) Point1) | - | void |

### SAMSON_DASH_HIT_WALL {: .copyable }

Called after a player performs a dash with Killer Instinct and hits a grid or wall. `Point1` is the first point of collision between grid and player attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_DASH_HIT_WALL {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [Vector](https://repentogon.com/Vector.html) Point1) | - | void |

### SAMSON_PRE_PUNCH {: .copyable }

Called after Tarnished Samson picks out entities in his hitbox range, but before they get damaged.

Accepts returning an array of entities to override what entities are hit by the attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_PUNCH {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [Vector](https://repentogon.com/Vector.html) AttackDirection, [Entity](https://repentogon.com/Entity.html)[] EntityList, [EntityFamiliar](https://repentogon.com/EntityFamiliar.html)? Incubus) | - | [Entity](https://repentogon.com/Entity.html)[] |

### SAMSON_BOULDER_DEAD {: .copyable }

Called after a boulder dies.

Collider passes [Entity](https://repentogon.com/Entity.html) if detected that the boulder hit an entity before dying. Otherwise, passes `nil`.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_BOULDER_DEAD {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [Entity](https://repentogon.com/Entity.html)? Collider, number DamageAmount, [TearFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/TearFlags.html)) | - | void |

### SAMSON_PRE_HITBOX_GENERATE {: .copyable }

Called after a hitbox is generated for Tarnished Samson's punches, but before it is used to detect what collides with.

Accepts returning an array of tables containing `Hitbox` and `Direction` to append additional hitboxes with the original one. Optionally, a boolean can be a second return value to prevent the original hitbox from being used.

Hitboxes can be made with `Epiphany.Character.SAMSON:MakePunchHitbox([EntityPlayer](https://repentogon.com/EntityPlayer.html), number PunchLength, number PunchWidth, [Vector](https://repentogon.com/Vector.html) AimDirection, [EntityFamiliar](https://repentogon.com/EntityFamiliar.html)? Incubus, [Vector](https://repentogon.com/Vector.html)? Forced Position): SamsonHitboxData`.

???+ example "Example Code"
	```Lua
	Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.SAMSON_PRE_HITBOX_GENERATE, function(_, player, hitbox, familiar)
		--TODO: Actually write something here
	end)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_HITBOX_GENERATE {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), SamsonHitboxData Hitbox, [EntityFamiliar](https://repentogon.com/EntityFamiliar.html)? Incubus) | - | table or boolean |

### SAMSON_POST_THROW_BOULDER {: .copyable }

Called after a player through a boulder.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_POST_THROW_BOULDER {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [EntityEffect](https://repentogon.com/EntityEffect.html) Boulder) | - | void |

### SAMSON_PRE_DAMAGE_ENTITY {: .copyable }

Called between [SAMSON_PRE_PUNCH](#samson_pre_punch) and [SAMSON_PUNCH_ENTITY](#samson_punch_entity), right before damage is dealt to an entity.

- `Incubus` passes [EntityFamiliar](https://repentogon.com/EntityFamiliar.html) if the attacker is a weapon-copying familiar like Incubus. Otherwise, passes `nil`
- `Hand`: Returns `"Left"` or `"Right"` depending on the hand hit the enemy. If `isSlam` is `true`, passes `nil` instead.
- `Point1`: The first point of collision between the enemy and player attack.
- `PunchOrigin`: Origin of the punch hitbox. Not the center of the position, but where it originates from.  If `isSlam` is `true`, passes `nil` instead.

Accepts returning `-1` to prevent damage or a table of any combination of the following variables to modify the damage:

- Multiply: Stacks additively with other `Multiply` modifiers,
- MultiplyExponential: Stacks exponentially with other `MultiplyExponential` modifiers,
- AddFlat: Applied after multipliers,
- [DamageFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/DamageFlag.html): Adds [[TearFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/TearFlags.html)](https://wofsauge.github.io/IsaacDocs/rep/enums/[TearFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/TearFlags.html).html) to the damage.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_DAMAGE_ENTITY {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [EntityNPC](https://repentogon.com/EntityNPC.html) Enemy, boolean IsSlam, [Vector](https://repentogon.com/Vector.html) Point1, [EntityFamiliar](https://repentogon.com/EntityFamiliar.html)? Incubus, string Hand, [Vector](https://repentogon.com/Vector.html) PunchOrigin) | - | `-1` or table |

### SAMSON_PRE_DAMAGE_ENTITY_DASH {: .copyable }

Called before [SAMSON_DASH_HIT_ENEMY](#samson_dash_hit_enemy) and before damage is dealt. `Point1` is the first point of collision between the enemy and player attack.

Accepts returning `-1` to prevent damage or a table of any combination of the following variables to modify the damage:

- Multiply: Stacks additively with other `Multiply` modifiers,
- MultiplyExponential: Stacks exponentially with other `MultiplyExponential` modifiers,
- AddFlat: Applied after multipliers,
- [DamageFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/DamageFlag.html): Adds [[TearFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/TearFlags.html)](https://wofsauge.github.io/IsaacDocs/rep/enums/[TearFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/TearFlags.html).html) to the damage.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_DAMAGE_ENTITY_DASH {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [EntityNPC](https://repentogon.com/EntityNPC.html) Enemy, Point1) | - | `-1`  or table|

### SAMSON_POST_HITBOX_GENERATE {: .copyable }

Called after [SAMSON_PRE_HITBOX_GENERATE](#samson_pre_hitbox_generate) for each hitbox. Accepts returning a new `SamsonHitboxData` object to override the generated hitbox.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_POST_HITBOX_GENERATE {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [EntityFamiliar](https://repentogon.com/EntityFamiliar.html)? Incubus, SamsonHitboxData Hitbox) | - | SamsonHitboxData |

### SAMSON_PRE_BOULDER_SPRITE_INIT {: .copyable }

Called before a stationary or thrown boulder has its sprite initialized.

- `Tag`: A table that can hold arbitrary data for the purposes of synergies.
- `Variant`: Can either be `1`, `2`, or `3`, corresponding to which spritesheet it should utilize for the boulder.

Accepts returning a table of any combination of the following variables to modify the boulder sprite:

- `Anm2`: String path to the anm2 to use for the boulder, or `nil` to use the default.
- `ThrownAnm2`: String path to the anm2 to use for the boulder when it's thrown, or `nil` to use the default.
- `Spritesheet`: String path to the spritesheet to use for the boulder.
- `Priority`: Number denoting how important this sprite is. Higher numbers override lower numbers.
- `DontUpdate`: Set to `true` to not update the spritesheet animations at all.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_BOULDER_SPRITE_INIT {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), integer Variant, table Tag, [EntityEffect](https://repentogon.com/EntityEffect.html) Boulder or [EntityTear](https://repentogon.com/EntityTear.html) Boulder) | - | table |

### SAMSON_PRE_CONSUME_HEART {: .copyable }

Called before Tarnished Samson consumes a heart pickup for Slaughter Bar tokens while in Immortal Rage.

Accepts returning an integer to change the number of tokens gained from the heart, or `false` to prevent consuming the heart.

Can pass up to two optional arguments inside the AddExtraCallback function.

???- example "Example Code"
	Runs a SAMSON_PRE_CONSUME_HEART callback only for Soul Hearts:

	```Lua
	Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.SAMSON_PRE_CONSUME_HEART, function(_, player, heart, tokens)

	end, PickupVariant.PICKUP_HEART, HeartSubType.HEART_SOUL)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_CONSUME_HEART {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [EntityPickup](https://repentogon.com/EntityPickup.html) Heart, integer TokensGained) | [PickupVariant](https://wofsauge.github.io/IsaacDocs/rep/enums/PickupVariant.html), [HeartSubType](https://wofsauge.github.io/IsaacDocs/rep/enums/HeartSubType.html) | integer or boolean |

### MAGDALENE_PRE_CONSUME_HEART {: .copyable }

Called before Tarnished Magdalene consumes a heart pickup for her bonus heart meter. Restricted to red hearts.

Accepts returning an integer to change the number of bonus hearts gained from the heart, or `false` to prevent consuming the heart.

Can pass up to two optional arguments inside the AddExtraCallback function.

???- example "Example Code"
	Runs a MAGDALENE_PRE_CONSUME_HEART callback only for Soul Hearts:

	```Lua
	Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.MAGDALENE_PRE_CONSUME_HEART, function(_, player, heart, bonusHearts)

	end, PickupVariant.PICKUP_HEART, HeartSubType.HEART_FULL)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|MAGDALENE_PRE_CONSUME_HEART {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [EntityPickup](https://repentogon.com/EntityPickup.html) Heart, integer BonusHeartsGained) | [PickupVariant](https://wofsauge.github.io/IsaacDocs/rep/enums/PickupVariant.html), [HeartSubType](https://wofsauge.github.io/IsaacDocs/rep/enums/HeartSubType.html) | integer or boolean |

### PLAYER_DAMAGED_ENTITY {: .copyable }

Called when the player has inflicted damage onto an entity.

Triggers on [MC_ENTITY_TAKE_DMG](https://repentogon.com/enums/ModCallbacks.html#mc_entity_take_dmg) if:
- The `source` is the player, or was a source spawned by the player
- The hit entity is an [EntityNPC](https://repentogon.com/EntityNPC.html)
- [EntityNPC](https://repentogon.com/EntityNPC.html):IsVulnerableEnemy() returns `true`

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PLAYER_DAMAGED_ENTITY {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [Entity](https://repentogon.com/Entity.html), number Amount, [DamageFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/DamageFlag.html)) | - | void |

### SLOT_ON_DEATH {: .copyable }

Called after a slot machine dies.

Triggers on [MC_POST_SLOT_UPDATE](https://repentogon.com/enums/ModCallbacks.html#mc_post_slot_update) the first time that [EntitySlot:GetState()](https://repentogon.com/EntitySlot.html#getstate) returns [SlotState.DESTROYED](https://repentogon.com/enums/SlotState.html)

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SLOT_ON_DEATH {: .copyable } | ([EntitySlot](https://repentogon.com/EntitySlot.html)) | [SlotVariant](https://repentogon.com/enums/SlotVariant.html) | void |

### POST_GRID_UPDATE {: .copyable }

Called every update frame for rocks, doors, and trapdoors. Accepts no return parameters.

Can pass up to two optional arguments inside the AddExtraCallback function.

???- example "Example Code"
	Runs a POST_GRID_UPDATE callback only for Twisted Rock:

	```Lua
	Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.POST_GRID_UPDATE, function(_, gridEnt)

	end, GridEntityType.GRID_ROCK, Epiphany.Grid.TWISTED_ROCK.ID)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_GRID_UPDATE {: .copyable } | ([GridEntity](https://repentogon.com/GridEntity.html)) | [GridEntityType](https://wofsauge.github.io/IsaacDocs/rep/enums/GridEntityType.html), integer GridVariant | void |

### POST_GRID_DESTROY {: .copyable }

Called once a specified grid entity is destroyed. Accepts no return parameters.

Can pass up to two optional arguments inside the AddExtraCallback function.

???- example "Example Code"
	Runs a POST_GRID_DESTROY callback only for Twisted Rock:

	```Lua
	Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.POST_GRID_DESTROY, function(_, gridEnt)

	end, GridEntityType.GRID_ROCK, Epiphany.Grid.TWISTED_ROCK.ID)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_GRID_DESTROY {: .copyable } | ([GridEntity](https://repentogon.com/GridEntity.html)) | [GridEntityType](https://wofsauge.github.io/IsaacDocs/rep/enums/GridEntityType.html), integer GridVariant | void |

### ROCK_SPAWN_DROPS {: .copyable }

Called once when a rock type should spawn its drops. This accounts for destroying grid entities normally, a player throwing it as a tear via [Mom's Bracelet](https://bindingofisaacrebirth.wiki.gg/wiki/Mom%27s_Bracelet), or a [Polty](https://bindingofisaacrebirth.wiki.gg/wiki/Polty) throwing it as a projectile.

Can pass up to two optional arguments inside the AddExtraCallback function.

???- example "Example Code"
	Runs a ROCK_SPAWN_DROPS callback only for Twisted Rock:

	```Lua
	Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.ROCK_SPAWN_DROPS, function(_, pos, gridType, var, seed, ent)

	end, GridEntityType.GRID_ROCK, Epiphany.Grid.TWISTED_ROCK.ID)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|ROCK_SPAWN_DROPS {: .copyable } | ([Vector](https://repentogon.com/Vector.html) Position, [GridEntityType](https://wofsauge.github.io/IsaacDocs/rep/enums/GridEntityType.html), integer GridVariant, integer Seed, [GridEntity](https://repentogon.com/GridEntity.html) Source or [EntityTear](https://repentogon.com/EntityTear.html) or [EntityProjectile](https://repentogon.com/EntityProjectile.html)) | [GridEntityType](https://wofsauge.github.io/IsaacDocs/rep/enums/GridEntityType.html), integer GridVariant | void |

### TARNISHED_PLAYER_INIT {: .copyable }

Called on [MC_PLAYER_INIT_POST_LEVEL_INIT_STATS](https://repentogon.com/enums/ModCallbacks.html#mc_player_init_post_level_init_stats) for any tarnished character. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|TARNISHED_PLAYER_INIT {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html)) | [PlayerType](https://wofsauge.github.io/IsaacDocs/rep/enums/PlayerType.html) | void |

### ESAU_JR_INIT {: .copyable }

Called when a player uses [Esau Jr.](https://bindingofisaacrebirth.wiki.gg/wiki/Esau_Jr.) and the its state is initialized for the first time. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|ESAU_JR_INIT {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html)) | [PlayerType](https://wofsauge.github.io/IsaacDocs/rep/enums/PlayerType.html) | void |

### POST_ROOM_CLEAR {: .copyable }

Called for every room clear-like event: room clear, challenge room wave, boss rush wave, greed mode wave. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_ROOM_CLEAR {: .copyable } | ([RoomClearType](Epiphany.RoomClearType.md)) | [RoomClearType](Epiphany.RoomClearType.md) | void |

### PRE_MULTITOOL_OPEN_CHEST {: .copyable }

Called when a player touches a chest while holding the DROP key with at least 1 [multitool](https://tboiepiphany.wiki.gg/wiki/Keys#Multitool).

Returning `true` will consume the multitool and open the chest.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PRE_MULTITOOL_OPEN_CHEST {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [EntityPickup](https://repentogon.com/EntityPickup.html) Chest) | [PickupVariant](https://wofsauge.github.io/IsaacDocs/rep/enums/PickupVariant.html) | boolean |

### PRE_GAME_STARTED {: .copyable }

Called on [MC_POST_PLAYER_INIT](https://wofsauge.github.io/IsaacDocs/rep/enums/ModCallbacks.html#mc_post_player_init) - the first callback to be ran - when a run is started or continued. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PRE_GAME_STARTED {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), boolean IsContinued) | - | void |

### POST_FIRST_PAGE_CLONE {: .copyable }

Called when a player uses [First Page](https://tboiepiphany.wiki.gg/wiki/First_Page) and makes a duplicate of themselves as a Strawman-like player. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_FIRST_PAGE_CLONE {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html) StrawmanPlayer, [EntityPlayer](https://repentogon.com/EntityPlayer.html) ParentPlayer) | [PlayerType](https://wofsauge.github.io/IsaacDocs/rep/enums/PlayerType.html) | void |

### PRE_PLAYER_GRID_COLLISION {: .copyable }

Called on REPENTOGON's [MC_PRE_PLAYER_GRID_COLLISION](https://repentogon.com/enums/ModCallbacks.html), before a player collides with a grid, but only when [GridEntity](https://repentogon.com/GridEntity.html) doesn't return `nil`. Accepts no return arguments.

Can pass up to two optional arguments inside the AddExtraCallback function.

???- example "Example Code"
	Runs a PRE_PLAYER_GRID_COLLISION callback only for Twisted Rock:

	```Lua
	Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.PRE_PLAYER_GRID_COLLISION, function(_, player, gridEnt)

	end, GridEntityType.GRID_ROCK, Epiphany.Grid.TWISTED_ROCK.ID)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PRE_PLAYER_GRID_COLLISION {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [GridEntity](https://repentogon.com/GridEntity.html)) | [GridEntityType](https://wofsauge.github.io/IsaacDocs/rep/enums/GridEntityType.html), integer GridVariant | void |

### PRE_GOLDEN_ACTIVE_RENDER {: .copyable }

Called before a [golden active item](https://tboiepiphany.wiki.gg/wiki/Golden_Item) is rendered on the HUD, which is rendered directly on top of the original active item.

???+ info "HudHelper"
	This callback gets all of its parameters from the [HudHelper library](https://github.com/BenevolusGoat/hud-helper). More information on how its parameters function can be found [here](https://github.com/BenevolusGoat/hud-helper/wiki/Registering-HUD-Elements) on the repository's wiki.

Return `true` to stop the default render. Ideal for customizing the golden active render yourself.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PRE_GOLDEN_ACTIVE_RENDER {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), integer PlayerHUDIndex, HUDLayout, [Vector](https://repentogon.com/Vector.html) RenderPosition, float Alpha, float Scale, ActiveSlot) | [CollectibleType](https://wofsauge.github.io/IsaacDocs/rep/enums/CollectibleType.html) | boolean |

### BETHANY_PRE_CONSUME_HEART {: .copyable }

Called before Tarnished Bethany consumes a heart pickup for bonus charges.

Accepts returning an integer to change the number of charges gained from the heart, or `false` to prevent consuming the heart.

Can pass up to two optional arguments inside the AddExtraCallback function.

???- example "Example Code"
	Runs a BETHANY_PRE_CONSUME_HEART callback only for Soul Hearts:

	```Lua
	Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.BETHANY_PRE_CONSUME_HEART, function(_, player, heart, charges)

	end, PickupVariant.PICKUP_HEART, HeartSubType.HEART_SOUL)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|BETHANY_PRE_CONSUME_HEART {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [EntityPickup](https://repentogon.com/EntityPickup.html) Heart, integer ChargesGained) | [PickupVariant](https://wofsauge.github.io/IsaacDocs/rep/enums/PickupVariant.html), [HeartSubType](https://wofsauge.github.io/IsaacDocs/rep/enums/HeartSubType.html) | integer or boolean |

### POST_PLAYERTYPE_CHANGE {: .copyable }

Called after the current character's [PlayerType](https://wofsauge.github.io/IsaacDocs/rep/enums/PlayerType.html) changes. Accepts no return arguments.

The optional argument is checked against the player's previous [PlayerType](https://wofsauge.github.io/IsaacDocs/rep/enums/PlayerType.html).

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_PLAYERTYPE_CHANGE {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [PlayerType](https://wofsauge.github.io/IsaacDocs/rep/enums/PlayerType.html) OldPlayerType) | [PlayerType](https://wofsauge.github.io/IsaacDocs/rep/enums/PlayerType.html) | void |

### ESSENCE_OF_CAIN_REVERT_PEDESTAL {: .copyable }

Called when Essence of Cain searches for pedestals to revert to chests or slot machines.

Accepts returning a table of `{EntityType, Variant}` to successfully convert the pedestal, or `false` to prevent it from being converted.

???+ note "EntityType"
	The table return only accepts `EntityType.ENTITY_PICKUP` (intended for chests) and `EntityType.ENTITY_SLOT`. Otherwise, the return value will be ignored.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|ESSENCE_OF_CAIN_REVERT_PEDESTAL {: .copyable } | ([EntityPickup](https://repentogon.com/EntityPickup.html)) | - | table or boolean |

### CRIMSON_WISP_GROWTH {: .copyable }

Called after a Crimson Wisp matures by one stage. Accepts no return arguments.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CRIMSON_WISP_GROWTH {: .copyable } | ([EntityFamiliar](https://repentogon.com/EntityFamiliar.html) Wisp) | [CrimsonWispType](Epiphany.CrimsonWispType.md) | void |

### RULES_CARD_GET_ROOM_TYPE {: .copyable }

Called when House Rules Card attempts to get the [RoomType](https://wofsauge.github.io/IsaacDocs/rep/enums/RoomType.html) of the current room.

Accepts returning a [RoomType](https://wofsauge.github.io/IsaacDocs/rep/enums/RoomType.html) to override it, or a string to define as custom room name.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|RULES_CARD_GET_ROOM_TYPE {: .copyable } | ([RoomType](https://wofsauge.github.io/IsaacDocs/rep/enums/RoomType.html)) | [RoomType](https://wofsauge.github.io/IsaacDocs/rep/enums/RoomType.html) | [RoomType](https://wofsauge.github.io/IsaacDocs/rep/enums/RoomType.html) or string |

### RULES_CARD_SPAWN_ROOM_TYPE {: .copyable }

Called when House Rules Card spawns a room on a new floor.

Accepts returning a boolean to prevent generating a room, a [RoomType](https://wofsauge.github.io/IsaacDocs/rep/enums/RoomType.html) to change what type of room is generated, or a RoomConfigRoom object to place it on the floor.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|RULES_CARD_SPAWN_ROOM_TYPE {: .copyable } | ([RoomType](https://wofsauge.github.io/IsaacDocs/rep/enums/RoomType.html) OR string RoomName) | [RoomType](https://wofsauge.github.io/IsaacDocs/rep/enums/RoomType.html) | boolean or [RoomType](https://wofsauge.github.io/IsaacDocs/rep/enums/RoomType.html) or RoomConfigRoom |

### BETHANY_UPDATE_SKIN_COLOR {: .copyable }

Called whenever Bethany's skin color on her head or body changes. Used for updating her costumes to a unique skin color that otherwise don't have one. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|BETHANY_UPDATE_SKIN_COLOR {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [SkinColor](https://wofsauge.github.io/IsaacDocs/rep/enums/SkinColor.html) HeadColor, [SkinColor](https://wofsauge.github.io/IsaacDocs/rep/enums/SkinColor.html) BodyColor) | - | void |

### POST_LOCUST_DAMAGE {: .copyable }

Called whenever an Abyss locust damages an entity. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_LOCUST_DAMAGE {: .copyable } | ([EntityFamiliar](https://repentogon.com/EntityFamiliar.html) Locust, [Entity](https://repentogon.com/Entity.html) Hitter, [EntityPlayer](https://repentogon.com/EntityPlayer.html) Owner, number DamageAmount, [DamageFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/DamageFlag.html)) | - | void |

### POST_AQUARIOUS_DAMAGE {: .copyable }

Called whenever aquarious creep damages an entity. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_AQUARIOUS_DAMAGE {: .copyable } | ([EntityEffect](https://repentogon.com/EntityEffect.html) Creep, [Entity](https://repentogon.com/Entity.html) Hitter, [EntityPlayer](https://repentogon.com/EntityPlayer.html) Owner, number DamageAmount, [DamageFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/DamageFlag.html)) | - | void |

### REMEMBRANCE_WISP_POST_SPAWN_FAMLIIAR {: .copyable }

Called after a Remembrance wisp spawns a familiar it contained. Runs twice as it always spawns two of the familiar. Accepts no return parameters.

`CustomName` will pass a string identifier for a mod's familiar if spawned. Otherwise, returns `nil`.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|REMEMBRANCE_WISP_POST_SPAWN_FAMLIIAR {: .copyable } | ([EntityFamiliar](https://repentogon.com/EntityFamiliar.html) SpawnedFamiliar, string CustomName) | [FamiliarVariant](https://wofsauge.github.io/IsaacDocs/rep/enums/FamiliarVariant.html) | void |

### POST_GRAFT_COLLECTIBLE {: .copyable }

Called when a player has a collectible grafted unto them through the Grafting Altar. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_GRAFT_COLLECTIBLE {: .copyable } | ([EntityPlayer](https://repentogon.com/EntityPlayer.html), [ItemConfigItem](https://repentogon.com/ItemConfig_Item.html) GraftedItem) | [CollectibleType](https://wofsauge.github.io/IsaacDocs/rep/enums/CollectibleType.html) | void |

### PRE_OPEN_DUSTY_CHEST {: .copyable }

Called when a player attempts to open a Dusty Chest. This runs after checking for Pay to Play and if it's a unique Keeper Dusty Chest to open with money, but before player-specific checks.

Accepts returning a boolean to control chest behaviour. Return `false` to prevent opening the chest, or `true` to allow opening the chest. Returning nothing or `nil` will move forward with normal behaviour.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PRE_OPEN_DUSTY_CHEST {: .copyable } | ([EntityPickup](https://repentogon.com/EntityPickup.html) Chest, [EntityPlayer](https://repentogon.com/EntityPlayer.html)) | [PlayerType](https://wofsauge.github.io/IsaacDocs/rep/enums/PlayerType.html) | boolean |
