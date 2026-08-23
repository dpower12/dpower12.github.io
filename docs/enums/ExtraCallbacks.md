# Enum "ExtraCallbacks"

This is a list of all new callbacks added by Epiphany. Instead of using your mod reference, call the function `Epiphany:AddExtraCallback` or `Epiphany:AddExtraPriorityCallback` with the same parameters. The enums themselves are accessible through `Epiphany.ExtraCallbacks`.

## Callbacks

{% include-markdown "hidden/unfinished_notice.md" start="<!-- start -->" end="<!-- end -->" %}

### TURNOVER_GET_PICKUP_POOL {: .copyable }

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|TURNOVER_GET_PICKUP_POOL {: .copyable } | (table Pool, integer currentTier, RoomType) | - | PickupPool |

### PRE_UNLOCK_CACHE {: .copyable }

Called on MC_POST_GAME_STARTED on LATE priority. Passes a table with `Items` and `Cards` as variables. Used to define if modded items/cards are defined or not.

???- note "REPENTOGON achievements"
	If your mod uses REPENTOGON achievements, this callback isn't necessary, as it will properly lock the item inside the vanilla game. If not cache'd already and there is an achievement ID tied to it, Epiphany will check `ItemConfig[Item/Card]:IsAvailable()`, then cache the result.

The table can be modified to have your CollectibleType or Card ID as the key, and a boolean for setting if they're unlocked or not.

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

### TURNOVER_GET_LAYOUT_INFO {: .copyable }

Called when getting the layout of a shop, which defines positions of shop items and what kind of item will be sold.

Accepts returning a new layout to override the current one

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|TURNOVER_GET_LAYOUT_INFO {: .copyable } | (ShopLayout layout, integer CurrentTier, RoomType) | - | ShopLayout |

### SAMSON_PUNCH_ENTITY {: .copyable }

Called after Tarnished Samson hits an enemy with a punch or a slam

- `Familiar` will pass `EntityFamiliar` if the attacker is a weapon-copying familiar like Incubus. Otherwise, passes `nil`
- `Hand` will pass `"Left"` or `"Right"` depending on the hand hit the enemy. If `isSlam` is `true`, passes `nil` instead.
- `Point1` is the first point of collision between the enemy and player attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PUNCH_ENTITY {: .copyable } | (EntityPlayer, EntityNPC, boolean isSlam, Vector point1, EntityFamiliar? Familiar, string? Hand, number DmgDealt) | - | void |

### SAMSON_PUNCH_GRID {: .copyable }

Called after Tarnished Samson hits an enemy with a punch or a slam. Only gets called for doors, poop, rocks and moveable TNT.

- `Familiar` will pass `EntityFamiliar` if the attacker is a weapon-copying familiar like Incubus. Otherwise, passes `nil`
- `Hand` will pass `"Left"` or `"Right"` depending on the hand hit the enemy. If `isSlam` is `true`, passes `nil` instead.
- `Point1` is the first point of collision between the grid and player attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PUNCH_ENTITY {: .copyable } | (EntityPlayer, GridEntity, boolean isSlam, Vector point1, EntityFamiliar? Familiar, string? Hand) | - | void |

### SAMSON_SLAM {: .copyable }

Called after Tarnished Samson performs a slam attack and after boulders are dropped, but before grids and entities are damaged.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_SLAM {: .copyable } | (EntityPlayer, number SlamRange) | - | void |

### SAMSON_DASH_HIT_ENEMY {: .copyable }

Called after a player performs a dash with Killer Instinct and hits an enemy.

`Point1` is the first point of collision between grid and player attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_DASH_HIT_ENEMY {: .copyable } | (EntityPlayer, EntityNPC, Vector Point1) | - | void |

### SAMSON_DASH_HIT_WALL {: .copyable }

Called after a player performs a dash with Killer Instinct and hits a grid or wall.

`Point1` is the first point of collision between grid and player attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_DASH_HIT_WALL {: .copyable } | (EntityPlayer, Vector Point1) | - | void |

### SAMSON_PRE_PUNCH {: .copyable }

Called after Tarnished Samson picks out entities in his hitbox range, but before they get damaged.

Accepts returning an array of entities to override what entities are hit by the attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_PUNCH {: .copyable } | (EntityPlayer, Vector AttackDirection, Entity[] EntityList, EntityFamiliar? Familiar) | - | Entity[] |

### SAMSON_BOULDER_DEAD {: .copyable }

Called after a boulder dies.

Collider passes `Entity` if detected that the boulder hit an entity before dying. Otherwise, passes `nil`.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_BOULDER_DEAD {: .copyable } | (EntityPlayer, Entity? Collider, number DamageAmount, TearFlags) | - | void |

### PLAYER_DAMAGED_ENTITY {: .copyable }

Called when the player has inflicted damage onto an entity.

Triggers on `MC_ENTITY_TAKE_DMG` if:
- The `source` is the player, or was a source spawned by the player
- The hit entity is an EntityNPC
- EntityNPC:IsVulnerableEnemy() returns `true`

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PLAYER_DAMAGED_ENTITY {: .copyable } | (EntityPlayer, Entity, number Amount, DamageFlags) | - | void |

### SLOT_ON_DEATH {: .copyable }

Called after a slot machine dies.

Triggers on `MC_POST_SLOT_UPDATE` the first time that `EntitySlot:GetState()` returns `SlotState.DESTROYED`.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SLOT_ON_DEATH {: .copyable } | (EntitySlot) | SlotVariant | void |

### SAMSON_PRE_HITBOX_GENERATE {: .copyable }

Called after a hitbox is generated for Tarnished Samson's punches, but before it is used to detect what collides with.

Accepts returning an array of tables containing `Hitbox` and `Direction` to append additional hitboxes with the original one. Optionally, a boolean can be a second return value to prevent the original hitbox from being used.

Hitboxes can be made with `Epiphany.Character.SAMSON:MakePunchHitbox(EntityPlayer, number PunchLength, number PunchWidth, Vector AimDirection, EntityFamiliar? Familiar, Vector? Forced Position): SamsonHitboxData`.

???+ example "Example Code"
	```Lua
	Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.SAMSON_PRE_HITBOX_GENERATE, function(_, player, hitbox, familiar)
		a
	end)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_HITBOX_GENERATE {: .copyable } | (EntityPlayer, SamsonHitboxData Hitbox, EntityFamiliar? Familiar) | - | table or boolean |

### CALLBACK_NAME {: .copyable }

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CALLBACK_NAME {: .copyable } | (Arg Name, Arg Name) | OptionalArg | Return |

### CALLBACK_NAME {: .copyable }

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CALLBACK_NAME {: .copyable } | (Arg Name, Arg Name) | OptionalArg | Return |

### CALLBACK_NAME {: .copyable }

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CALLBACK_NAME {: .copyable } | (Arg Name, Arg Name) | OptionalArg | Return |

	Callbacks = {

		SAMSON_PUNCH_ENTITY = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "EntityNPC", Name = "npc"}, {Type = "boolean", Name = "isSlam"}, {Type = "Vector", Name = "point1"}, {Type = "EntityFamiliar?", Name = "incubus"}, {Type = "string?", Name = "hand"}, {Type = "number", Name = "dmgDealt"}},
		},
		SAMSON_PUNCH_GRID = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "GridEntity", Name = "grid"}, {Type = "boolean", Name = "isSlam"}, {Type = "Vector", Name = "point1"}, {Type = "EntityFamiliar?", Name = "incubus"}}
		},
		SAMSON_SLAM = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "number", Name = "slamRange"}},
		},
		SAMSON_DASH_HIT_ENEMY = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "EntityNPC", Name = "npc"}, {Type = "Vector", Name = "point1"}},
		},
		SAMSON_DASH_HIT_WALL = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "Vector", Name = "point1"}},
		},
		SAMSON_PRE_PUNCH = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "Vector", Name = "direction"}, {Type = "Entity[]", Name = "entities"}, {Type = "EntityFamiliar?", Name = "incubus"}},
			Returns = {{Type = "Entity[]"}}
		},
		SAMSON_BOULDER_DEAD = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "Entity", Name = "entity"}, {Type = "number", Name = "amount"}, {Type = "DamageFlag", Name = "flags"}},
		},
		PLAYER_DAMAGED_ENTITY = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "Entity", Name = "entity"}, {Type = "number", Name = "amount"}, {Type = "DamageFlag", Name = "flags"}},
		},
		SLOT_ON_DEATH = {
			Args = {{Type = "EntitySlot", Name = "slot"}},
		},
		SAMSON_PRE_HITBOX_GENERATE = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "SamsonHitboxData", Name = "hitbox"}, {Type = "EntityFamiliar?", Name = "incubus"}},
			Returns = {{Type = "{Hitbox: SamsonHitboxData?, Direction: Vector?}"}}
		},
		SAMSON_POST_THROW_BOULDER = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "EntityEffect", Name = "boulder"}},
		},
		SAMSON_PRE_DAMAGE_ENTITY = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "EntityNPC", Name = "npc"}, {Type = "boolean", Name = "isSlam"}, {Type = "Vector", Name = "point1"}, {Type = "EntityFamiliar?", Name = "incubus"}, {Type = "string", Name = "incubus"}, {Type = "Vector", Name = "punchOrigin"}},
			Returns = {{Type =  "{Add: number?, Multiply: number?, MultiplyExponential: number?, AddFlat: number?, DamageFlags: TearFlags?}"}}
		},
		SAMSON_PRE_DAMAGE_ENTITY_DASH = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "EntityNPC", Name = "npc"}, {Type = "Vector", Name = "point1"}},
			Returns = {{Type = "{Add: number?, Multiply: number?, MultiplyExponential: number?, AddFlat: number?, DamageFlags: TearFlags?}"}}
		},
		SAMSON_POST_HITBOX_GENERATE = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "EntityFamiliar?", Name = "incubus"}, {Type = "SamsonHitboxData", Name = "hitbox"}},
		},
		CAIN_POST_BAG_ITEM = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "BagsInfo", Name = "bagsInfo"}, {Type = "CollectibleType", Name = "item"}, {Type = "boolean", Name = "iGolden"}},
		},
		CAIN_POST_CREATE_BAG = {
			Args = {{Type = "EntityPlpayer", Name = "player"}, {Type = "BagsInfo", Name = "bagsInfo"}, {Type = "BagData", Name = "bagData"}, {Type = "string", Name = "newBagId"}},
		},
		SAMSON_PRE_BOULDER_SPRITE_INIT = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "1|2|3", Name = "variant"}, {Type = "table", Name = "tag"}, {Type = "EntityEffect|EntityTear", Name = "entity"}},
			Returns = {{Type = "{Anm2: string?, ThrownAnm2: string?, Spritesheet: string?, Priority: number?, DontUpdate: boolean?}"}}
		},
		POST_GRID_UPDATE = {
			Args = {{Type = "GridEntity", Name = "grid"}},
		},
		POST_GRID_DESTROY = {
			Args = {{Type = "GridEntity", Name = "grid"}},
		},
		ROCK_SPAWN_DROPS = {
			Args = {{Type = "Vector", Name = "Position"}, {Type = "GridEntityType", Name = "Type"}, {Type = "integer", Name = "Variant"}, {Type = "integer", Name = "Seed"}, {Type = "GridEntity|EntityTear|EntityProjectile", Name = "Entity"}},
		},
		TARNISHED_PLAYER_INIT = {
			Args = {{Type = "EntityPlayer", Name = "player"}},
		},
		ESAU_JR_INIT = {
			Args = {{Type = "EntityPlayer", Name = "player"}},
		},
		TURNOVER_GET_PRICE = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "PickupPrice|integer", Name = "price"}, {Type = "PickupPrice|integer", Name = "originalPrice"}},
			Returns = {{Type = "PickupPrice|integer|boolean"}},
		},
		TURNOVER_POST_CREATE_SHOP = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "EntityNPC[]|EntityPickup[]|EntitySlot[]", Name = "spawnedItems"}}
		},
		POST_ROOM_CLEAR = {
			Args = {{Type = "RoomClearType", Name = "clearType"}},
		},
		PRE_MULTITOOL_OPEN_CHEST = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "EntityPickup", Name = "pickup"}},
			Returns = {{Type = "boolean"}},
		},
		PRE_GAME_STARTED = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "boolean", Name = "isContinued"}},
		},
		POST_FIRST_PAGE_CLONE = {
			Args = {{Type = "EntityPlayer", Name = "clonePlayer"}, {Type = "EntityPlayer", Name = "parentPlayer"}},
		},
		CAIN_POST_SWING_HIT = {
	-- - `finalDamage` - The final damage inflicted by the bag
			Args = {{Type = "EntityEffect", Name = "swingBag"}, {Type = "Entity", Name = "entity"}, {Type = "EntityPlayer", Name = "player"}, {Type = "SwingingBagData", Name = "swingingBagData"}, {Type = "number", Name = ""}},
		},
		CAIN_POST_BAG_THROW = {
			Args = {{Type = "EntityEffect", Name = "thrownBag"}, {Type = "BagData", Name = "BagData"}},
		},
		CAIN_POST_BAG_HIT = {
			Args = {{Type = "EntityEffect", Name = "thrownBag"}, {Type = "Entity", Name = "entity"}, {Type = "ThrownBagData", Name = "thrownBagData"}, {Type = "number", Name = "finalDamage"}},
		},
		PRE_PLAYER_GRID_COLLISION = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "GridEntity", Name = "grid"}},
		},
		PRE_GOLDEN_ACTIVE_RENDER = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "integer", Name = "playerHUDIndex"}, {Type = "integer", Name = "hudLayout"}, {Type = "Vector", Name = "position"}, {Type = "number", Name = "alpha"}, {Type = "number", Name = "scale"}, {Type = "ActiveSlot", Name = "slot"}},
			Returns = {{Type = "boolean"}},
		},
		POST_ACHIEVEMENT_GET = {
			Args = {{Type = "string", Name = "achName"}, {Type = "integer", Name = "value"}},
		},
		BETHANY_PRE_CONSUME_HEART = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "EntityPickup", Name = "heart"}, {Type = "integer", Name = "chargesGained"}},
			Returns = {{Type = "integer|boolean"}},
		},
		SAMSON_PRE_CONSUME_HEART = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "EntityPickup", Name = "heart"}, {Type = "integer", Name = "tokensGained"}},
			Returns = {{Type = "integer|boolean"}},
		},
		POST_PLAYERTYPE_CHANGE = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "PlayerType", Name = "oldPlayerType"}},
		},
		ESSENCE_OF_CAIN_REVERT_PEDESTAL = {
			Args = {{Type = "EntityPickup", Name = "pedestal"}},
			Returns = {{Type = "{integer, integer}"}}
		},
		CRIMSON_WISP_GROWTH = {
			Args = {{Type = "EntityFamiliar", Name = "wisp"}},
		},
		RULES_CARD_GET_ROOM_TYPE = {
			Args = {{Type = "RoomType", Name = "roomType"}},
			Returns = {{Type = "RoomType|string"}},
		},
		RULES_CARD_SPAWN_ROOM_TYPE = {
			Args = {{Type = "RoomType|string", Name = "roomType"}},
			Returns = {{Type = "boolean|RoomType|RoomConfigRoom"}},
		},
		BETHANY_UPDATE_SKIN_COLOR = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "SkinColor", Name = "headColor"}, {Type = "SkinColor", Name = "bodyColor"}},
		},
		POST_LOCUST_DAMAGE = {
			Args = {{Type = "EntityFamiliar", Name = "locust"}, {Type = "Entity", Name = "entity"}, {Type = "EntityPlayer", Name = "player"},{Type = "number", Name = "dmg"}, {Type = "DamageFlag", Name = "flags"}},
		},
		POST_AQUARIOUS_DAMAGE = {
			Args = {{Type = "EntityEffect", Name = "creep"}, {Type = "Entity", Name = "entity"}, {Type = "EntityPlayer", Name = "player"},{Type = "number", Name = "dmg"},{Type = "DamageFlag", Name = "flags"}},
		},
		REMEMBRANCE_WISP_FIRST_INIT = {
			Args = {{Type = "EntityFamiliar", Name = "wisp"}, {Type = "EntityFamiliar", Name = "familiar"}},
			Returns = {{Type = "table"}},
		},
		REMEMBRANCE_WISP_PRE_PLAY_ANIMATION = {
			Args = {{Type = "EntityFamiliar", Name = "wisp"}, {Type = "FamiliarVariant", Name = "variant"}, {Type = "integer", Name = "subtype"}, {Type = "table", Name = "customData"}},
			Returns = {{Type = "boolean"}},
		},
		REMEMBRANCE_WISP_POST_SPAWN_FAMLIIAR = {
			Args = {{Type = "EntityFamiliar", Name = "familiar"}, {Type = "table", Name = "customData"}},
		},
		POST_GRAFT_COLLECTIBLE = {
			Args = {{Type = "EntityPlayer", Name = "player"}, {Type = "ItemConfigItem", Name = "itemConfig"}},
		},
		PRE_OPEN_DUSTY_CHEST = {
			Args = {{Type = "EntityPickup", Name = "chest"}, {Type = "EntityPlayer", Name = "player"}},
			Returns = {{Type = "boolean"}},
		}
	}