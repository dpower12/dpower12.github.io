# Class "PlayerSwingParams"

???+ info
	You can get this class with the following function:

	- Epiphany.Item.THROWING_BAG:GetPlayerSwingParams(player)

	???+ example "Example Code"

		```Lua
		local playerBagData = Epiphany.Item.THROWING_BAG:GetPlayerSwingParams(Isaac.GetPlayer())
		```

## Variables

### SwingingDuration {: aria-label='Variables' }
#### integer SwingingDuration {: .copyable aria-label='Variables' }

Returns `0` if the player is not swinging any Throwing Bags, and increments by `player.ShotSpeed` otherwise, with a limit defined by [SwingingDurationCap](#swingdurationcap).

While swung, used to modify certain attributes like the position and length of the bag, as well as pitch of swinging sounds.

___

### SwingingBagRef {: aria-label='Variables' }
#### [EntityEffect](https://repentogon.com/EntityEffect.html)[] SwingingBagRef {: .copyable aria-label='Variables' }

Array of Throwing Bag entities currently being swung.

___
### SwingingDirection {: aria-label='Variables' }
#### float SwingingDirection {: .copyable aria-label='Variables' }

___
### SwingDurationCap {: aria-label='Variables' }
#### float SwingDurationCap {: .copyable aria-label='Variables' }

The defined cap for [SwingignDuration](#swingingduration).

???+ info "SwingDurationCap calculation"
	The cap is decided by the following code:

	```Lua
	local swingDurationCap = math.max(math.min(240, player.TearRange / 8), Epiphany.Item.THROWING_BAG:GetBagChargeTime(player))
	```

___