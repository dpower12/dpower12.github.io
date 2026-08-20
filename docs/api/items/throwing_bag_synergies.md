[Throwing Bag](https://tboiepiphany.wiki.gg/wiki/Throwing_Bag) is an active item that serves as [Tarnished Cain](https://tboiepiphany.wiki.gg/wiki/Tarnished_Cain)'s primary method of attacking, throwing the bag and needing to collect it to attack again. Throwing Bag can absorb two items in order to make additional bags to throw, each item contributing a unique effect to the bag it was crafted into. If the item is collected on its own, it may also provide a unique synergy to all of the player's bags.

Epiphany's API allows for adding your own custom synergies for Throwing Bag. This article will cover the large array of options available for customizing its effect.

## Adding to existing synergies

While Throwing Bag has unique synergies for many notable items, it also has a wide array of synergies that group many items together, all contributing to the same bag effect. This is an easy shortcut to giving an item a Throwing Bag synergy, especially for other mods that may not want to take the time to make completely unique synergies from scratch. To contribute an item to an existing synergy, the function `Epiphany.API.AddCollectibleToCainBagSynergy` accepts an identifier string and an array of CollectibleTypes. Below is a full list of available generic synergies and what items should be part of it. To view the existing items already present in these synergies and their effects, see [this page on the official Epiphany wiki](https://tboiepiphany.wiki.gg/wiki/Throwing_Bag#Bagged_Item_Synergies).

???- info "Throwing Bag generic synergies"
	| string ID | Theme |
	|:--|:--|
	|`angel_bagged`				|Angel|
	|`bait_bagged`				|Inflicts the Bait status|
	|`battery_bagged`			|Battery|
	|`birthing_bagged`			|Pregnancy, birth, fetuses|
	|`blackhole_bagged`			|Black hole, void, abyss|
	|`bomb_bagged`				|Bombs|
	|`bone_bagged`				|Bones|
	|`book_bagged`				|Books|
	|`boss_bagged`				|Represents a boss|
	|`charm_bagged`				|Inflicts the Charm status|
	|`childhood_bagged`			|Childhood|
	|`confusion_bagged`			|Inflicts the Confusion status|
	|`cursed_bagged`			|Voodoo, sacrifice, rituals, shadows|
	|`devil_bagged`				|Devil|
	|`dice_bagged`				|Dice|
	|`drug_bagged`				|Pills, mushrooms, syringes|
	|`explorer_bagged`			|Exploration, maps, traversal|
	|`familiar_bagged`			|Baby-like familiars. **Items with the baby tag contribute to this synergy automatically**.|
	|`fart_bagged`				|Triggers fart effects|
	|`fate_bagged`				|Blue Baby, Isaac's death|
	|`fear_bagged`				|Inflicts the Fear status|
	|`fertilizer_bagged`		|Flowers, plants, dirt|
	|`fire_bagged`				|Inflicts the Burn status, fire, flame, hot|
	|`flies_bagged`				|Is a fly, or spawns flies|
	|`gamer_bagged`				|Gamer culture and other games|
	|`glitched_bagged`			|Glitchy|
	|`golden_bagged`			|Gold, money, shops, greed|
	|`guppy_bagged`				|Guppy parts|
	|`harbinger_bagged`			|Horsemen of the Apocalypse|
	|`homing_bagged`			|Grants homing tears|
	|`isack_bagged`				|Isaac, bums, has significant importance to Isaac's life, Option items|
	|`latex_bagged`				|Latex, razor|
	|`lotto_bagged`				|Lottery, gambling|
	|`luck_bagged`				|Increases Isaac's luck stat|
	|`lunch_bagged`				|Low-quality food|
	|`magnet_bagged`			|Magnet, magnetism, inflicts Magnetized status|
	|`meal_bagged`				|High-quality food|
	|`medical_bagged`			|Blood, bandages, medicine, surgical tools, medical conditions|
	|`generic_milk_bagged`		|Milk|
	|`mom_bagged`				|Mom|
	|`mystic_bagged`			|Planetarium, Zodiac signs, runes, fortune telling|
	|`organ_bagged`				|Organ|
	|`paper_bagged`				|Paper, card|
	|`petrifying_bagged`		|Inflicts the Petrify status|
	|`piercing_bagged`			|Grants Isaac piercing tears|
	|`poison_bagged`			|Inflicts the Poison status|
	|`poop_bagged`				|Poop|
	|`punching_bagged`			|Violence, Samson|
	|`purse_bagged`				|Can be found inside a purse|
	|`rock_bagged`				|Rock, and stone|
	|`sack_bagged`				|Sack, pouch|
	|`sharps_bagged`			|Sharp|
	|`slow_bagged`				|Inflicts the Slow status|
	|`spectral_bagged`			|Grants Isaac spectral tears|
	|`spider_bagged`			|Is a spider, or spawns spiders|
	|`spirit_bagged`			|Spirits, ghosts, souls|
	|`tool_bagged`				|Tools for various purposes|
	|`tough_bagged`				|Heavy protection|

## Throwing Bag tables

Before moving onto creating custom synergies for Throwing Bag, this section lists all "classes", or tables of data, associated with Throwing Bag, and the variables contained within. Refer back to this section when working with them.

???+ warning "Incomplete documentation"
	Many variables, and descriptions of classes, are currently missing descriptions. Please feel free to contribute by giving them one!

???+ note "VSCode autocomplete"
	If you don't want to constantly look back at this documentation while working on custom synergies, it is recommended to use the [Isaac Lua Extention for VSCode](https://marketplace.visualstudio.com/items?itemName=Filloax.isaac-lua-api-vscode). Throwing Bag's file contains EmmyLua annotations for all of these classes. These can be stored in any Lua file, but it is recommended to use a `globals.lua` file at the root of your mod folder, as [sumneko's VSCode Lua extension](https://marketplace.visualstudio.com/items?itemName=sumneko.lua) detects this file for manually defining and recognizing global variables.

### BagData
Data of the Bag, not be confused with SwingingBagData and ThrownBagData

???- info "Variables"
	|Variable Name|Possible values|Description|
	|:--|:--|:--|
	|ExtraMass 	| integer	 | How heavier or lighter the bag is |
	|Content 	| BagContent | Table keys are stringified item ids |
	|IsGolden 	| boolean 	 | Whether the bag has a golden item in it. Used for visuals |
	|Timestamp 	| integer	 | The time this bag was created. Used for comparing a bag info with all bag data without having the bag itself |

### SwingingBagData
For a Bag currently being swung.

???- info "Variables"
	|Variable Name|Possible values|Description|
	|:--|:--|:--|
	|BagId				| string		||
	|PlayerOwner		| EntityPlayer	||
	|BagData			| BagData		||
	|DamageMultiplier	| number		||
	|TransitionalData	| table			| Gets moved over to the thrown bag |
	|Data				| table			||
	|LuckBonus			| integer		| Bonus from lucky bag |
	|DamageFlags		| DamageFlag	||
	|ChildBags			| EntityEffect[]| Mechanics and functionality unknown, better avoid using this |
	|FlatDamageBonus	| integer		| Extra damage to be added on top of ThrownDamage |
	|Mass				| number		| Mass of the bag |

### ThrownBagData
For a Bag that has been thrown. Shares all variables from [SwingingBagData](./throwing_bag_synergies.md#swingingbagdata).

???- info "Variables"
	|Variable Name|Possible values|Description|
	|:--|:--|:--|
	|BagBouncesRemaining	| integer			 ||
	|BaggedBombs			| EntityBomb[]		 ||
	|GridBouncesRemaining	| integer			 ||
	|ThrownDamage			| integer			 ||
	|EnemyBouncesRemaining	| integer			 ||
	|BagFlags				| table				 | Table of variable names to booleans. Can hold `PIERCING` and `HOMING`, which changes behaviour of some bags if set to `true`.|
	|CustomVelocity			| Vector			 ||
	|BagUnloadCount			| integer			 ||
	|HitCount				| integer			 ||
	|CanRecall				| boolean, nil		 | Default: `true`. Set to `false` so the bag cannot be recalled, only picked up manually.|
	|Aura					| EntityEffect		 | An `EntityEffect` that will follow the bag|
	|OriginalOffset			| Vector			 ||
	|IsFalling				| boolean			 ||
	|CanPickUp				| boolean			 ||
	|IgnoreGrid				| boolean			 ||
	|IgnoreWalls			| boolean			 ||
	|SlowingFactor			| number			 | Bag velocity will be multiplied by this number every frame. default: 0.97|
	|ImpactBurstParams		| ImpactBurstParams[]||
	|IsFirstBagInPool		| boolean			 ||
	|IgnoreEnemies			| boolean			 ||
	|LastPosition			| Vector			 ||

### ImpactBurstParams

???- info "Variables"
	|Variable Name|Possible values|Description|
	|:--|:--|:--|
	|color			| Color, nil		||
	|count			| number, nil		||
	|count_std		| number, nil		||
	|speed			| number, nil		||
	|speed_std		| number, nil		||
	|damage_multi	| number, nil		||
	|variant		| TearVariant, nil	||
	|tear_flags		| TearFlags, nil	||

### BurstParamsConfig

???- info "Variables"
	|Variable Name|Possible values|Description|
	|:--|:--|:--|
	|count_add_multi	| number, nil		| Minimum multiplier of tears that are spawned based on tearrate (default: 1.5) |
	|count_std_multi	| number, nil		| Maximum value for random addition to multiplier of tears (default: 0.1665) |
	|speed_add_multi	| number, nil		| Minimum multiplier of amount of speed (value 15) of tears (default: 1) |
	|speed_std_multi	| number, nil		| Maximum value for random addition to multiplier of speed (value 15) of tears (default: 0.2) |
	|damage_multi		| number, nil		| Damage multiplier of tears (default: 1) |
	|variant			| TearVariant, nil	| TearVariant of tears (default: TearVariant.BLUE) |
	|flags				| TearFlags, nil	| TearFlags of tears (default: 0 AKA none) |

### BagsInfo

???- info "Variables"
	|Variable Name|Possible values|Description|
	|:--|:--|:--|
	|BagData			| table					| Table of strings to BagData
	|BagQueue			| table					|
	|BaggedItemsList	| CollectibleType|[]	|
	|BagCountLimit		| integer				|
	|ExtraItemsBagged	| integer				|
	|GoldenItemBagged	| boolean				| Indicates whether a golden item was added to the current WIP bag. Used for displaying golden visual effect

### BagMetaData

???- info "Variables"
	|Variable Name|Possible values|Description|
	|:--|:--|:--|
	|BagsData 	  | BagsInfo[]  ||
	|CurrentIndex | integer 	| Index of the currently used bag queue in BagsData array |
