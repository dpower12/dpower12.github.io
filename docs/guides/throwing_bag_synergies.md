{% include-markdown "hidden/unfinished_notice.md" start="<!-- start -->" end="<!-- end -->" %}

[Throwing Bag](https://tboiepiphany.wiki.gg/wiki/Throwing_Bag) is an active item that serves as [Tarnished Cain](https://tboiepiphany.wiki.gg/wiki/Tarnished_Cain)'s primary method of attacking, throwing the bag and needing to collect it to attack again. Throwing Bag can absorb two items in order to make additional bags to throw, each item contributing a unique effect to the bag it was crafted into. If the item is collected on its own, it may also provide a unique synergy to all of the player's bags.

Epiphany's API allows for adding your own custom synergies for Throwing Bag. This guide will cover the large array of options available for customizing its effect.

## Adding to existing synergies

While Throwing Bag has unique synergies for many notable items, it also has a wide array of synergies that group many items together, all contributing to the same bag effect. This is an easy shortcut to giving an item a Throwing Bag synergy, especially for other mods that may not want to take the time to make completely unique synergies from scratch. To contribute an item to an existing synergy, the function `Epiphany.API:AddCollectibleToCainBagSynergy(syngergyName, collectibles)` accepts an identifier string and an individual or array of CollectibleTypes. Below is a full list of available generic synergies and what items should be part of it. To view the existing items already present in these synergies and their effects, see [this page on the official Epiphany wiki](https://tboiepiphany.wiki.gg/wiki/Throwing_Bag#Bagged_Item_Synergies).

???- info "Throwing Bag generic synergies"
	| string ID | Theme |
	|:--|:--|
	|`angel_bagged`				|Angelic<br>**Auto-includes items with the `angel` tag**|
	|`bait_bagged`				|Inflicts the Bait status|
	|`battery_bagged`			|Battery<br>**Auto-includes items with the `battery` tag**|
	|`birthing_bagged`			|Pregnancy, birth, fetuses|
	|`blackhole_bagged`			|Black hole, void, abyss|
	|`bomb_bagged`				|Bomb synergies|
	|`bone_bagged`				|Bones|
	|`book_bagged`				|Books<br>**Auto-includes items with the `book` tag**|
	|`boss_bagged`				|Represents a boss|
	|`charm_bagged`				|Inflicts the Charm status|
	|`childhood_bagged`			|Childhood|
	|`confusion_bagged`			|Inflicts the Confusion status|
	|`cursed_bagged`			|Voodoo, sacrifice, rituals, shadows|
	|`devil_bagged`				|Devil<br>**Auto-includes items with the `devil` tag**|
	|`dice_bagged`				|Demonic|
	|`drug_bagged`				|Pills, mushrooms, syringes<br>**Please see the note at the bottom of the table.**|
	|`explorer_bagged`			|Exploration, maps, traversal|
	|`familiar_bagged`			|Baby-like familiars<br>**Auto-includes items with the `baby` tag**.|
	|`fart_bagged`				|Triggers fart effects|
	|`fate_bagged`				|Blue Baby, Isaac's death|
	|`fear_bagged`				|Inflicts the Fear status|
	|`fertilizer_bagged`		|Flowers, plants, dirt|
	|`fire_bagged`				|Inflicts the Burn status, fire, flame, hot|
	|`flies_bagged`				|Is a fly, or spawns flies<br>**Auto-includes items with the `fly` tag**.|
	|`gamer_bagged`				|Gamer culture and other games|
	|`glitched_bagged`			|Glitchy|
	|`golden_bagged`			|Gold, money, shops, greed|
	|`guppy_bagged`				|Guppy parts<br>**Auto-includes items with the `guppy` tag**.|
	|`harbinger_bagged`			|Horsemen of the Apocalypse|
	|`homing_bagged`			|Grants homing tears|
	|`isack_bagged`				|Isaac, bums, has significant importance to Isaac's life, Option items|
	|`latex_bagged`				|Latex, razor|
	|`lotto_bagged`				|Lottery, gambling|
	|`luck_bagged`				|Increases Isaac's luck stat|
	|`lunch_bagged`				|Low-quality food<br>**Auto-includes items with the `food` tag**.|
	|`magnet_bagged`			|Magnet, magnetism, inflicts Magnetized status|
	|`meal_bagged`				|High-quality food|
	|`medical_bagged`			|Blood, bandages, medicine, surgical tools, medical conditions|
	|`generic_milk_bagged`		|Milk|
	|`mom_bagged`				|Mom<br>**Auto-includes items with the `mom` tag**.|
	|`mystic_bagged`			|Planetarium, Zodiac signs, runes, fortune telling<br>**Auto-includes items with the `stars` tag**.|
	|`organ_bagged`				|Organ|
	|`paper_bagged`				|Paper, card|
	|`petrifying_bagged`		|Inflicts the Petrify status|
	|`piercing_bagged`			|Grants Isaac piercing tears|
	|`poison_bagged`			|Inflicts the Poison status|
	|`poop_bagged`				|Poop<br>**Auto-includes items with the `poop` tag**.|
	|`punching_bagged`			|Violence, Samson|
	|`purse_bagged`				|Can be found inside a purse|
	|`rock_bagged`				|Rock, and stone|
	|`sack_bagged`				|Sack, pouch|
	|`sharps_bagged`			|Sharp|
	|`slow_bagged`				|Inflicts the Slow status|
	|`spectral_bagged`			|Grants Isaac spectral tears|
	|`spider_bagged`			|Is a spider, or spawns spiders<br>**Auto-includes items with the `spider` tag**.|
	|`spirit_bagged`			|Spirits, ghosts, souls|
	|`sanctum_spiritus_bagged`	|Holy spirits, significant holy items|
	|`tool_bagged`				|Tools for various purposes|
	|`tough_bagged`				|Heavy protection|

???- note "drug_bagged synergies"
	No items are added to this tag directly, instead having its own synergy to change the `burst_color` variable based on the drug, and assigning `drug_bagged` as its parent. When adding to this synergy, use one of the following group tags or create your own with a unique color:

	- `whitedrug`
	- `reddrug`
	- `momspills`
	- `whitepills`

???+ Exclude auto-adding tagged items
	In some cases you may want to create a unique synergy for an item with a specific tag that doesn't contribute to a different synergy. For this, use the function `Epiphany.API:ExcludeItemFromTagSynergy(itemTag, collectibles)` which accepts a tag from the [ItemConfig](https://wofsauge.github.io/IsaacDocs/rep/enums/ItemConfig.html) enumeration to target and an indivudal or array of CollectibleTypes.

## Throwing Bag tables

There are many tables associated with Throwing Bag. While not classes in the literal sense, they are documented as such, and ones pertaining to Throwing Bag are listed below:

???+ note "VSCode autocomplete"
	If you don't want to constantly look back at this documentation while working on custom synergies, it is recommended to use the [Isaac Lua Extention for VSCode](https://marketplace.visualstudio.com/items?itemName=Filloax.isaac-lua-api-vscode). Throwing Bag's file contains EmmyLua annotations for all of these classes. These can be stored in any Lua file, but it is recommended to use a `globals.lua` file at the root of your mod folder, as [sumneko's VSCode Lua extension](https://marketplace.visualstudio.com/items?itemName=sumneko.lua) detects this file for manually defining and recognizing global variables.

- [BagData](../classes/BagData.md)
- [BagMetaData](../classes/BagMetaData.md)
- [BagsInfo](../classes/BagsInfo.md)
- [BagSwingParams](../classes/BagSwingParams.md)
- [BurstParamsConfig](../classes/BurstParamsConfig.md)
- [ImpactBurstParams](../classes/ImpactBurstParams.md)
- [PlayerBagData](../classes/PlayerBagData.md)
- [PlayerSwingParams](../classes/PlayerSwingParams.md)
- [SwingingBagData](../classes/SwingingBagData.md)
- [ThrownBagData](../classes/ThrownBagData.md)

## Creating custom synergies

Adding synergies is done through one largely customizable function:

```Lua
function Epiphany.API:AddCainBagSynergy(synergyName: string, synergyTable:table): table
```

`synergyName` can be anything so long as it does not conflict with an existing synergy name. The usual naming convention for Throwing Bag synergies within Epiphany is using snake case, the name of the item os group name, and whether its bagged, passive, or universal. For example, a bagged synergy for Binge Eater would be `"binge_eater_bagged"`, and a passive synergy for brimstone would be `"brimstone_passive"`.

`synergyTable` will be split into two sections: Basic variables, and callback variables. The callbacks may have their system improved in the future (notably, becoming actual callbacks), but their current implementation will still be covered. There are also actual Throwing bag-related callbacks, which you can find [here](../enums/Epiphany.ExtraCallbacks.md).

### Basic variables

???+ info "Throwing Bag synergy variables"
	|Variable Name|Possible values|Description|
	|:--|:--|:--|
	|id_list|[CollectibleType](https://wofsauge.github.io/IsaacDocs/rep/enums/CollectibleType.html)[]|**Required**. List of collectibles that contribute to the synergy.
	|flags|[BagSynergyFlags](../enums/Epiphany.API.BagSynergyFlags.md)|**Required**. Determines synergy behaviour of when its active.
	|sprite_path|string or function|Can be a string path to an anm2 file for the throwing bag, or a function, which is structured like so:<br>fun([EntityPlayer](https://repentogon.com/EntityPlayer.html), [BagData](../classes/BagData.md), [BagsInfo](../classes/BagsInfo.md)):string?|nil
	|color|[Color](https://repentogon.com/Color.html)|Color applied to the bag. If more than one item in a bag has a color assigned, the color will be lerped between them|
	|burst_color|[Color](https://repentogon.com/Color.html)|Color of tears spawned when the bag impacts a wall, obstacle, or enemy|
	|burst_params|[BurstParamsConfig](../classes/BurstParamsConfig.md)|List of parameters for customizing the tears that spawn when the bag impacts a wall, obstacle, or enemy|
	|callback_priority|integer|Determines the order in which the callbacks are called. Higher priority values go first. Applies to all callbacks added in the table
	|gfx_priority|[BagGFXPriority](../enums/Epiphany.API.BagGFXPriority.md)|Determines priority of synergy graphics. Higher priority values go first|
	|parent|string|Name of existing synergy to parent this synergy to. Will inherent all properties from the parent synergy|

### Callback variables

Some of these callbacks can already be found as actual callbacks under [Epiphany.ExtraCallback](../enums/Epiphany.ExtraCallbacks.md), but serve as for more generic cases, while the callbacks here only run for the assigned synergy. None of these callbacks accept any return parameters.

???+ info "Throwing Bag callback synergy variables"

	- `callback_swing`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, player:[EntityPlayer](https://repentogon.com/EntityPlayer.html), bagData:[SwingingBagData](../classes/SwingingBagData.md))
		<br>Called when the bag starts swinging.
	- `callback_pre_swing`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, player:[EntityPlayer](https://repentogon.com/EntityPlayer.html), bagData:[SwingingBagData](../classes/SwingingBagData.md))
		<br>Called right before `callback_swing`. Can be used to modify bag contents.
	- `callback_swing_update`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, player:[EntityPlayer](https://repentogon.com/EntityPlayer.html), bagData:[SwingingBagData](../classes/SwingingBagData.md))
		<br>Called every frame for each bag that is swinging.
	- `callback_post_bomb_insert`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), bomb:[EntityBomb](https://repentogon.com/EntityBomb.html), count:integer, player:[EntityPlayer](https://repentogon.com/EntityPlayer.html), bagData:[SwingingBagData](../classes/SwingingBagData.md))
		<br>Called when a bomb is inserted into the bag.
	- `callback_throw`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, bagData:[ThrownBagData](../classes/ThrownBagData.md))
		<br>Called when the bag is thrown.
	- `callback_pre_throw`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, bagData:[ThrownBagData](../classes/ThrownBagData.md))
		<br>Called right before `callback_throw`. Can be used to modify bag contents.
	- `callback_flying`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, bagData:[ThrownBagData](../classes/ThrownBagData.md))
		<br>Called every frame while the bag is flying.
	- `callback_update`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, bagData:[ThrownBagData](../classes/ThrownBagData.md))
		<br>Called every frame while thrown bag is flying or on the ground.
	- `callback_pre_hit`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, entity:[Entity](https://repentogon.com/Entity.html), bagData:[ThrownBagData](../classes/ThrownBagData.md))
		<br>Called before thrown bag hits an entity.
	- `callback_post_hit`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, entity:[Entity](https://repentogon.com/Entity.html), bagData:[ThrownBagData](../classes/ThrownBagData.md), dmgDealt:integer)
		<br>Called after thrown bag hits an entity.
	- `callback_hit_bullet`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count, bullet:[EntityProjectile](https://repentogon.com/EntityProjectile.html), bagData:[ThrownBagData](../classes/ThrownBagData.md))
		<br>Called when the thrown bag hits an enemy bullet.
	- `callback_hit_grid`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, gridEntity:[GridEntity](https://repentogon.com/GridEntity.html), bagData:[ThrownBagData](../classes/ThrownBagData.md))
		<br>Called when the thrown bag hits a grid entity.
	- `callback_swing_pre_hit`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, entity:[Entity](https://repentogon.com/Entity.html), player:[EntityPlayer](https://repentogon.com/EntityPlayer.html), bagData:[SwingingBagData](../classes/SwingingBagData.md))
		<br>Called before thrown bag hits an entity while swinging.
	- `callback_swing_post_hit`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, entity:[Entity](https://repentogon.com/Entity.html), player:[EntityPlayer](https://repentogon.com/EntityPlayer.html), bagData:[SwingingBagData](../classes/SwingingBagData.md), dmgDealt:integer)
		<br>Called after thrown bag hits an entity while swinging
	- `callback_swing_hit_bullet`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, bullet:[EntityProjectile](https://repentogon.com/EntityProjectile.html), player:[EntityPlayer](https://repentogon.com/EntityPlayer.html), bagData:[SwingingBagData](../classes/SwingingBagData.md))
		<br>Called when thrown bag hits a projectile while swinging.
	- `callback_unload`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, bagData:[ThrownBagData](../classes/ThrownBagData.md))
		<br>Called when the bag shoots out rocks.
	- `callback_fall`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, bagData:[ThrownBagData](../classes/ThrownBagData.md))
		<br>Called when the bag falls to the ground.
	- `callback_recall`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, bagData:[ThrownBagData](../classes/ThrownBagData.md))
		<br>Called when the bag is recalled.
	- `callback_remove`: fun(bag:[EntityEffect](https://repentogon.com/EntityEffect.html), count:integer, bagData:[ThrownBagData](../classes/ThrownBagData.md))
		<br>Called when the bag is removed.
	- `callback_swing_update_combined`: fun(player:[EntityPlayer](https://repentogon.com/EntityPlayer.html), count:integer, combinedBagData:[BagData](../classes/BagData.md))
		<br>Called every frame while the player is swinging any amount of bags. `combinedBagData` contains the contents of all bags the player is currently swinging, added together.
	- `callback_swing_combined` fun(player:[EntityPlayer](https://repentogon.com/EntityPlayer.html), count:integer, combinedBagData:[BagData](../classes/BagData.md))
		<br>Called when the player starts swinging any amount.