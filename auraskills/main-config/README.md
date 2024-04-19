---
description: Guide to the config.yml file
---

# Main Config

The `config.yml` is the main plugin configuration file found in the `plugins/AuraSkills` folder. It is used for general or miscellaneous config options related to storage/database, external plugin hooks, languages, action bar, boss bar, worlds/regions, modifiers, requirements, and more.

Options for each [skill](../skills/) and stat were formerly here, but have been moved to their own `skills.yml` and `stats.yml` files.

If an option you see in the config is missing, this page may not have been updated yet or the option may have been removed. You can find any config additions and changes in the full plugin [changelog](https://github.com/Archy-X/AureliumSkills/blob/master/Changelog.txt).

Last Updated Version: `2.0.6`

## Options

### SQL

`sql:`

* `enabled` - Whether SQL should be used for data storage (requires a restart to enable).
* `type` - The type of SQL database to use; currently only `mysql` is supported.
* `host` - SQL hostname
* `port`- Port (must be number)
* `database` - Database name (must be created already)
* `username` - SQL username
* `password` - SQL password
* `load_delay` - Number of ticks to delay loading data after a player joins, useful for syncing multiple servers to a single database.
* `always_load_on_join` - If true, player data will always be loaded from the database when a player joins, regardless if it is already in memory.
* `ssl` - Whether to use SSL.

### Languages

`default_language` - The default language for players; must have a file that matches (ex: `messages_en.yml` for `en`)

`try_detect_client_language` - If set to `true`, the plugin will try to use the client's language, if available and valid. This is only for players who have not set a language using commands, or if their language was reset after a server restart. If the client language is not a valid plugin language, it will use the `default-language`. If set to `false`, all unset players will use the `default-language`.

`languages` - A list of languages players can switch to using `/skills lang <language>`; must also have a file that matches. Custom language files are defined here.

### Hooks

`hooks:`

Options for each hook are under the section with the plugin name.

* `enabled` - Whether the hook to the given plugin should be registered. Hooks will only attempt to be loaded if the given plugin is detected and enabled, so hooks can be set to `true` without having the plugin on the server. Some hooks, like HolographicDisplays and DecentHolograms perform the same function, so one must be disabled before the other can be enabled.

#### WorldGuard

* `WorldGuard:`
  * `blocked_regions` - Players in regions on this list will not be able to gain XP naturally in any skill.
  * `blocked_check_replace_regions` - Regions on this list will disable block sources checking if the block broken has been player placed.

### Action Bar

`action_bar:`

* `enabled` - Whether the action bar should be enabled/disabled. (Must be set to `true` to have any action bars; setting to `false` disables all action bar types).
* `idle` - Controls the idle action bar (not gaining xp). **Enable this if you want the action bar to always display.**
* `ability` - Controls the action bar for ability messages (raise/lower, activate, etc.). If set the false, the ability messages will be sent through chat instead.
* `xp` - Controls the action bar for gaining xp (not maxed)
* `maxed` - Controls the action bar when xp is gained in a maxed skill.
* `update_period` - How often the action bar should update, in ticks (Increase this value if action bar is causing lag).
* `round_xp` - If enabled, current xp will be rounded to an integer.
* `placeholder_api` - Whether PlaceholderAPI placeholders should be replaced in the action bar, given that you have PlaceholderAPI.
* `use_suffix` - Whether to format the current player's XP with number suffixes (k, m, etc). Only applies if `xp` is set to true.

### Boss Bar

`boss_bar:`

* `enabled` - Whether boss bars should be enabled for xp gains.
* `mode` - Can be either `single` or `multi`. `multi` means multiple boss bars will display if gaining XP from different types of skills, `single` is only one at a time.
* `stay_time` - How long the boss bar should stay up after not gaining xp, in ticks.
* `update_every` - Controls how often the boss bar should update when gaining xp consecutively, increase if having lag issues.
* `round_xp` - If enabled, the current xp will be rounded to an integer.
* `use_suffix` - Whether to format the current player's XP with number suffixes (k, m, etc).
* `format` - The format list allows you to change the boss bar color and style for each skill:
  * Format: '\[SKILL] \[COLOR] \[STYLE]'
  * Available colors are BLUE, GREEN, PINK, PURPLE, WHITE, RED, and YELLOW
  * Available styles are SOLID, NOTCHED\_6, NOTCHED\_10, NOTCHED\_12, and NOTCHED\_20

`base_mana` - The base amount of mana players should have at 0 Wisdom.

`enable_roman_numerals` - Whether Roman numerals should be used for skill levels.

### Damage Holograms

`damage_holograms:`

* `enabled` - Enable/disable damage holograms (requires either HolographicDisplays or DecentHolograms hook to be enabled and that plugin to be on the server).
* `scaling` - Whether the damage displayed on holograms should be scaled according to the `action_bar_scaling` option of the hp trait in `stats.yml`.
* `decimal:`
  * `display_when_less_than:` - Display decimals in damage holograms when less than a specified damage.
  * `max_amount` - The maximum amount of decimal digits to display.
* `offset:`
  * `x` - X coordinate offset
  * `y` - Y coordinate offset
  * z - Z coordinate offset
  * `random`:
    * `enabled` - Whether random hologram positions should be enabled.
    * `x_min` - Minimum X coordinate offset
    * `x_max` - Maximum X coordinate offset
    * `y_min` - Minimum Y coordinate offset
    * `y_max` - Maximum Y coordinate offset
    * `z_min` - Minimum Z coordinate offset
    * `z_max` - Maximum Z coordinate offset

### Leaderboards

`leaderboards:`

* `update_period` - How often leaderboards should be updated, in ticks.
* `update_delay` - How long after server startup should the leaderboards be updated, in ticks (does not include the immediate update on startup).

`start_level` - The skill level that players start at. Defaults to 0, use 1 to revert to Beta mechanics.

`enable_skill_commands` - Whether skill name commands should be enabled such as `/farming` or `/mining` (Requires restart to have an effect).

`check_block_replace:`

* `enabled` - Whether blocks placed by players should not give xp; keep `true` unless you are having plugin compatibility issues.
* `blocked_worlds` - A list of worlds that should not be checked for block replacement. Checking will be disabled in these worlds regardless of what `enabled` is set to.

### Worlds and Regions

`blocked_worlds` - Players in worlds on this list will not be able to gain xp naturally in any skill.

`disabled_worlds` - Most of the plugin's gameplay functionality will be disabled in worlds on this list, including but not limited to stats, abilities, gaining xp, and the action bar (commands and menus will still be available).

`disable_in_creative_mode` - Whether players should not be able to gain xp while in creative mode.

### Death options

`on_death:`

* `reset_skills` - Whether to reset a player's skill levels when they die.
* `reset_xp` - Whether to reset a player's XP in their current skill levels when they die. Skill levels are not changed.

### Auto-save

`auto_save:`

* `enabled` - Whether data for online players should save periodically instead of just when they log out. This is useful if you experience skill data losses due to server crashes.
* `interval_ticks` - How often (in ticks) to auto-save.

### Leveler

`leveler:`

* `title:`
  * `enabled` - Whether a title should be displayed to players on skill level up.
  * `fade_in` - Title fade in time, in ticks
  * `stay` - How long the title should last, in ticks
  * `fade_out` - Title fade out time, in ticks
* `sound:`
  * `enabled` - Whether a sound should be played to players on skill level up.
  * `type` - The name of the sound that should be played (must be a valid sound name).
  * `category` - The sound category the sound should be played in.
  * `volume` - Sound volume
  * `pitch` - Sound pitch
* `double_check_delay` - The level up check delay for large xp gains at once, in ticks (lower is faster).

### Mana

`mana:`

* `enabled` - If false, mana abilities will not cost mana to use and mana displays will be hidden from the action bar and menus.

### Modifier

`modifier:`

* `armor:`
  * `equip_blocked_materials` - A list of blocks that should not grant stats of armor when right-clicked; add to this list when stats are given but armor is not equipped.
* `item:`
  * `check_period` - How often, in ticks, the item held in a player's hand should be checked for stat item modifiers (increase if you have lag)
  * `enable_off_hand` - Whether stat modifiers should work in the off hand
* `auto_convert_from_legacy` - Whether the old modifier nbt format should be converted to the new one. Set to false if you are having performance issues and all your items have been converted.

### Requirement

`requirement:`

* `enabled` - Whether requirements should be checked at all. If you do not use requirements, disabling will improve performance.
* `item:`
  * `prevent_tool_use` - Whether block breaking should be blocked when a player does not meet a requirement
  * `prevent_weapon_use` - Whether attacking entities should be blocked when a player does not meet a requirement
  * `prevent_block_place` - Whether block placing should be blocked when a player does not meet a requirement
  * `prevent_interact` - Whether interacting (right clicking) should be blocked when a player does not meet a requirement
  * `global` - Define item requirements that should apply to every item of that type. Format: - '\[material] \[skill\_1]:\[level\_1] \[skill\_2]:\[level\_2] ...'
* `armor:`
  * `prevent_armor_equip` - Whether armor should be unable to be equipped when a player does not meet a requirement
  * `global` - Define armor requirements that should apply to every item of that type. Format: - '\[material] \[skill\_1]:\[level\_1] \[skill\_2]:\[level\_2] ...'

### Critical

`critical:`

* `base_multiplier` - The base damage multiplier for critical hits
* `enabled` - Options in this category control whether that item type should be able to deal critical hits. (`hand` is for empty fist, `other` is for holding any other item not on the list)

### Source

`grindstone:`

* `blocked_enchants` - A list of enchantments that should be blocked from counting XP for grindstone sources. Add unremovable enchants/curses from custom enchant plugins here.

`statistic:`

* `gain_period_ticks` - How often statistic sources should give XP. This does not change the effective rate of XP gain, just the time between statistic checks.

`entity:`

* `give_alchemy_on_potion_combat` - Whether killing/damaging an entity using potions should be counted as Alchemy XP instead of Fighting/Archery.

### Menus

`menus:`

* `lore_wrapping_width` - The number of characters per line before newlines are automatically inserting in menu lore lines where wrapping is enabled.
* `placeholder_api` - Whether PlaceholderAPI placeholders should be used in menus.
* `stats:`
  * `show_trait_values_directly` - If true, stats that have a single trait with a `modifier` of exactly 1 will show the value of the trait instead in menus. This allows trait base values, such as the 20 HP every player has by default, to be included in the shown level.

### Loot

`loot:`

* `update_loot_tables` - Whether new loot items introduced in default configs should be automatically added when updating the plugin.

`check_for_updates` - Whether the plugin should check for new updates on startup and when a player with the `aureliumskills.checkupdates` permission joins

### Automatic Backups

`automatic_backups:`

* `enabled` - Whether automatic backups should be taken on server shutdown
* `minimum_interval_hours` - The minimum interval, in hours, between automatic backups. Automatic backups will only be taken at least this amount of hours after the last one.

`save_blank_profiles` - If false, player data of players who have not leveled any skills or gained any XP will not be saved into storage.
