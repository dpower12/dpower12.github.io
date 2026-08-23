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

W.I.P!