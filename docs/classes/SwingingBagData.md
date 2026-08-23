# Class "SwingingBagData"

???+ info
	You can get this class by using the following function:

	- Epiphany.Item.THROWING_BAG:GetSwingingBagData(swingingBag)
	- Epiphany.Item.THROWING_BAG:CreateSwingingBag(player, bagsInfo, bagId)

	???+ example "Example Code"

		```Lua
			local THROWING_BAG = Epiphany.Item.THROWING_BAG
			local SWING_BAG_VARIANT = THROWING_BAG.SWINGING_BAG_EFFECT
			local bag = Isaac.FindByType(EntityType.ENTITY_EFFECT, SWING_BAG_VARIANT)[1]
			if bag then
				local swingParams = THROWING_BAG:GetSwingingBagData(bag)
			end
		```

???+ info
	Detailed information pertaining to a swung bag, not to be confused with [BagSwingParams](BagSwingParams.md).

## Variables

### BagId {: aria-label='Variables' }
#### string BagId {: .copyable aria-label='Variables' }

___
### PlayerOwner {: aria-label='Variables' }
#### [EntityPlayer](https://repentogon.com/EntityPlayer.html) PlayerOwner {: .copyable aria-label='Variables' }

___
### BagData {: aria-label='Variables' }
#### [BagData](BagData.md) BagData {: .copyable aria-label='Variables' }

___
### DamageMultiplier {: aria-label='Variables' }
#### float DamageMultiplier {: .copyable aria-label='Variables' }

___
### TransitionalData {: aria-label='Variables' }
#### table TransitionalData {: .copyable aria-label='Variables' }

Can hold arbitrary data that is preserved when the swung bag is thrown.

___
### Data {: aria-label='Variables' }
#### table Data {: .copyable aria-label='Variables' }

Holds arbitrary data.

___
### LuckBonus {: aria-label='Variables' }
#### int LuckBonus {: .copyable aria-label='Variables' }

Bonus luck from Lucky Bag.

___
### DamageFlags {: aria-label='Variables' }
#### [DamageFlag](https://wofsauge.github.io/IsaacDocs/rep/enums/DamageFlag.html) DamageFlags {: .copyable aria-label='Variables' }

___
### FlatDamageBonus {: aria-label='Variables' }
#### float FlatDamageBonus {: .copyable aria-label='Variables' }

Extra damage to be added on top of [ThrownBagData.ThrownDamage](ThrownBagData.md#throwndamage).

___
### Mass {: aria-label='Variables' }
#### float Mass {: .copyable aria-label='Variables' }

Mass of the bag. Affects its total damage output.

___