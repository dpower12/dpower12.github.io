Epiphany takes certain types of entities and splits them into different groups in order to better categorize them. This allows us to have finer control over what is spawned under specific conditions, such as only spawning "demonic beggars" in Turnover shops. Below is a brief display on how to add your modded content to Epiphany's various groups:

## Slot Machines

```Lua
Epiphany.API:AddSlotsToSlotGroup(groupName: string, ...: integer)
```

`groupName` uses the [Epiphany.API.SlotGroup](../enums/Epiphany.API.SlotGroup.md) enumeration. The vararg accepts any number of slot variants.

## Hearts

```Lua
Epiphany.API:AddHeartsToHeartGroup(groupName: string, ...: integer)
```

`groupName` uses the [Epiphany.API.HeartGroup](../enums/Epiphany.API.HeartGroup.md) enumeration. The vararg accepts any number of heart subtypes.

???+ note
	At this time, the function does not support different pickup variants for hearts. This may change in the future, as Epiphany has other heart-related systems that do support it.

## Cards

```Lua
Epiphany.API:AddCardsToCardGroup(groupName: string, ...: integer|table)
```

`groupName` uses the [Epiphany.API.CardGroup](../enums/Epiphany.API.CardGroup.md) enumeration. The vararg accepts any number of card IDs, or tables.

The tables must have a `V` variable assigned as the card ID, and optionally a `Weight` variable to define how likely the card is to be selected. Higher weights appear more often over lesser ones. Example below:

```Lua
--Add Wild Card to TAROT group for no particlar reason with 10x reduced weight, and The Fool card a second time.
Epiphany.API:AddCardsToCardGroup(Epiphany.API.CardGroup.TAROT, {V = Card.CARD_WILD, Weight = 0.1}, {V = Card.CARD_FOOL, Weight = 1})
```

???+ warning
	There is an "All" group, but it is only used internally to use a card ID of -1 for a random card. Do not add to this group.
