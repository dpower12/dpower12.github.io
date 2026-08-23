# Class "ThrownBagData"

???+ info
	You can get this class with the following function:

	- Epiphany.Item.THROWING_BAG:GetThrownBagData(thrownBag)

	???+ example "Example Code"

		```Lua
			local THROWING_BAG = Epiphany.Item.THROWING_BAG
			local THROWN_BAG_VARIANT = THROWING_BAG.DETACHED_BAG_VARIANT
			local bag = Isaac.FindByType(EntityType.ENTITY_EFFECT, THROWN_BAG_VARIANT)[1]
			if bag then
				local swingParams = THROWING_BAG:GetThrownBagData(bag)
			end
		```
???+ info
	This class inherits from [SwingingBagData](SwingingBagData.md).

## Variables

### BagBouncesRemaining {: aria-label='Variables' }
#### int BagBouncesRemaining {: .copyable aria-label='Variables' }

The number of times the bag can bounce off of other bags.

Calculated as `player.TearRange // 200` (1 bounce for every 5 range)

___
### BaggedBombs {: aria-label='Variables' }
#### [EntityBomb](https://repentogon.com/EntityBomb.html)[] BaggedBombs {: .copyable aria-label='Variables' }

Array of EntityBombs held inside the bag.

___
### GridBouncesRemaining {: aria-label='Variables' }
#### int GridBouncesRemaining {: .copyable aria-label='Variables' }

The number of times the bag can bounce off of grid entities.

Calculated as `player.TearRange // 180` (1 bounce for every 4.5 range)

___
### ThrownDamage {: aria-label='Variables' }
#### int ThrownDamage {: .copyable aria-label='Variables' }

The base damage of the bag. Does not reflect the final damage dealt to enemies.

???+ note "Damage calculation"
	Damage is caluclated using the formula below, which takes velocity, mass into consideration before adding multipliers and flat bonuses:

	```Lua
	local speed = Velocity:Length()
	local massMultiplier = 0.1
	local damageMultiplier = ThrownBagData.DamageMultiplier or 1.0

	-- The damage dealt to the enemy is based on the speed of the bag and the mass of the bag
	local massOfBag = (effect.Mass / massMultiplier / 10.0)
	-- Complicated formula to calculate the damage dealt to the enemy
	ThrownBagData.ThrownDamage = math.min(player.Damage * massOfBag * ((speed / 40.0) ^ 2) * 3.0 * damageMultiplier,
		player.Damage * 4 * damageMultiplier)
	ThrownBagData.ThrownDamage = ThrownBagData.ThrownDamage + ThrownBagData.FlatDamageBonus
	```
___
### EnemyBouncesRemaining {: aria-label='Variables' }
#### int EnemyBouncesRemaining {: .copyable aria-label='Variables' }

The number of times the bag can bounce off of enemies.

Calculated as `player.TearRange // 300` (1 bounce for every 9.5 range)

___
### BagFlags {: aria-label='Variables' }
#### table BagFlags {: .copyable aria-label='Variables' }

Can hold arbitrary variables set to `true` to indicate changing certain behaviour about the bag.

Currently only holds `PIERCING` and `HOMING`, which is set by other synergies when active.

___
### CustomVelocity {: aria-label='Variables' }
#### [Vector](https://repentogon.com/Vector.html) CustomVelocity {: .copyable aria-label='Variables' }

The velocity of the bag.

___
### BagUnloadCount {: aria-label='Variables' }
#### int BagUnloadCount {: .copyable aria-label='Variables' }

The number of times the bag has collided with something and shot out a burst of tears on impact.

___
### BagUnloadCount {: aria-label='Variables' }
#### int BagUnloadCount {: .copyable aria-label='Variables' }

The number of times the bag has hit an enemy.

___
### CanRecall {: aria-label='Variables' }
#### boolean CanRecall {: .copyable aria-label='Variables' }

If `false`, the bag cannot be recalled, only picked up manually. Otherwise, can be recalled by using the Throwing Bag active item.

___
### OriginalOffset {: aria-label='Variables' }
#### [Vector](https://repentogon.com/Vector.html) OriginalOffset {: .copyable aria-label='Variables' }

Stored [Entity.PositionOffset](https://wofsauge.github.io/IsaacDocs/rep/Entity.htmlf#positionoffset) right before the bag falls to the ground. Is restored to the bag if it is recalled.

___
### IsFalling {: aria-label='Variables' }
#### boolean IsFalling {: .copyable aria-label='Variables' }

Returns `true` if the bag has fallen, even if it visually appears still on the ground. Set back to `false` when recalled.

___
### CanPickUp {: aria-label='Variables' }
#### boolean CanPickUp {: .copyable aria-label='Variables' }

Returns if the player can collect the bag from the ground. Is set to `true` once the bag has finished its falling animation.

___
### IgnoreGrid {: aria-label='Variables' }
#### boolean IgnoreGrid {: .copyable aria-label='Variables' }

Returns `true` if the bag will ignore collision with grid entities.

___
### IgnoreWalls {: aria-label='Variables' }
#### boolean IgnoreWalls {: .copyable aria-label='Variables' }

Returns `true` if the bag will ignore collision with walls.

___
### SlowingFactor {: aria-label='Variables' }
#### float SlowingFactor {: .copyable aria-label='Variables' }

Bag velocity will be multiplied by this number every frame. (Default: 0.97)

___
### ImpactBurstParams {: aria-label='Variables' }
#### [ImpactBurstParams](ImpactBurstParams.md)[] ImpactBurstParams {: .copyable aria-label='Variables' }

Array of `ImpactBurstParams`.

___
### IsFirstBagInPool {: aria-label='Variables' }
#### boolean IsFirstBagInPool {: .copyable aria-label='Variables' }

Set during Epiphany's custom [Pool!](https://tboiepiphany.wiki.gg/wiki/Pool!) challenge. Causes the default bag to not deal any damage or shoot any tears upon impact.

___
### IgnoreEnemies {: aria-label='Variables' }
#### boolean IgnoreEnemies {: .copyable aria-label='Variables' }

Ignores colliding with and damaging enemies. Is set to `true` when being recalled.

___
### LastPosition {: aria-label='Variables' }
#### [Vector](https://repentogon.com/Vector.html) LastPosition {: .copyable aria-label='Variables' }

Stores the last position the bag was located at. Updates every game tick.

___
