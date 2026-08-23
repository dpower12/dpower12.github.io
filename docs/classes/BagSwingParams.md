# Class "BagSwingParams"

???+ info
	You can get this class by using the following function:

	- Epiphany.Item.THROWING_BAG:GetBagSwingParams(swingingBag)
	- Epiphany.Item.THROWING_BAG:CreateSwingingBag(player, bagsInfo, bagId)

	???+ example "Example Code"

		```Lua
		local THROWING_BAG = Epiphany.Item.THROWING_BAG
		local SWING_BAG_VARIANT = THROWING_BAG.SWINGING_BAG_EFFECT
		local bag = Isaac.FindByType(EntityType.ENTITY_EFFECT, SWING_BAG_VARIANT)[1]
		if bag then
			local swingParams = THROWING_BAG:GetBagSwingParams(bag)
		end
		```

???+ info
	Basic data pertaining to a swung Throwing Bag, not to be confused with [SwingingBagData](SwingingBagData.md).

## Variables

### SwingingAngle {: aria-label='Variables' }
#### number SwingingAngle {: .copyable aria-label='Variables' }

Current rotational angle of the bag.

___
### SwingingEnemyCollisionCooldown {: aria-label='Variables' }
#### table SwingingEnemyCollisionCooldown {: .copyable aria-label='Variables' }

Map of entity indexes to a cooldown. Enemies cannot be hit with the bag while this number is above `0`.

___
### BaggedBombs {: aria-label='Variables' }
#### [EntityBomb](https://repentogon.com/EntityBomb.html)[] BaggedBombs {: .copyable aria-label='Variables' }

Array of EntityBombs held inside the bag.

___