# Class "BagData"

???+ info
	You can get this class by using the following function:

	- [BagsInfo.BagData](BagsInfo.md#bagdata)

	???+ example "Example Code"

		```Lua
			local bagsInfo = Epiphany.Item.THROWING_BAG.GetCurrentBagsInfo(Isaac.GetPlayer())
			local bagDataArray = bagsInfo.BagData
			local bagData = bagDataArray[1]
		```


???+ info
	This class contains data for an individual Throwing Bag.

## Variables

### ExtraMass () {: aria-label='Variables' }
#### int ExtraMass {: .copyable aria-label='Variables' }

How heavier or lighter the bag is.

___
### Content () {: aria-label='Variables' }
#### table Content {: .copyable aria-label='Variables' }

Returns a table of CollectibleType strings to how many of that item are present inside the bag.

___
### IsGolden () {: aria-label='Variables' }
#### bool IsGolden {: .copyable aria-label='Variables' }

Whether the bag has a golden item in it. Used for visuals.

___
### Timestamp () {: aria-label='Variables' }
#### int Timestamp {: .copyable aria-label='Variables' }

The time in game ticks that the bag was first created.

___