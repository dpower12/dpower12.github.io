# Class "BurstParamsConfig"

???+ info
	Configuration of variables exclusively for Throwing Bag synergies. Is later combined with the respective variables in [ImpactBurstParams](ImpactBurstParams.md).

## Variables

### count_add_multi {: aria-label='Variables' }
#### float|nil count_add_multi {: .copyable aria-label='Variables' }

Minimum multiplier of tears that are spawned based on tearrate (default: 1.5)

___
### count_std_multi {: aria-label='Variables' }
#### float|nil count_std_multi {: .copyable aria-label='Variables' }

Maximum value for random addition to multiplier of tears (default: 0.1665)

___
### speed_add_multi {: aria-label='Variables' }
#### float|nil speed_add_multi {: .copyable aria-label='Variables' }

Minimum multiplier of amount of speed (value 15) of tears (default: 1)

___
### speed_std_multi {: aria-label='Variables' }
#### float|nil speed_std_multi {: .copyable aria-label='Variables' }

Maximum value for random addition to multiplier of speed (value 15) of tears (default: 0.2)

___
### damage_multi {: aria-label='Variables' }
#### float|nil damage_multi {: .copyable aria-label='Variables' }

Damage multiplier of tears (default: 1)

___
### variant {: aria-label='Variables' }
#### [TearVariant](https://wofsauge.github.io/IsaacDocs/rep/enums/TearVariant.html)|nil variant {: .copyable aria-label='Variables' }

TearVariant of tears (default: TearVariant.BLUE)

___
### flags {: aria-label='Variables' }
#### [TearFlags](https://wofsauge.github.io/IsaacDocs/rep/enums/TearFlags.html)|nil flags {: .copyable aria-label='Variables' }

TearFlags of tears (default: TearFlags.TEAR_NORMAL)

___
