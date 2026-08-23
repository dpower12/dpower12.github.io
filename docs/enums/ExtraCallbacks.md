# Enum "Epiphany.ExtraCallbacks"

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

Called when getting the layout of a Turnover shop, which defines positions of shop items and what kind of item will be sold.

Accepts returning a new layout to override the current one

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|TURNOVER_GET_LAYOUT_INFO {: .copyable } | (ShopLayout layout, integer CurrentTier, RoomType) | - | ShopLayout |

### SAMSON_PUNCH_ENTITY {: .copyable }

Called after Tarnished Samson hits an enemy with a punch or a slam. Accepts no return parameters.

- `EntityFamiliar` is passed if the attacker is a weapon-copying familiar like Incubus. Otherwise, passes `nil`
- `Hand`: Returns `"Left"` or `"Right"` depending on the hand hit the enemy. If `isSlam` is `true`, passes `nil` instead.
- `Point1`: The first point of collision between the enemy and player attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PUNCH_ENTITY {: .copyable } | (EntityPlayer, EntityNPC Enemy, boolean isSlam, Vector point1, EntityFamiliar?, string? Hand, number DmgDealt) | - | void |

### SAMSON_PUNCH_GRID {: .copyable }

Called after Tarnished Samson hits an enemy with a punch or a slam. Only gets called for doors, poop, rocks and moveable TNT. Accepts no return parameters.

- `EntityFamiliar` is passed if the attacker is a weapon-copying familiar like Incubus. Otherwise, passes `nil`
- `Hand`: Returns `"Left"` or `"Right"` depending on the hand hit the enemy. If `isSlam` is `true`, passes `nil` instead.
- `Point1`: The first point of collision between the grid and player attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PUNCH_GRID {: .copyable } | (EntityPlayer, GridEntity, boolean isSlam, Vector point1, EntityFamiliar?, string? Hand) | - | void |

### SAMSON_SLAM {: .copyable }

Called after Tarnished Samson performs a slam attack and after boulders are dropped, but before grids and entities are damaged. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_SLAM {: .copyable } | (EntityPlayer, number SlamRange) | - | void |

### SAMSON_DASH_HIT_ENEMY {: .copyable }

Called after a player performs a dash with Killer Instinct and hits an enemy. `Point1` is the first point of collision between grid and player attack. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_DASH_HIT_ENEMY {: .copyable } | (EntityPlayer, EntityNPC Enemy, Vector Point1) | - | void |

### SAMSON_DASH_HIT_WALL {: .copyable }

Called after a player performs a dash with Killer Instinct and hits a grid or wall. `Point1` is the first point of collision between grid and player attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_DASH_HIT_WALL {: .copyable } | (EntityPlayer, Vector Point1) | - | void |

### SAMSON_PRE_PUNCH {: .copyable }

Called after Tarnished Samson picks out entities in his hitbox range, but before they get damaged.

Accepts returning an array of entities to override what entities are hit by the attack.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_PUNCH {: .copyable } | (EntityPlayer, Vector AttackDirection, Entity[] EntityList, EntityFamiliar?) | - | Entity[] |

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

Hitboxes can be made with `Epiphany.Character.SAMSON:MakePunchHitbox(EntityPlayer, number PunchLength, number PunchWidth, Vector AimDirection, EntityFamiliar?, Vector? Forced Position): SamsonHitboxData`.

???+ example "Example Code"
	```Lua
	Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.SAMSON_PRE_HITBOX_GENERATE, function(_, player, hitbox, familiar)
		--TODO: Actually write something here
	end)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_HITBOX_GENERATE {: .copyable } | (EntityPlayer, SamsonHitboxData Hitbox, EntityFamiliar?) | - | table or boolean |

### SAMSON_POST_THROW_BOULDER {: .copyable }

Called after a player through a boulder.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_POST_THROW_BOULDER {: .copyable } | (EntityPlayer, EntityEffect Boulder) | - | void |

### SAMSON_PRE_DAMAGE_ENTITY {: .copyable }

Called between [SAMSON_PRE_PUNCH](#samson_pre_punch) and [SAMSON_PUNCH_ENTITY](#samson_punch_entity), right before damage is dealt to an entity.

- `EntityFamiliar` is passed if the attacker is a weapon-copying familiar like Incubus. Otherwise, passes `nil`
- `Hand`: Returns `"Left"` or `"Right"` depending on the hand hit the enemy. If `isSlam` is `true`, passes `nil` instead.
- `Point1`: The first point of collision between the enemy and player attack.
- `PunchOrigin`: Origin of the punch hitbox. Not the center of the position, but where it originates from.  If `isSlam` is `true`, passes `nil` instead.

Accepts returning `-1` to prevent damage or a table of any combination of the following variables to modify the damage:

- Multiply: Stacks additively with other `Multiply` modifiers,
- MultiplyExponential: Stacks exponentially with other `MultiplyExponential` modifiers,
- AddFlat: Applied after multipliers,
- DamageFlags: Adds [TearFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/TearFlags.html) to the damage.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_DAMAGE_ENTITY {: .copyable } | (EntityPlayer, EntityNPC Enemy, boolean IsSlam, Vector Point1, EntityFamiliar?, string Hand, Vector PunchOrigin) | - | `-1` or table |

### SAMSON_PRE_DAMAGE_ENTITY_DASH {: .copyable }

Called before [SAMSON_DASH_HIT_ENEMY](#samson_dash_hit_enemy) and before damage is dealt. `Point1` is the first point of collision between the enemy and player attack.

Accepts returning `-1` to prevent damage or a table of any combination of the following variables to modify the damage:

- Multiply: Stacks additively with other `Multiply` modifiers,
- MultiplyExponential: Stacks exponentially with other `MultiplyExponential` modifiers,
- AddFlat: Applied after multipliers,
- DamageFlags: Adds [TearFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/TearFlags.html) to the damage.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_DAMAGE_ENTITY_DASH {: .copyable } | (EntityPlayer, EntityNPC Enemy, Point1) | - | `-1`  or table|

### SAMSON_POST_HITBOX_GENERATE {: .copyable }

Called after [SAMSON_PRE_HITBOX_GENERATE](#samson_pre_hitbox_generate) for each hitbox. Accepts returning a new `SamsonHitboxData` object to override the generated hitbox.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_POST_HITBOX_GENERATE {: .copyable } | (EntityPlayer, EntityFamiliar?, SamsonHitboxData Hitbox) | - | SamsonHitboxData |

### CAIN_POST_BAG_ITEM {: .copyable }

Called after an item is bagged with Throwing Bag. Accepts no return parameters.

???+ warning "Save data"
	The passed [BagsInfo](../classes/BagsInfo.md) object is mutable and will reflect inside Epiphany's save data.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CAIN_POST_BAG_ITEM {: .copyable } | (EntityPlayer, BagsInfo, CollectibleType ID, boolean IsGolden) | CollectibleType | void |

### CAIN_POST_CREATE_BAG {: .copyable }

Called after a new Throwing Bag is created. Accepts no return parameters.

???+ warning "Save data"
	The passed [BagsInfo](../classes/BagsInfo.md) object is mutable and will reflect inside Epiphany's save data.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CAIN_POST_CREATE_BAG {: .copyable } | (EntityPlayer, BagsInfo, BagData, string NewBagID) | - | void |

### SAMSON_PRE_BOULDER_SPRITE_INIT {: .copyable }

Called before a stationary or thrown boulder has its sprite initialized.

- `Tag`: A table that can hold arbitrary data for the purposes of synergies.
- `Variant`: Can either be `1`, `2`, or `3`, corresponding to which spritesheet it should utilize for the boulder.

Accepts returning a table of any combination of the following variables to modify the boulder sprite:

- `Anm2`: String path to the anm2 to use for the boulder, or nil to use the default
- `ThrownAnm2`: String path to the anm2 to use for the boulder when it's thrown, or `nil` to use the default.
- `Spritesheet`: String path to the spritesheet to use for the boulder.
- `Priority`: Number denoting how important this sprite is. Higher numbers override lower numbers.
- `DontUpdate`: Set to `true` to not update the spritesheet animations at all.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|SAMSON_PRE_BOULDER_SPRITE_INIT {: .copyable } | (EntityPlayer, integer Variant, table Tag, EntityEffect or EntityTear Boulder) | - | table |

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
|POST_GRID_UPDATE {: .copyable } | (GridEntity) | GridEntityType, integer GridVariant | void |

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
|POST_GRID_DESTROY {: .copyable } | (GridEntity) | GridEntityType, integer GridVariant | void |

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
|ROCK_SPAWN_DROPS {: .copyable } | (Vector Position, GridEntityType, integer GridVariant, integer Seed, GridEntity or EntityTear or EntityProjectile) | GridEntityType, integer GridVariant | void |

### TARNISHED_PLAYER_INIT {: .copyable }

Called on [MC_PLAYER_INIT_POST_LEVEL_INIT_STATS](https://repentogon.com/enums/ModCallbacks.html#mc_player_init_post_level_init_stats) for any tarnished character. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|TARNISHED_PLAYER_INIT {: .copyable } | (EntityPlayer) | PlayerType | void |

### ESAU_JR_INIT {: .copyable }

Called when a player uses [Esau Jr.](https://bindingofisaacrebirth.wiki.gg/wiki/Esau_Jr.) and the its state is initialized for the first time.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|ESAU_JR_INIT {: .copyable } | (EntityPlayer) | PlayerType | Return |

### TURNOVER_GET_PRICE {: .copyable }

Called for each player with Turnover every game update to refresh the price of setting up a shop.

Accepts returning a new price, or `false` to indicate that a shop cannot be set up and prevent later callback functions from running.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|TURNOVER_GET_PRICE {: .copyable } | (EntityPlayer, integer ShopPrice, integer OriginalPrice) | OptionalArg | integer or boolean |

### TURNOVER_POST_CREATE_SHOP {: .copyable }

Called after all the shop items are spawned for a Turnover shop. `SpawnedItems` passes an array of all the entities spawned by Turnover, including slot machines and the shopkeeper. Pickups and Collectibles are already cast to EntityPickup.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|TURNOVER_POST_CREATE_SHOP {: .copyable } | (EntityPlayer, Entity[] SpawnedItems) | OptionalArg | Return |

### POST_ROOM_CLEAR {: .copyable }

Called for every room clear-like event: room clear, challenge room wave, boss rush wave, greed mode wave. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_ROOM_CLEAR {: .copyable } | (RoomClearType) | [RoomClearType] | void |

### PRE_MULTITOOL_OPEN_CHEST {: .copyable }

Called when a player touches a chest while holding the DROP key with at least 1 [multitool](https://tboiepiphany.wiki.gg/wiki/Keys#Multitool).

Returning `true` will consume the multitool and open the chest.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PRE_MULTITOOL_OPEN_CHEST {: .copyable } | (EntityPlayer, EntityPickup Chest) | PickupVariant | boolean |

### PRE_GAME_STARTED {: .copyable }

Called on MC_POST_PLAYER_INIT - the first callback to be ran - when a run is started or continued. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PRE_GAME_STARTED {: .copyable } | (EntityPlayer, boolean IsContinued) | - | void |

### POST_FIRST_PAGE_CLONE {: .copyable }

Called when a player uses [First Page](https://tboiepiphany.wiki.gg/wiki/First_Page) and makes a duplicate of themselves as a Strawman-like player. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_FIRST_PAGE_CLONE {: .copyable } | (EntityPlayer StrawmanPlayer, EntityPlayer ParentPlayer) | PlayerType | void |

### CAIN_POST_SWING_HIT {: .copyable }

Called after a throwing bag currently being swung hits an enemy. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CAIN_POST_SWING_HIT {: .copyable } | (EntityEffect SwingBag, Entity Enemy, EntityPlayer, SwingingBagData, float FinalDamage) | - | void |

### CAIN_POST_BAG_THROW {: .copyable }

Called after a throwing bag is thrown. Accepts no return parameters.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CAIN_POST_BAG_THROW {: .copyable } | (EntityEffect ThrownBag, BagData) | - | void |

### CAIN_POST_BAG_HIT {: .copyable }

Called after a throwing bag that was thrown hits an enemy.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CAIN_POST_BAG_HIT {: .copyable } | (EntityEffect ThrownBag, Entity Enemy, ThrownBagData, float FinalDamage) | - | void |

### PRE_PLAYER_GRID_COLLISION {: .copyable }

Called on REPENTOGON's [MC_PRE_PLAYER_GRID_COLLISION](https://repentogon.com/enums/ModCallbacks.html), before a player collides with a grid, but only when GridEntity doesn't return `nil`. Accepts no return arguments.

Can pass up to two optional arguments inside the AddExtraCallback function.

???- example "Example Code"
	Runs a PRE_PLAYER_GRID_COLLISION callback only for Twisted Rock:

	```Lua
	Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.PRE_PLAYER_GRID_COLLISION, function(_, player, gridEnt)

	end, GridEntityType.GRID_ROCK, Epiphany.Grid.TWISTED_ROCK.ID)
	```

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PRE_PLAYER_GRID_COLLISION {: .copyable } | (EntityPlayer, GridEntity) | GridEntityType, integer GridVariant | void |

### PRE_GOLDEN_ACTIVE_RENDER {: .copyable }

Called before a [golden active item](https://tboiepiphany.wiki.gg/wiki/Golden_Item) is rendered on the HUD, which is rendered directly on top of the original active item.

???+ info "HudHelper"
	This callback gets all of its parameters from the [HudHelper library](https://github.com/BenevolusGoat/hud-helper). More information on how its parameters function can be found [here](https://github.com/BenevolusGoat/hud-helper/wiki/Registering-HUD-Elements) on the repository's wiki.

Return `true` to stop the default render. Ideal for customizing the golden active render yourself.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PRE_GOLDEN_ACTIVE_RENDER {: .copyable } | (EntityPlayer, integer PlayerHUDIndex, HUDLayout, Vector RenderPosition, float Alpha, float Scale, ActiveSlot) | CollectibleType | boolean |

### POST_ACHIEVEMENT_GET {: .copyable }

Called after an achievement from Epiphany is obtained. `Value` returns `1` for Normal Mode, `2` for Hard Mode. Does not run for achievements obtained through cheats. Accepts no return parameters.


|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_ACHIEVEMENT_GET {: .copyable } | (string AchievementName, integer Value) | string AchievementName | void |

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
|BETHANY_PRE_CONSUME_HEART {: .copyable } | (EntityPlayer, EntityPickup Heart, integer ChargesGained) | PickupVariant, HeartSubType | Return |

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
|SAMSON_PRE_CONSUME_HEART {: .copyable } | (EntityPlayer, EntityPickup Heart, integer TokensGained) | PickupVariant, HeartSubType | Return |

### POST_PLAYERTYPE_CHANGE {: .copyable }

Called after the current character's PlayerType changes. Accepts no return arguments.

The optional argument is checked against the player's previous PlayerType.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_PLAYERTYPE_CHANGE {: .copyable } | (EntityPlayer, PlayerType OldPlayerType) | PlayerType | void |

### CRIMSON_WISP_GROWTH {: .copyable }

Called after a Crimson Wisp matures by one stage. Accepts no return arguments.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|CRIMSON_WISP_GROWTH {: .copyable } | (EntityFamiliar Wisp) | CrimsonWispType | void |

### RULES_CARD_GET_ROOM_TYPE {: .copyable }

Called when House Rules Card attempts to get the RoomType of the current room.

Accepts returning a RoomType to override it, or a string to define as custom room name.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|RULES_CARD_GET_ROOM_TYPE {: .copyable } | (RoomType) | RoomType | RoomType or string |

### RULES_CARD_SPAWN_ROOM_TYPE {: .copyable }

Called when House Rules Card spawns a room on a new floor.

Accepts returning a boolean to prevent generating a room, a RoomType to change what type of room is generated, or a RoomConfigRoom object to place it on the floor.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|RULES_CARD_SPAWN_ROOM_TYPE {: .copyable } | (RoomType OR string RoomName) | OptionalArg | Return |

### BETHANY_UPDATE_SKIN_COLOR {: .copyable }

Called whenever Bethany's skin color on her head or body changes. Used for updating her costumes to a unique skin color that otherwise don't have one.

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|BETHANY_UPDATE_SKIN_COLOR {: .copyable } | (Arg Name) | OptionalArg | Return |

### POST_LOCUST_DAMAGE {: .copyable }

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_LOCUST_DAMAGE {: .copyable } | (Arg Name) | OptionalArg | Return |

### POST_AQUARIOUS_DAMAGE {: .copyable }

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_AQUARIOUS_DAMAGE {: .copyable } | (Arg Name) | OptionalArg | Return |

### REMEMBRANCE_WISP_POST_SPAWN_FAMLIIAR {: .copyable }

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|REMEMBRANCE_WISP_POST_SPAWN_FAMLIIAR {: .copyable } | (Arg Name) | OptionalArg | Return |

### POST_GRAFT_COLLECTIBLE {: .copyable }

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|POST_GRAFT_COLLECTIBLE {: .copyable } | (Arg Name) | OptionalArg | Return |

### PRE_OPEN_DUSTY_CHEST {: .copyable }

|Name|Function Args|Optional Args|Return Type|
|:--|:--|:--|:--|
|PRE_OPEN_DUSTY_CHEST {: .copyable } | (Arg Name) | OptionalArg | Return |
