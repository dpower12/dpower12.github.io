[Turnover](https://tboiepiphany.wiki.gg/wiki/Turnover) is an active item from Epiphany that creates shops unique to the type of room it was created in, containing collectibles from the room's item pool (unique item pools have been made for special rooms that don't normally have one!). The shop can have any number of different levels from repeated uses of the active in the same room, which grant more pickups and items. These unique shops and their levels are also accompanied by their own Shopkeeper sprites for each level (Usually a maximum of 3 sprites). This document covers how to add your own Turnover Shop for your own custom room.

## Variables
Turnover shops are added through the function `Epiphany.API.AddTurnoverShop`, which accepts a table, which should hold the following variables:

| Variable Name | Possible Values | Description |
|:--|:--|:--|
|Name|string|Internal identifier name for the Turnover shop|
|Checker|function|No arguments. Return `true` if this shop should spawn|
|ShopLayout|table|Table containing variables to define the location and types of entities to spawn for the shop and its level|
|PickupPool|table|Table to determine what pickups are possible to spawn inside the shop, separated by shop level|

## ShopLayout

## PickupPool