---
description: Guide to skills and the skills.yml file
---

# Skills

**Skills** are measures of a player's experience or progress in different aspects of the game. As the main feature of AuraSkills, all other features are connected or related to skills. All information about skills is viewed in the skills menu using `/skills`. Each skill has a set of sources: specific tasks or actions used to gain XP in that skill. Players level up by reaching the XP requirement for the next level, which increases with higher skill levels.

Upon reaching a new skill level, players are rewarded with some of the following:

* Stat levels
* Ability unlocks and level-ups
* Mana Ability unlocks and level-ups
* Other customizable [rewards](rewards.md), which can include:
  * Commands
  * Permissions
  * Items
  * Money

There are 15 skills included in the plugin:

* Farming
* Foraging
* Mining
* Fishing
* Excavation
* Archery
* Defense
* Fighting
* Endurance
* Agility
* Enchanting
* Alchemy
* Sorcery
* Healing
* Forging

## Configuration

Skills are configured in the `skills.yml` file. Each skill has a section named after its full Namespaced ID (prefixed with `auraskills/` for all default skills). Below is an example skill section for Farming with comments explaining each option:

{% code title="skills.auraskills/farming" %}
```yaml
auraskills/farming: # The Namespaced ID of the skill
  abilities: # The list of abilities the skill levels up
    - auraskills/bountiful_harvest
    - auraskills/farmer
    - auraskills/scythe_master
    - auraskills/geneticist
    - auraskills/triple_harvest
  mana_ability: auraskills/replenish # The mana ability of the skill (optional)
  options: # Configurable options for the skill, see below for details on each option
    enabled: true
    max_level: 97
    check_cancelled: true
    check_multiplier_permissions: true
```
{% endcode %}

### Abilities

The `abilities` key is a list of the abilities attached to the skill (will level up as the skill levels up). The ability name must be a full Namespaced ID. All other configuration for abilities is done in `abilities.yml`.

### Mana Ability

The `mana_ability` key is the name (full Namespaced ID) of the mana ability of the skill. This is an optional value, as not all skills have mana abilities. All other configuration for mana abilities is done in `mana_abilities.yml`.

### Options

#### General Options

* `enabled` - Whether the skill and all its connected features should be enabled. If set to false, players cannot gain XP in the skill, use the abilities or mana abilities, and the skill will be hidden from menus. Disabling a skill will not reset player levels, so re-enabling a skill will retain previously gained levels.
* `max_level` - The maximum attainable level of a skill. Lowering this level will not decrease player levels that are already higher than the new max level.
* `check_cancelled` - Whether source levelers should ignore cancelled events when giving XP. This should not be changed unless you have a specific compatibility issue that requires it.
* `check_multiplier_permissions` - Whether permission-based multipliers (`auraskills.multiplier.*`) should work. If you do not use permission multipliers, setting this to false may improve performance.

#### Archery and Fighting Options

* `damage_based` - If true, XP will be given when a player damages a mob, rather than on the mob's death. This will still use the xp values in the sources files to calculate the given xp _per damage done_ (given XP = source XP \* damage). Thus, it is recommended to greatly lower the values in the sources files if setting this option to true.
* `spawner_multiplier` - Multiplies the given XP by this value for mobs spawned by a mob spawner block. Set to 0 to disable XP from spawners.

#### Defense Options

* `max` - The maximum amount of XP to reward for defense.
* `min` - The minimum amount of XP required to be rewarded. If the XP is lower than min, no XP will be given.
* `allow_shield_blocking` - Whether XP should be given when a player is hit while blocking with a shield.

#### Endurance Options

* `xp_gain_period` - How often to give Endurance XP, in ticks. This will apply to all statistic sources.

#### Alchemy Options

* `give_xp_on_takeout` - If true, XP for Alchemy will be given when the potion is taken out of the brewing stand by a player, rather then as soon as the potion is brewed. Setting this to true will prevent automatic brewing from giving XP.
* `give_xp_on_potion_combat` - If true, using a splash potion to damage an entity will give XP for Alchemy instead of Fighting/Archery.
* `ignore_custom_potions` - If true, potions with custom effects (non-brewable potions) will not work with Alchemy abilities such as Alchemist, Sugar Rush, and Splasher.

#### Healing Options

* `exclude_negative_potions` - If true, drinking or splashing potions that give negative effects (debuffs) will not give Healing XP.

#### Forging Options

* `blocked_grindstone_enchants` - A list of enchants to not give XP for when disenchanting an item in a grindstone. Names of unremovable enchants (curses) from custom enchant plugins should be added here.

## Commands

The names of skills can be used to directly open the level progression menus for that skill, such as `/farming`, `/archery`, etc. This can be disabled by setting `enable_skill_commands` to false in `config.yml`.

Skill levels can be manipulated using the following commands:

* `/skills skill setlevel <player> <skill> <level>`
* `/skills skill setall <player> <skill> <level>`
* `/skills skill reset <player> [skill]`

## Menu

The skills menu is accessed using `/skills`. The design and format of the menu is configured in `menus/skills.yml`.

## Messages

The display names and descriptions of skills can be changed under the `skills` section of the messages file.

