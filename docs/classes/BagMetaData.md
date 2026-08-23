# Class "BagMetaData"

???+ info
	You can get this class by using the following function:

	- Epiphany.Item.THROWING_BAG:GetBagMetaData(player)

	???+ example "Example Code"

		```Lua
		local bagMetaData = Epiphany.Item.THROWING_BAG.GetBagMetaData(Isaac.GetPlayer())
		```
???+ info
	"Global scope of

## Variables

### BagsData {: aria-label='Variables' }
#### [BagsInfo](BagsInfo.md)[] BagsData {: .copyable aria-label='Variables' }

Array of `BagsInfo`. Not to be confused with `BagData`, which is data on an individual bag.

___
### CurrentIndex {: aria-label='Variables' }
#### table CurrentIndex {: .copyable aria-label='Variables' }

Index of the currently used bag queue in BagsData array.

___