# Class "BagsInfo"

???+ info
	You can get this class by using the following function:

	- Epiphany.Item.THROWING_BAG:GetCurrentBagsInfo(player)

	???+ example "Example Code"

		```Lua
		local bagsInfo = Epiphany.Item.THROWING_BAG.GetCurrentBagsInfo(Isaac.GetPlayer())
		```

## Variables

### BagData {: aria-label='Variables' }
#### [BagData](BagData.md)[] BagData {: .copyable aria-label='Variables' }

Array of `BagData` objects. The integers represent the order the bags were created in, starting with 1 with the default bag.

___
### BagQueue {: aria-label='Variables' }
#### table BagQueue {: .copyable aria-label='Variables' }

Passes a table that contains information a queue of bags.

Map of integers to integer strings and the variables `Tail` and `Head`, which both hold integers.

???+ info
	- The integer keys represent where the bag is positioned in the queue relative to `Tail` (e.g. The next bag in queue is equal to `Tail`).
	- The gap between `Tail` and `Head` is the number of bags present in the queue.
	- Throwing a bag increases Tail by one, until it reaches `Head`, where there are no more bags to throw. Collecting a bag to add it to the queue with the key equal to Head, and increase `Head` by one.

___
### BaggedItemsList {: aria-label='Variables' }
#### [CollectibleType](https://wofsauge.github.io/IsaacDocs/rep/enums/CollectibleType.html)[] BaggedItemsList {: .copyable aria-label='Variables' }

Array of `CollectibleType`. Displays what items are inside the player's current W.I.P. bag.

___
### BagCountLimit {: aria-label='Variables' }
#### int BagCountLimit {: .copyable aria-label='Variables' }

Maximum number of bags the player is allowed to hold at a time.

Crafting a bag as Tarnished Cain will increase this number by one. Any other character is forced to a count of 1.

___
### ExtraItemsBagged {: aria-label='Variables' }
#### int ExtraItemsBagged {: .copyable aria-label='Variables' }

Number of extra items present in [BaggedItemsList](#baggeditemslist), as to not contribute to the amount of items needed to craft a new bag.

___
### GoldenItemBagged {: aria-label='Variables' }
#### boolean GoldenItemBagged {: .copyable aria-label='Variables' }

Set to `true` if a golden item was added to the current W.I.P. bag. Used for displaying golden visual effect.

___