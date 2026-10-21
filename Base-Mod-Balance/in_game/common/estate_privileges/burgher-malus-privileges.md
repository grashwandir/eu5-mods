# Vanilla privileges with a burgher malus

Every vanilla estate privilege that lowers burgher satisfaction or burgher power. Blocks are copied verbatim from the local install (extracted 2026-10-21). Paths are relative to `game/in_game/common/estate_privileges/`.

Five privileges lower `burghers_estate_target_satisfaction`. `nihil_novi` lowers `global_burghers_estate_power` instead.

## Summary

| Privilege | Estate | Source | Burgher malus | Available to |
|---|---|---|---|---|
| `noble_leaders_only` | nobles | `nobles_estate.txt:152` | satisfaction −0.025 (small) | everyone (no `potential`) |
| `jewish_court_financier_charter` | nobles | `nobles_estate.txt:1863` | satisfaction −0.05 (medium), scaled by 1 + Jewish share | Christian/Muslim state with Jewish pops |
| `nihil_novi` | nobles | `nobles_estate.txt:1425` | power −0.25 (no satisfaction malus) | `has_variable = unlock_nihil_novi` |
| `maintain_clerical_proceedings` | clergy | `clergy_estate.txt:285` | satisfaction −0.025 (small) | republic |
| `mandatory_clerical_donations` | clergy | `clergy_estate.txt:320` | satisfaction −0.05 (medium) | republic + `maintain_clerical_proceedings` |
| `jewish_sabbath_economy` | clergy | `clergy_estate.txt:867` | satisfaction −0.05 (medium) | state religion Judaism |

## Named values used

Resolved from `game/main_menu/common/script_values/default_values.txt`.

| Name | Value |
|---|---|
| `diplomatic_reputation_mild_penalty` | -2 |
| `large_privilege_target_satisfaction` | 0.1 |
| `large_tax_income_efficiency_bonus` | 0.05 |
| `medium_privilege_target_satisfaction` | 0.05 |
| `medium_privilege_target_satisfaction_penalty` | -0.05 |
| `small_privilege_target_satisfaction_penalty` | -0.025 |
| `societal_value_minor_monthly_move` | 0.05 |

## `noble_leaders_only`

`nobles_estate.txt`, lines 152–172. Burgher malus: satisfaction −0.025 (small).

```
noble_leaders_only = {
	estate = nobles_estate
	country_modifier = {
		no_lowborn_leaders = yes
		peasants_estate_blocked_from_leading_military = yes
		tribes_estate_blocked_from_leading_military = yes
		burghers_estate_blocked_from_leading_military = yes
		dhimmi_estate_blocked_from_leading_military = yes
		cossacks_estate_blocked_from_leading_military = yes
		clergy_estate_blocked_from_leading_military = yes
		peasants_estate_blocked_from_leading_military = yes
		tribes_estate_blocked_from_leading_military = yes
		global_nobles_estate_power = 0.5
		nobles_estate_target_satisfaction = medium_privilege_target_satisfaction
		burghers_estate_target_satisfaction = small_privilege_target_satisfaction_penalty
		peasants_estate_target_satisfaction = small_privilege_target_satisfaction_penalty
		cossacks_estate_target_satisfaction = small_privilege_target_satisfaction_penalty
		tribes_estate_target_satisfaction = small_privilege_target_satisfaction_penalty
		dhimmi_estate_target_satisfaction = small_privilege_target_satisfaction_penalty
	}
}
```

## `jewish_court_financier_charter`

`nobles_estate.txt`, lines 1863–1890. Burgher malus: satisfaction −0.05 (medium), scaled by 1 + Jewish share.

```
jewish_court_financier_charter = {
	estate = nobles_estate
	potential = {
		religion ?= {
			OR = {
				group ?= religion_group:christian
				group ?= religion_group:muslim
			}
		}
		religion_percentage_in_country = {
			religion = religion:judaism
			value > 0
		}
	}
	country_modifier = {
		global_nobles_estate_power = 0.25
	}
	country_modifier = {
		burghers_estate_target_satisfaction = medium_privilege_target_satisfaction_penalty
		clergy_estate_target_satisfaction = small_privilege_target_satisfaction_penalty
		diplomatic_reputation = diplomatic_reputation_mild_penalty
		global_merchant_capacity_modifier = 0.10
		scale = {
			value = "religion_percentage_in_country(religion:judaism)"
			add = 1
		}
	}
}
```

## `nihil_novi`

`nobles_estate.txt`, lines 1425–1451. Burgher malus: power −0.25 (no satisfaction malus).

```
nihil_novi = {
	estate = nobles_estate
		
	potential = {
		has_variable = unlock_nihil_novi
	}
	allow = {
		
	}
	can_revoke = {
		trigger_if = {
			limit = {
				NOT = { has_country_modifier = plc_strive_for_reform }
			}
			estate_satisfaction:nobles_estate > 0.8
		}
	}
	
	country_modifier = {
		nobles_estate_target_satisfaction = medium_privilege_target_satisfaction
		global_nobles_estate_power = 0.25
		change_policy_cost_modifier = 1
		global_peasants_migration_allowed = no
		global_burghers_estate_power = -0.25
		nobles_estate_levy_size = 1 
	}
}
```

## `maintain_clerical_proceedings`

`clergy_estate.txt`, lines 285–299. Burgher malus: satisfaction −0.025 (small).

```
maintain_clerical_proceedings = {
	estate = clergy_estate 

	potential = {
		government_type = government_type:republic
	}

	
	country_modifier = {
		nobles_estate_target_satisfaction = small_privilege_target_satisfaction_penalty
		burghers_estate_target_satisfaction = small_privilege_target_satisfaction_penalty
		clergy_estate_target_satisfaction = large_privilege_target_satisfaction
		global_clergy_estate_power = 0.2
	}
}
```

## `mandatory_clerical_donations`

`clergy_estate.txt`, lines 320–340. Burgher malus: satisfaction −0.05 (medium).

```
mandatory_clerical_donations = {
	estate = clergy_estate 

	potential = {
		government_type = government_type:republic
	}

	
	allow = {
		has_estate_privilege = estate_privilege:maintain_clerical_proceedings
	}

	
	country_modifier = {
		tax_income_efficiency = large_tax_income_efficiency_bonus
		nobles_estate_target_satisfaction = medium_privilege_target_satisfaction_penalty
		burghers_estate_target_satisfaction = medium_privilege_target_satisfaction_penalty
		clergy_estate_target_satisfaction = large_privilege_target_satisfaction
		global_clergy_estate_power = 1
	}
}
```

## `jewish_sabbath_economy`

`clergy_estate.txt`, lines 867–881. Burgher malus: satisfaction −0.05 (medium).

```
jewish_sabbath_economy = {
	estate = clergy_estate
	potential = {
		religion ?= religion:judaism
	}
	country_modifier = {
		burghers_estate_target_satisfaction = medium_privilege_target_satisfaction_penalty
		clergy_estate_target_satisfaction = medium_privilege_target_satisfaction
		global_clergy_estate_power = 0.25
		global_pop_assimilation_speed_modifier = -0.15
		monthly_prestige = 0.05
		monthly_towards_communalism = societal_value_minor_monthly_move
		trade_income = -0.05	#WAD, decision by johan
	}
}
```

-----------------------------------------------------------------------------
# CUSTOM

I found 6 vanilla privileges that already lower burgher satisfaction, so I built six propositions on their pattern and value scales. All modifier and script-value names exist in 1.4.x (I checked them against `00_modifier_types.txt` and `default_values.txt`). None has been tested in-game.

**Vanilla precedents**
- `noble_leaders_only`: +0.5 noble power, medium noble satisfaction, **small** burgher penalty ([nobles_estate.txt:152](in_game/common/estate_privileges/nobles_estate.txt#L152)).
- `jewish_court_financier_charter`: **medium** burgher penalty, paid for with +10% merchant capacity ([nobles_estate.txt:1863](in_game/common/estate_privileges/nobles_estate.txt#L1863)).
- `nihil_novi`: `global_burghers_estate_power = -0.25` ([nobles_estate.txt:1448](in_game/common/estate_privileges/nobles_estate.txt#L1448)).
- The step sizes are tiny 0.01, small 0.025, medium 0.05 and large 0.1, from `main_menu/common/script_values/default_values.txt:944-952`.

---

### 1. Seigneurial Tolls
Lords collect tolls at their bridges, fords and river castles. It mirrors the burghers' `toll_and_tariff_exemptions` ([burghers_estate.txt:395](in_game/common/estate_privileges/burghers_estate.txt#L395)), which gives +0.10 land trade efficiency.
```
seigneurial_tolls = {
	estate = nobles_estate

	allow = {
		NOT = { has_estate_privilege = estate_privilege:toll_and_tariff_exemptions }
	}

	country_modifier = {
		nobles_estate_target_satisfaction = medium_privilege_target_satisfaction
		burghers_estate_target_satisfaction = medium_privilege_target_satisfaction_penalty
		global_nobles_estate_power = 0.33
		nobles_estate_max_tax = 0.05
		trade_land_efficiency = small_trade_land_efficiency_penalty
		monthly_towards_decentralization = societal_value_minor_monthly_move
	}
}
```
**Trade:** the tolls make the nobles richer, so you can tax them a bit more. In return, land trade efficiency drops by 10%.

### 2. Sumptuary Ordinances
Silk, fur and some colours are reserved for people of noble rank. This is a cheap way to gain prestige.
```
sumptuary_ordinances = {
	estate = nobles_estate

	potential = {
		government_type = government_type:monarchy
	}

	country_modifier = {
		nobles_estate_target_satisfaction = small_privilege_target_satisfaction
		burghers_estate_target_satisfaction = small_privilege_target_satisfaction_penalty
		global_nobles_estate_power = 0.25
		monthly_prestige = 0.05
		monthly_legitimacy = 0.02
		monthly_towards_aristocracy = societal_value_minor_monthly_move
	}
}
```
**Trade:** small gains for a small cost. Vanilla has a `sumptuary_law`, but it is a Nahuatl-only law, so the two don't collide.

### 3. Noble Monopoly on Land
Commoners may not buy landed estates. Poland's Piotrków statutes are the classic example.
```
noble_monopoly_on_land = {
	estate = nobles_estate

	allow = {
		NOT = { has_estate_privilege = estate_privilege:burghers_land_rights }
	}

	country_modifier = {
		nobles_estate_target_satisfaction = medium_privilege_target_satisfaction
		burghers_estate_target_satisfaction = medium_privilege_target_satisfaction_penalty
		global_nobles_estate_power = 0.5
		global_burghers_estate_power = -0.1
		nobles_estate_levy_size = 0.2
		monthly_towards_serfdom = societal_value_minor_monthly_move
		monthly_towards_aristocracy = societal_value_monthly_move
	}
}
```
**Trade:** the landholders owe military service, so noble levies grow by 20% (the same value as `auxilium_et_consilium`). The cost is a power shift toward the nobles.

### 4. Manorial Workshops
Lords can run workshops on their estates without following guild rules.
```
manorial_workshops = {
	estate = nobles_estate

	allow = {
		NOT = { has_estate_privilege = estate_privilege:formal_guilds }
	}

	country_modifier = {
		nobles_estate_target_satisfaction = small_privilege_target_satisfaction
		burghers_estate_target_satisfaction = medium_privilege_target_satisfaction_penalty
		global_nobles_estate_power = 0.33
		global_production_efficiency = medium_production_efficiency_bonus
	}
}
```
**Trade:** +2.5% production efficiency. Vanilla uses `medium_production_efficiency_bonus` five times in privileges and laws. The guilds are the ones who lose out.

### 5. Noble Customs Exemption
Goods that nobles import or export for their own use pay no duty. This is a pure appeasement option.
```
noble_customs_exemption = {
	estate = nobles_estate

	country_modifier = {
		nobles_estate_target_satisfaction = large_privilege_target_satisfaction
		burghers_estate_target_satisfaction = small_privilege_target_satisfaction_penalty
		global_nobles_estate_power = 0.2
		trade_income = -0.05
		monthly_towards_aristocracy = societal_value_minor_monthly_move
	}
}
```
**Trade:** a lot of noble satisfaction for little noble power, paid for with 5% trade income (the same value as vanilla `jewish_sabbath_economy`).

### 6. Noble Seats in Town Councils
Some seats on town councils are reserved for nobles. This is a tool to curb burghers who have grown too strong.
```
noble_seats_in_town_councils = {
	estate = nobles_estate

	potential = {
		government_type = government_type:monarchy
	}

	country_modifier = {
		nobles_estate_target_satisfaction = small_privilege_target_satisfaction
		burghers_estate_target_satisfaction = small_privilege_target_satisfaction_penalty
		global_nobles_estate_power = 0.25
		global_burghers_estate_power = -0.25
		monthly_political_influence_gain_modifier = -0.10
	}
}
```
**Trade:** the payoff is the burgher power reduction itself, as with `nihil_novi`. It costs −10% political influence gain, the same cost as `banal_lordship` and `manorial_courts`.

---

### Summary

| # | Privilege | Noble sat. | Noble power | Burgher sat. | Payoff | Cost |
|---|---|---|---|---|---|---|
| 1 | Seigneurial Tolls | +0.05 | 0.33 | −0.05 | +5% noble max tax | −10% land trade |
| 2 | Sumptuary Ordinances | +0.025 | 0.25 | −0.025 | prestige, legitimacy | — |
| 3 | Monopoly on Land | +0.05 | 0.5 | −0.05 | +20% noble levies | −0.1 burgher power |
| 4 | Manorial Workshops | +0.025 | 0.33 | −0.05 | +2.5% production | locks out guilds |
| 5 | Customs Exemption | +0.1 | 0.2 | −0.025 | appeasement | −5% trade income |
| 6 | Town Council Seats | +0.025 | 0.25 | −0.025 | −0.25 burgher power | −10% PI gain |

**Caveats**
- **Historical background is unverified.** The flavour lines (Rhine-style toll castles, the 1496 Piotrków ban on burghers buying land, the Polish nobles' customs exemption) come from my general knowledge. I didn't check them against sources.
- **The `allow` blocks only work one way.** They stop you from enacting the noble privilege while the burgher one is active. They don't stop the burgher privilege being enacted afterwards. To block both directions, the burgher privilege would also need editing, which isn't confirmed to work.
- **Unchecked guesses:** the value of `monthly_prestige = 0.05` (borrowed from `jewish_sabbath_economy`) and whether a 20% noble levy is too much early on.
- **Each privilege needs localisation keys** before it shows properly in-game.

I can drop the ones you pick into one of your mods.
