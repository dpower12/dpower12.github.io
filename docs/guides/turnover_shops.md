{% include-markdown "hidden/unfinished_notice.md" start="<!-- start -->" end="<!-- end -->" %}

[Turnover](https://tboiepiphany.wiki.gg/wiki/Turnover) is an active item from Epiphany that creates shops unique to the type of room it was created in, containing collectibles from the room's item pool (unique item pools have been made for special rooms that don't normally have one!). The shop can have any number of different levels from repeated uses of the active in the same room, which grant more pickups and items. These unique shops and their levels are also accompanied by their own Shopkeeper sprites for each level (Usually a maximum of 3 sprites). This guide covers how to add your own Turnover Shop for your own custom room.

## Adding your custom Turnover Shop

### Variables
Turnover shops are added through the function `Epiphany.API:AddTurnoverShop`, which accepts a table, which should hold the following variables:

| Variable Name | Possible Values | Description |
|:--|:--|:--|
|Name|string|Internal identifier name for the Turnover shop|
|Checker|function|No arguments. Return `true` if this shop should spawn|
|ShopLayout|table|Table containing variables to define the location and types of entities to spawn for the shop and its level|
|PickupPool|table|Table to determine what pickups are possible to spawn inside the shop, separated by shop level|

### ShopLayout
This table is where you define the layout of your shop. What items and pickups spawn, where your shopkeeper spawns, and how much it costs to setup the shop for creating and upgrading your shop.

Below is a detailed example of a ShopLayout table:

???+ info "ShopItemType"
	This is how you determine what "type" of item spawns in your shop. Accessed through Epiphany.Item.TURNOVER.ShopItemType, it has the following variables available:

	| Variable Name | Value | Description |
	|:--|:--|:--|
	|Collectible|1|Spawns a collectible|
	|Pickup|2|Spawns a pickup|
	|Slot|3|Spawns a slot machine|
	|RestockMachine|4|Spawns a special Restock Machine. Only appears if Turnover is used by Tarnished Keeper with Birthright|
	|VanityShop|5|Exclusive to the [Revlations mod](https://tboirevelations.wiki.gg/wiki/Prank%27s_shop). It has its own "VanityType" list to determine what to spawn|

```Lua
local ShopItemType = Epiphany.Item.TURNOVER.ShopItemType

ShopLayout = {
	SetUpPrice = 15 -- number. Cost of creating and upgrading your shop. Is optional, being 10 by default.
	ShopKeeperPosition = Vector(320,180), -- Vector. Position of the Turnover Shopkeeper. `Vector(320, 200)` by default.
	[0] = { --0 is the first shop level, 1 is the second, and so on.
		SetUpPrice = 5, --number. Defines the price for setting up this shop's level, overrides the shop's normal SetUpPrice.
		--From here, an array is used to determine the shop spawns.
		--{[1] = Vector, [2] = TURNOVER.ShopItemType, [3] = function|nil, OptionsIndex = number|nil}
		--OptionsIndex will pair the shop item with other shop items with the same OptionsIndex so that collecting one of them will make the others disappear.

		--Pickups are picked from the shop's PickupPool. Index [3] serves no purpose here.
		{Vector(280, 320), ShopItemType.Pickup},

		-- Collectibles are picked from current room's item pool, unless a getter function is specified at index [3]
		{Vector(360, 320), ShopItemType.Collectible},
		{Vector(360, 360), ShopItemType.Collectible, function (itemPoolType, rng)
			local itemID = rng:RandomInt(CollectibleType.NUM_COLLECTIBLES) + 1
			return itemID
		end},

		-- Slots are picked from Slots SlotGroup, unless a getter function is specified at index [3]
		{Vector(360, 400), ShopItemType.Slot},
		{Vector(360, 440), ShopItemType.Slot, function (rng)
			local slotID = rng:RandomInt(9) + 1
			return slotID
		end},

		-- Restock machine only spawns if Turnover is used by TR Keeper with Birthright
		{Vector(360, 400), ShopItemType.RestockMachine}
	},
	[1] = { --Shop Level 2
		SetupPrice = 999,
		{Vector(360, 320), ShopItemType.Collectible, function()
			return CollectibleType.COLLECTIBLE_DEATH_CERTIFICATE
		end},
	}
}
```

### PickupPool
This table determines what pickups are chosen to appear whenever an entry with `ShopItemType.Pickup` from the ShopLayout is spawned. They are separated by level and are chosen at random, with the ability to set weights to have some appear less or more often than others.

Below is a detailed example layout of a PickupPool table:

```Lua
PickupPool = {
	--[[
	PICKUP POOL ENTRY INFO (Counts for one pickup entry):
	{
		[1] = {
			[1] = PickupVariant
			[2] = number. Pickup subtype
			minTier = number. Optional. Lowest shop tier the pickup can appear in
			maxTier = number. Optional. Highest shop tier the pickup can appear in
		},
		Weight = number. Optional, defaults to 1.0. Determines how likely the pickup is to appear. Higher is more likely, lower is less likely.
		--Whenever a pickup pool entry is chosen to spawn, its weight is multiplied by 0.25 (4x less likely to spawn again) for the current shop tier.
	}
	]]--

	{{ PickupVariant.PICKUP_BOMB, BombSubType.BOMB_NORMAL,		minTier = 0, maxTier = 3 }},
	{{ PickupVariant.PICKUP_BOMB, BombSubType.BOMB_DOUBLEPACK,	minTier = 2, maxTier = 4 }},
	{{ PickupVariant.PICKUP_BOMB, BombSubType.BOMB_GIGA,		minTier = 2, maxTier = 4 },  Weight = 0.06},
	{{ PickupVariant.PICKUP_BOMB, BombSubType.BOMB_GOLDEN,		minTier = 2, maxTier = 4 },  Weight = 0.03},
	{{ PickupVariant.PICKUP_KEY,  KeySubType.KEY_NORMAL,		minTier = 0, maxTier = 3 }},
}
```

### Example

An example is provided below that combines the previous lua examples to add a custom Turnover shop:

```Lua
local function IsInExampleShop()
	--Would check if your room is your custom room.
	--This is not exclusive to precisely custom rooms, just whatever conditions this Turnover shop should meet to show up when using Turnover.
end

Epiphany.API:AddTurnoverShop({
	Name = "EXAMPLE_SHOP",
	Checker = function()
		return IsInExampleShop()
	end,
	ShopLayout = {
		SetUpPrice = 15
		ShopKeeperPosition = Vector(320,180),
		[0] = {
			SetUpPrice = 5,
			{Vector(280, 320), ShopItemType.Pickup},
			{Vector(360, 320), ShopItemType.Collectible},
			{Vector(360, 360), ShopItemType.Collectible, function (itemPoolType, rng)
				local itemID = rng:RandomInt(CollectibleType.NUM_COLLECTIBLES) + 1
				return itemID
			end},

			{Vector(360, 400), ShopItemType.Slot},
			{Vector(360, 440), ShopItemType.Slot, function (rng)
				local slotID = rng:RandomInt(9) + 1
				return slotID
			end},

			{Vector(360, 400), ShopItemType.RestockMachine}
		},
		[1] = { --Shop Level 2
			SetupPrice = 999,
			{Vector(360, 320), ShopItemType.Collectible, function()
				return CollectibleType.COLLECTIBLE_DEATH_CERTIFICATE
			end},
		}
	},
	PickupPool = {
		{{ PickupVariant.PICKUP_BOMB, BombSubType.BOMB_NORMAL,		minTier = 0, maxTier = 3 }},
		{{ PickupVariant.PICKUP_BOMB, BombSubType.BOMB_DOUBLEPACK,	minTier = 2, maxTier = 4 }},
		{{ PickupVariant.PICKUP_BOMB, BombSubType.BOMB_GIGA,		minTier = 2, maxTier = 4 },  Weight = 0.06},
		{{ PickupVariant.PICKUP_BOMB, BombSubType.BOMB_GOLDEN,		minTier = 2, maxTier = 4 },  Weight = 0.03},
		{{ PickupVariant.PICKUP_KEY,  KeySubType.KEY_NORMAL,		minTier = 0, maxTier = 3 }},
	}
})
```

## Callbacks

Turnover has callbacks for the following:
- Changing the pickup pool
- Changing the shop layout info
- Changing the price to setup/upgrade a shop, or stopping one from spawning altogether
- After a shop is created

You can read more about these callbacks on the [ExtraCallbacks](../enums/ExtraCallbacks.md) page.

## Custom Shopkeeper

Currently, the shopkeepers that spawn for Turnover's shop layouts do not naturally support new sprites. They load with their assigned anm2 and, after being spawned, have their animation changed to the desired shopkeeper.

This brief section provides code for adding your own custom shopkeeper to your Turnover shop:

```Lua

```