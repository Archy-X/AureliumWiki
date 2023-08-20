---
description: Guide to the config.yml file
---

# Main Config

The `config.yml` is the main plugin configuration file found in the `plugins/AureliumSkills` folder. It is used for general or miscellaneous config options related to storage/database, external plugin hooks, languages, action bar, boss bar, worlds/regions, modifiers, requirements, and more.

Options for each skill and stat were formerly here, but have been moved to their own `skills.yml` and `stats.yml` files.&#x20;

If an option you see in the config is missing, this page may not have been updated yet or the option may have been removed. You can find any config additions and changes in the full plugin [changelog](https://github.com/Archy-X/AureliumSkills/blob/master/Changelog.txt).

Last Updated Version: `2.0.0`

## Options

### MySQL

`mysql:`

* `enabled` - Whether MySQL should be used for data storage (requires a restart to enable)
* `host` - MySQL hostname
* `port`- Port (must be number)
* `database` - Database name (must be created already)
* `username` - MySQL username
* `password` - MySQL password
* `load-delay` - Number of ticks to delay loading data after a player joins, useful for syncing multiple servers to a single database
* `always-load-on-join` - If true, player data will always be loaded from the database when a player joins, regardless if it is already in memory
* `ssl` - Whether to use SSL

### Languages

`default-language` - The default language for players; must have a file that matches (ex: `messages_en.yml` for `en`)

`try-detect-client-language` - If set to `true`, the plugin will try to use the client's language, if available and valid. This is only for players who have not set a language using commands, or if their language was reset after a server restart. If the client language is not a valid plugin language, it will use the `default-language`. If set to `false`, all unset players will use the `default-language`.

`languages` - A list of languages players can switch to using `/skills lang <language>`; must also have a file that matches. Custom language files are defined here.

### Hooks

`hooks:`

Options for each hook are under the section with the plugin name.

* `enabled` - Whether the hook to the given plugin should be registered. Hooks will only attempt to be loaded if the given plugin is detected and enabled, so hooks can be set to `true` without having the plugin on the server. Some hooks, like HolographicDisplays and DecentHolograms perform the same function, so one must be disabled before the other can be enabled.

#### WorldGuard

`WorldGuard:`

* `blocked_regions` - Players in regions on this list will not be able to gain XP naturally in any skill.
* `blocked_check_replace_regions` - Regions on this list will disable block sources checking if the block broken has been player placed.

### Action Bar

`action-bar:`

* `enabled` - Whether the action bar should be enabled/disabled (Must be set to `true` to have any action bars; setting to `false` disables all action bar types)
* `idle` - Controls the idle action bar (not gaining xp). **Enable this if you want the action bar to always display.**
* `ability` - Controls the action bar for ability messages (raise/lower, activate, etc.). If set the false, the ability messages will be sent through chat instead. &#x20;
* `xp` - Controls the action bar for gaining xp (not maxed)
* `maxed` - Controls the action bar when xp is gained in a maxed skill
* `update-period` - How often the action bar should update, in ticks (Increase this value if action bar is causing lag)
* `round-xp` - If enabled, current xp will be rounded to an integer.
* `placeholder-api` - Whether PlaceholderAPI placeholders should be replaced in the action bar, given that you have PlaceholderAPI
* `use-suffix` - Whether to format the current player's XP with number suffixes (k, m, etc). Only applies if `xp` is set to true.

### Boss Bar

`boss-bar:`

* `enabled` - Whether boss bars should be enabled for xp gains
* `mode` - Can be either `single` or `multi`. `multi` means multiple boss bars will display if gaining XP from different types of skills, `single` is only one at a time.
* `stay-time` - How long the boss bar should stay up after not gaining xp, in ticks
* `update-every` - Controls how often the boss bar should update when gaining xp consecutively, increase if having lag issues
* `round-xp` - If enabled, the current xp will be rounded to an integer.
* `use-suffix` - Whether to format the current player's XP with number suffixes (k, m, etc).
* `format` - The format list allows you to change the boss bar color and style for each skill:
  * Format: '\[SKILL] \[COLOR] \[STYLE]'
  * Available colors are BLUE, GREEN, PINK, PURPLE, WHITE, RED, and YELLOW
  * Available styles are SOLID, SEGMENTED\_6, SEGMENTED\_10, SEGMENTED\_12, and SEGMENTED\_20

`base-mana` - The base amount of mana players should have at 0 Wisdom

`enable-roman-numerals` - Whether Roman numerals should be used for skill levels

### Damage Holograms

`damage-holograms` - Enable/disable damage holograms (requires HolographicDisplays)

`damage-holograms-scaling` - Whether the damage displayed on holograms should be scaled according to `health.hp-indicator-scaling`

`damage-holograms-decimal:`

* `display-when-less-than:` - Display decimals in damage holograms when less than a specified damage
* `decimal-max-amount` - The maximum amount of decimal digits to display

`damage-holograms-offset:`

* `x` - X coordinate offset
* `y` - Y coordinate offset
* `Z` - Z coordinate offset
* `random`:
  * `enabled` - Whether random hologram positions should be enabled
  * `x-min` - Minimum X coordinate offset
  * `x-max` - Maximum X coordinate offset
  * `y-min` - Minimum Y coordinate offset
  * `y-max` - Maximum Y coordinate offset
  * `z-min` - Minimum Z coordinate offset
  * `z-max` - Maximum Z coordinate offset

### Leaderboards

`leaderboards:`

* `update-period` - How often leaderboards should be updated, in ticks
* `update-delay` - How long after server startup should the leaderboards be updated, in ticks (does not include the immediate update on startup)



`enable-skill-commands` - Whether skill name commands should be enabled such as `/farming` or `/mining` (Requires restart to have an effect)

`check-block-replace` - Whether blocks placed by players should not give xp; keep `true` unless you are having plugin compatibility issues

### Worlds and Regions

`blocked-check-block-replace-worlds` - Worlds on this list will not be checked for block replaces, allowing placing and breaking of blocks to gain xp.

`blocked-worlds` - Players in worlds on this list will not be able to gain xp naturally in any skill.

`disabled-worlds` - Most of the plugin's gameplay functionality will be disabled in worlds on this list, including but not limited to stats, abilities, gaining xp, and the action bar (commands and menus will still be available)

`disable-in-creative-mode` - Whether players should not be able to gain xp while in creative mode

### Auto-save

`auto-save:`

* `enabled` - Whether data for online players should save periodically instead of just when they log out. This is useful if you experience skill data losses due to server crashes.
* `interval-ticks` - How often (in ticks) to auto-save

### Economy

`skill-money-rewards:`

* `enabled` - Whether players should gain money for leveling up skills (requires Vault)
* `base` - The base amount of money players gain at level 2
* `multiplier` - The multiplier (`money = base + multiplier * level * level`)

### Leveler

`leveler:`

* `title:`
  * `enabled` - Whether a title should be displayed to players on skill level up
  * `fade-in` - Title fade in time, in ticks
  * `stay` - How long the title should last, in ticks
  * `fade-out` - Title fade out time, in ticks
* `sound:`
  * `enabled` - Whether a sound should be played to players on skill level up
  * `type` - The name of the sound that should be played (must be a valid sound name)
  * `category` - The sound category the sound should be played in
  * `volume` - Sound volume
  * `pitch` - Sound pitch
* `double-check-delay` - The level up check delay for large xp gains at once, in ticks (lower is faster)

### Modifier

`modifier:`

* `armor:`
  * `equip-blocked-materials` - A list of blocks that should not grant stats of armor when right-clicked; add to this list when stats are given but armor is not equipped.
* `item:`
  * `check-period` - How often, in ticks, the item held in a player's hand should be checked for stat item modifiers (increase if you have lag)
  * `enable-off-hand` - Whether stat modifiers should work in the off hand
* `auto-convert-from-legacy` - Whether the old modifier nbt format should be converted to the new one. Set to false if you are having performance issues and all your items have been converted.

### Requirement

`requirement:`

* `enabled` - Whether requirements should be checked at all. If you do not use requirements, disabling will improve performance.
* `item:`
  * `prevent-tool-use` - Whether block breaking should be blocked when a player does not meet a requirement
  * `prevent-weapon-use` - Whether attacking entities should be blocked when a player does not meet a requirement
  * `prevent-block-place` - Whether block placing should be blocked when a player does not meet a requirement
  * `prevent-interact` - Whether interacting (right clicking) should be blocked when a player does not meet a requirement
  * `global` - Define item requirements that should apply to every item of that type. Format: - '\[material] \[skill\_1]:\[level\_1] \[skill\_2]:\[level\_2] ...'
* `armor:`
  * `prevent-armor-equip` - Whether armor should be unable to be equipped when a player does not meet a requirement
  * `global` - Define armor requirements that should apply to every item of that type. Format: - '\[material] \[skill\_1]:\[level\_1] \[skill\_2]:\[level\_2] ...'

### Critical

`critical:`

* `base-multiplier` - The base damage multiplier for critical hits
* `enabled` - Options in this category control whether that item type should be able to deal critical hits. (`hand` is for empty fist, `other` is for holding any other item not on the list)

### Menus

`menus:`

* `placeholder-api` - Whether PlaceholderAPI placeholders should be used in menus.

### Miscellaneous

`check-for-updates` - Whether the plugin should check for new updates on startup and when a player with the `aureliumskills.checkupdates` permission joins

`automatic-backups:`

* `enabled` - Whether automatic backups should be taken on server shutdown
* `minimum-interval-hours` - The minimum interval, in hours, between automatic backups. Automatic backups will only be taken at least this amount of hours after the last one.

`save-blank-profiles` - If false, player data of players who have not leveled any skills or gained any XP will not be saved into storage.
