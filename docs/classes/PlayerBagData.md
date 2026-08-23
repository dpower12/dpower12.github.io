# Class "PlayerBagData"

???+ info
	You can get this class with the following function:

	- Epiphany.Item.THROWING_BAG:GetPlayerThrowingBagData(player)

	???+ example "Example Code"
		```Lua
		local playerBagData = Epiphany.Item.THROWING_BAG:GetPlayerThrowingBagData(Isaac.GetPlayer())
		```

???- info "Synergy data"
	This class may also be used to hold arbitrary data for synergies. This includes:

	- BrimstoneRing `EntityPtr`.
	- GamerBagLastSpawn `integer`.
	- Tech2SwingLasers `EntityPtr[]`.
	- TechXLaserList `EntityPtr[]`.
	- EpiphoraCainChargeBar `Sprite`.

## Variables

### BagMode () {: aria-label='Variables' }
#### boolean BagMode {: .copyable aria-label='Variables' }

Returns `true` if in "Throwing Bag" mode, being unable to fire tears and will throw Throwing Bags instead, if any are on the player.

___
### CanSwing () {: aria-label='Variables' }
#### boolean CanSwing {: .copyable aria-label='Variables' }

Returns `true` if the player is able to swing a Throwing Bag. Will always be true for Tarnished Cain, but otherwise is `false` if any of the following requirements are met:

- Curse mist is active
- Bag queue is empty
- Player is holding Urn of Souls or Notched Axe

___
### SwingParams () {: aria-label='Variables' }
#### [PlayerSwingParams](PlayerSwingParams.md) SwingParams {: .copyable aria-label='Variables' }

___
### IsHoldingItem () {: aria-label='Variables' }
#### boolean IsHoldingItem {: .copyable aria-label='Variables' }

Returns `true` is the player has an item queued for collection.

___
### BaggingCandidate () {: aria-label='Variables' }
#### [EntityPtr](https://wofsauge.github.io/IsaacDocs/rep/EntityPtr.html)|nil BaggingCandidate {: .copyable aria-label='Variables' }

The pedestal closest to Isaac to potentially bag.

___
### IsHoldingBag () {: aria-label='Variables' }
#### boolean IsHoldingBag {: .copyable aria-label='Variables' }

Returns `true` after initializing the bag sprite above Isaac's head when collecting an item, `false` after putting the item down.

___
### BagPickupSprite () {: aria-label='Variables' }
#### [Sprite](https://repentogon.com/Sprite.html) BagPickupSprite {: .copyable aria-label='Variables' }

Bag sprite above Isaac's head when they're able to bag an item.

___
### BagChargebar () {: aria-label='Variables' }
#### [Sprite](https://repentogon.com/Sprite.html) BagChargebar {: .copyable aria-label='Variables' }

Chargebar visual while swinging a bag.

___
### LastRecallCount () {: aria-label='Variables' }
#### int LastRecallCount {: .copyable aria-label='Variables' }

How many bags were last recalled. Used to decide [BagPocketCharge](#BagPocketCharge).

___
### BagPocketCharge () {: aria-label='Variables' }
#### float BagPocketCharge {: .copyable aria-label='Variables' }

Active item charge for Throwing Bag. Only used for Tarnished Cain.

___
### SoundCooldown () {: aria-label='Variables' }
#### int SoundCooldown {: .copyable aria-label='Variables' }

Cooldown between playing the swinging bag "woosh" sound effect.

Is set to `20` when first played and decreases by `3 + player.ShotSpeed` each game tick.

___