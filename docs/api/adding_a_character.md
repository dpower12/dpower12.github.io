In addition to the work-in-progress set of tarnished characters for the vanilla cast, Epiphany supports adding tarnished versions of modded characters. On top of everything that comes with creating a new character entry, this document covers everything you'd need to know about adding and configuring your own tarnished character.

## Setting up your character

This article assumes you know how to create a modded character to begin with. If for some reason you don't, see [this page on Isaac Blueprints](https://isaacblueprints.com/tutorials/crash_course/character/).

Setting up your character entry should be identical to making a normal modded character. All the actions that handle making it a tarnished character are done through Lua, not the XML files, but there will be additions needed there as well. For this document, we will be creating a tarnished version of a character named "Bluesaac".

Before Wave 8, you could create a tarnished character on its lonesome without the requirement of a normal or tainted version. With the implementation of tarnished characters directly on the main menu, it overrides an existing character on the menu. As such, **you MUST have at least one other character entry to add a tarnished character**. This may change in the future, but for now you need a character that will show up on the main menu at all times.

### Achievements

Due to how tarnished characters are implemented to be accessible from the main menu, **you MUST add at least one achievement tied to your character**, regardless of intention. This is so that your character can be hidden from the main menu but accessible in the co-op menu. In most cases, however, you will want to make your tarnished unlockable, and you will require two achievements. For the purposes of the document, we will be making Tarnished Guppy unlockable.

```xml
<achievements gfxroot="gfx/ui/achievement/">
    <!-- The character achievement must be "CHARACTER_" followed by the name of your character entered into the Epiphany API. This is what the player will actually see when you unlock the tarnished. -->
    <achievement name="CHARACTER_BLUESAAC" text='You unlocked "Guppy"' gfx="tarnished_achievement_character_guppy.png" gfxback="tarnished_achpaper.png" />
    <!-- The menu achievement must be "HIDE_MENU_" with the same requirements above. You MUST have this achievement for your character whether or not it is unlockable.-->
	<!-- Be sure to add the hidden attribute as this isn't an achievement meant to be seen -->
    <achievement name="HIDE_MENU_BLUESAAC" hidden="true" />
</achievements>
```

### Character entry

Inside the players.xml, your character will be setup like a regular non-tainted character. Majority of things your character starts out with can be managed here thanks to REPENTOGON, the main highlights being a costume, pocket active, and basic stats. See [here](https://repentogon.com/xml/players.html) for a full list of variables.

Following the pattern of tainteds, you may want to name your tarnished the same as its regular variant. To prevent name clashing, the unicode symbol `U+200B`, or "ZERO WIDTH SPACE", is used as a unique identifier without compromising the name of the character. Add this character to the start of your tarnished's name. It can be copied from [here](https://unicode-explorer.com/c/200B). This is not necessary if your tarnished uses a unique name.

Lastly, preventing your character from showing up on the menu as a regular character entry is done using the `hideachievement` attribute. Assign the HIDE_MENU achievement here.

```xml
<players root="gfx/characters/costumes/" portraitroot="gfx/ui/stage/" nameimageroot="gfx/ui/boss/">
	<player name="Bluesaac"
		birthright="???" skin="character_bluesaac.png" hp="2"
		nameimage="playername_bluesaac.png"
		portrait="playerportrait_bluesaac.png"
		skinColor="1"
	/>
	<player name="Bluesaac" bSkinParent="Bluesaac"
		birthright="???" skin="character_bluesaac_b.png" hp="2"
		nameimage="playername_bluesaac.png"
		portrait="playerportrait_bluesaac_b.png"
		skinColor="1"
	/>
	<player name="​Bluesaac"
		birthright="???" skin="character_bluesaac_c.png" hp="2" hideachievement="HIDE_MENU_BLUESAAC"
		nameimage="playername_bluesaac.png"
		portrait="playerportrait_bluesaac_c.png"
		skinColor="1"
	/>
</players>
```

### Menu sprites

The menu assets of tarnisheds characters are handled manually, so a unique anm2 must be created to hold the required sprites.

The anm2 must have the following animations present:
(TODO: Show example images!!)

- "Portrait": The character's menu portrait while unlocked.
- "Text": The character's menu text. This includes name, arrows, stats, etc. First frame for when they're locked, second frame for unlocked.
- "Door": The character's menu portrait while locked.

Layers and spritesheets are not relevant to the anm2 setup; just the animations and frames.

## Optional additions

Not required by the Epiphany API, this section covers optional additions to your tarnished that warrant more detail.

### Item-specific hair costumes

As of Wave 8, a system was created to allow for custom variants of a tarnished's hair costume depending on the currently active head costume from an item or null item. A lot of automation is in place to handle the majority of work involved, but your tarnished must have the `modcostume` and `costumeSuffix` variables present in their `players.xml`.

???- note "Head costumes only"
	This system only supports items that change the `head` layer, such as Brimstone, Ipecac, etc. An item that doesn't change that layer, such as Sad Onion, will have no effect.

When adding your tarnished through the Epiphany API, it can accept a table map of CollectibleType (or NullItemID) to strings, the latter of which will be the filenames of the associated spritesheet inside your tarnished's costume suffix folder. See below for example:

(TODO: Show image of costume suffix folder holding a head costume and hair costume)

```Lua
	local hairCostumeItems = {
		[CollectibleType.COLLECTIBLE_IPECAC] = "gross_ipecac_hair.png",
		[COLLECTIBLE_MY_CUSTOM_ITEM] = "super_awesome_hair.png"
	}
	local hairCostumeNulls = {
		[NullItemID.ID_GUPPY] = "furry_guppy_hair.png"
	}
```

### Overriding collectibles and trinkets

REPENTOGON allows the ability to block certain collectibles and trinkets per-player, acting as if they don't have the item at all. This is desireable for tarnisheds that may want to create a unique synergy/interaction without any of the attributes of the item itself.

Epiphany allows you to assign specific items and trinkets to your tarnished that will always be blocked on only that tarnished. It will be unblocked when switching characters. The following functions can be used for convenience:

- Epiphany:HasBlockedCollectible(`EntityPlayer` player, `CollectibleType` item)
- Epiphany:GetBlockedCollectibleNum(`EntityPlayer` player, `CollectibleType` item)
- Epiphany:HasBlockedTrinket(`EntityPlayer` player, `TrinketType` trinket)

## Registering the tarnished

Adding your tarnished to Epiphany's API uses the function `Epiphany.API.AddCharacter`. It accepts one argument; a table of parameters that holds information on your character:

???+ info "Required variables"
	`charName`, `charID`, `playerAnm2`, and `menuGraphics` are required variables. `taintedID` is not enforced, but is recommended. All other variables are optional
|Variable Name|Possible Values|Description|
|:--|:--|:--|
|charName|string|The name of your tarnished. **This is used as an important identifier** outside of the tarnished's PlayerType. It must match the name used in the HIDE_MENU and CHARACTER achievement|
|charID|integer|PlayerType of your tarnished using [Isaac.GetPlayerTypeByName](https://wofsauge.github.io/IsaacDocs/rep/Isaac.html#getplayertypebyname)|
|playerAnm2|string|Path to an anm2 file that your tarnished will use in place of the default player anm2. See Epiphany's own player anm2 files in `gfx/characters/` as a reference to copy from|
|taintedID|integer|PlayerType of the associated tainted using [Isaac.GetPlayerTypeByName](https://wofsauge.github.io/IsaacDocs/rep/Isaac.html#getplayertypebyname)|
|unlockChecker|function|Doesn't pass any arguments. Return a `boolean` for whether or not the tarnished is unlocked|
|floorTutorial|string|Path to an anm2 file. Must have one animation named "Tutorial" that holds the character tutorial. Unlike vanilla, does not support dynamic input sprites, so it must display keys manually|
|charStats|table|Accepts (bool)`FLYING`, (TearFlags)`TEAR_FLAGS`, (Color)`TEAR_COLOR`, and (Color)`LASER_COLOR` as variables inside the table. More are available, but remain for backwards compatibility pre-Wave 8|
|nullStats|integer|ID for a null item obtained with [Isaac.GetNullItemIdByName](https://repentogon.com/Isaac.html#getnullitemidbyname). Use for applying flat/multiplicative tears/damage to your tarnished|
|menuGraphics|string|File path to the anm2 file that contains menu assets. See [Menu sprites](./adding_a_character.md#menu-sprites)|
|bloodTears|boolean|Set to `true` to have your tarnished shoot blood variants of tears|
|hairCostumeItems|table|Map of CollectibleType to string containing a png filename. See [Item-specific hair costumes](./adding_a_character.md#item-specific-hair-costumes)|
|hairCostumeNulls|table|Map of NullItemID to string containing a png filename. See [Item-specific hair costumes](./adding_a_character.md#item-specific-hair-costumes)|
|blockedItems|table|Array of CollectibleType that will be blocked for the tarnished, acting as if they don't have the item. See [Overriding collectibles and trinkets](./adding_a_character.md#overriding-collectibles-and-trinkets)|
|blockedTrinkets|table|Array of TrinketType that will be blocked for the tarnished, acting as if they don't have the item. See [Overriding collectibles and trinkets](./adding_a_character.md#overriding-collectibles-and-trinkets)|