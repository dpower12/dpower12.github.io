There are various interactions across the Epiphany mod that allow for mod compatibility but can be explained in a sentence or two, thus requiring a full-on guide. This guide covers these interactions both inside and outside of the API file.

## Essence of Cain
[Essence of Cain](https://tboiepiphany.wiki.gg/wiki/Essence_of_Cain) is a rune that reseals every chest and repairs every slot machine in the room. Vanilla has closed chests as subtype 1 and open as 0, but some mods may do the opposite. This table assigns variants to the expected subtype for a closed chest:

```Lua
Epiphany.Essence.CAIN.CLOSED_CHEST[var: PickupVariant] = subtype: integer
```

When sealing chests and repairing slot machines, it also looks for pedestals spawned by them. As they're completely converted into a new entity, they need to have data added to them to know what they were previously. You can use the callback [ESSENCE_OF_CAIN_REVERT_PEDESTAL](../enums/Epiphany.ExtraCallbacks.md#essence_of_cain_revert_pedestal) to do so. If you have a custom chest or slot that can grant a collectible and displays a unique pedestal sprite, you should assign the pedestal custom data to detect within this callback to know if it should be converted back into your chest/slot machine.

## Midas Curse

[Tarnished Keeper](https://tboiepiphany.wiki.gg/wiki/Tarnished_Keeper) cannot interact with a majority of pickups in the game unless they cost money to obtain, as they will turn into coins otherwise. This is not desireable for some pickups, as it may be essential that the player be allowed to pick them up. You can add to the following table to blacklist a specific variant and/or subtype from being converted to coins. Note that this may be changed in the future to use a custom entity tag instead:

```Lua
Epiphany.Character.KEEPER.DisallowedPickupVariants[var: PickupVariant] = true
--OR
Epiphany.Character.KEEPER.DisallowedPickupVariants[var: PickupVariant][subtype: integer] = true
```

## Debug

When [Tarnished Eden](https://tboiepiphany.wiki.gg/wiki/Tarnished_Eden) uses [Debug](https://tboiepiphany.wiki.gg/wiki/Debug) while holding an item above their head, it prevents that item from being rerolled by Debug or entering new rooms. If there's an item that exists in the form of multiple items, commonly with active items such as Glass Cannon, they can be added to the following list such that protecting one item under Debug will also protect the other assigned item from being rerolled:

```Lua
Epiphany.Item.DEBUG.COMPATIBILITY_TABLE[protectedItem: CollectibleType] = secondaryItem: CollectibleType
```

## Bad Company

[Bad Company](https://tboiepiphany.wiki.gg/wiki/Bad_Company) allows shopkeepers to be paid for and turned into friendly Greeds or Super Greeds. As Epiphany has their own unique shopkeeper entities that are not meant to be presented as one, a blacklist exists to stop any entity with type `EntityType.ENTITY_SHOPKEEPER`, or `17`, them from being hireable and turned into a friendly Greed. At the moment, you can add to the following table, but this may be changed in the future to use a custom entity tag instead:

```Lua
Epiphany.Item.BAD_COMPANY.KEEPER_VARIANT_BLACKLIST[shopkeeperVariant: integer] = true
```

## Heart values

There are a total of three characters that need a unique value out of heart pickups to contribute towards their own unique mechanics:

- [Tarnished Magdalene](https://tboiepiphany.wiki.gg/wiki/Tarnished_Magdalene); collecting red hearts at full health to gain bonus heart containers.
- [Tarnished Samson](https://tboiepiphany.wiki.gg/wiki/Tarnished_Samson); collecting hearts during his Immortal Rage mode to convert into "tokens" that fill his Slaugher Bar.
- [Tarnished Bethany](https://tboiepiphany.wiki.gg/wiki/Tarnished_Bethany); collecting hearts that are instantly absorbed into her heart charges.

Different values may assigned to a heart pickup across each character, depending on the context. The following functions can be used to assign a unique value to a pickup with variant `PickupVariant.PICKUP_HEART`, or `10`, but may optionally accept a different pickup variant:

???- info "Samson Token values"
	List of vanilla hearts to their token value to server as a reference:

	- HeartSubType.HEART_HALF = 20
	- HeartSubType.HEART_FULL = 40
	- HeartSubType.HEART_HALF_SOUL = 40
	- HeartSubType.HEART_SOUL = 80
	- HeartSubType.HEART_BLACK = 100
	- HeartSubType.HEART_BLENDED = 60
	- HeartSubType.HEART_BONE = 120
	- HeartSubType.HEART_DOUBLEPACK = 80
	- HeartSubType.HEART_ETERNAL = 100
	- HeartSubType.HEART_ROTTEN = 30
	- HeartSubType.HEART_SCARED = 40
	- HeartSubType.HEART_GOLDEN = 0 (Coins spawning handled by callback)

```Lua
--Each function accepts a table that maps HeartSubTypes to integers, and a second optional argument for a different pickup variant
local exampleTable = {
	[HeartSubType.HEART_FULL] = 2,
	[HeartSubType.HEART_SOUL] = 4
}
--Maggy should only exclusively use red hearts! Values are 1:1 with heart value
Epiphany.API:AddMaggyHeartValue(subtypeToValue: table, variant?: PickupVariant)
--Tokens contributed are in multiples of 10 and depends on the "value" of the heart itself.
Epiphany.API:AddSamsonHeartValue(subtypeToValue: table, variant?: PickupVariant)
--Exact
Epiphany.API:AddBethanyHeartValue(subtypeToValue: table, variant?: PickupVariant)
```

If a heart's value may be different under different conditions, the following callbacks are available:

- [MAGDALENE_PRE_CONSUME_HEART](../enums/Epiphany.ExtraCallbacks.md#magdalene_pre_consume_heart)
- [SAMSON_PRE_CONSUME_HEART](../enums/Epiphany.ExtraCallbacks.md#samson_pre_consume_heart)
- [BETHANY_PRE_CONSUME_HEART](../enums/Epiphany.ExtraCallbacks.md#bethany_pre_consume_heart)

## Blighted Dice

When [Blighted Dice](https://tboiepiphany.wiki.gg/wiki/Blighted_Dice) is used and turned into Broken Dice, players can interact with item pedestals to consume them, turning Broken Dice back into Blighted Dice and giving it full overcharge. For characters that are salvage-type characters, being Tainted Cain and Tarnished Keeper, they are blacklisted from this unique interaction and lets their normal behaviour of salvaging the collectible happen instead. If you have a custom character that has similar mechanics, add them to the following table:

```Lua
Epiphany.Item.BLIGHTED_DICE.PLAYER_TYPE_COLLISION_BLACKLIST[playerType: PlayerType] = true
```