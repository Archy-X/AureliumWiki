---
description: List of plugin commands
---

# Commands

Syntax: `<this>` indicates a required argument, `[this]` is an optional argument. See Permissions for more details about permissions.

{% hint style="info" %}
Any command below that start with `/sk` also works with the aliases `/skills` and `/skill`.
{% endhint %}

## Player Commands

The following commands are accessible to all players by default.

<table data-full-width="true"><thead><tr><th>Syntax</th><th>Description</th><th>Permission</th></tr></thead><tbody><tr><td><code>/skills</code></td><td>Opens the skills menu, which shows an overview and player progress in all skills.</td><td>auraskills.command.skills</td></tr><tr><td><code>/stats</code></td><td>Opens the stats menu, which shows an overview and player levels in all stats.</td><td>auraskills.command.stats</td></tr><tr><td><code>/sources [skill]</code></td><td>Opens the sources menu for a skill, which displays the ways to gain XP.</td><td>auraskills.command.sources</td></tr><tr><td><code>/[skill]</code></td><td>Opens the level progression menu of a specific skill directly, where <code>[skill]</code> is the name of the skill (e.g. <code>/farming</code>, <code>/archery</code>, <code>/healing</code>). Thse commands can be disabled in config.yml at  <code>enable_skill_commands</code></td><td>N/A</td></tr><tr><td><code>/sk lang [language]</code></td><td>Changes your personal language used for messages and menus. Must be a valid language code (see tab completion).</td><td>auraskills.command.lang</td></tr><tr><td><code>/sk abtoggle</code></td><td>Toggles your personal action bar.</td><td>auraskills.command.abtoggle</td></tr><tr><td><code>/sk top [page]</code> or <code>/skilltop [page]</code></td><td>Shows the leaderboard of top players in all skills (sum of skill levels). </td><td>auraskills.command.top</td></tr><tr><td><code>/sk top &#x3C;skill> [page]</code> or <code>/skilltop &#x3C;skill> [page]</code></td><td>Shows the leaderboard of top players in a specific skill.</td><td>auraskills.command.top</td></tr><tr><td><code>/sk top average [page]</code> or <code>/skilltop average [page]</code></td><td>Shows the leaderboard of the average of a player's skill levels.</td><td>auraskills.command.top</td></tr><tr><td><code>/sk rank</code> or <code>/skillrank</code></td><td>Shows your skill rankings.</td><td>auraskills.command.rank</td></tr><tr><td><code>/sk claimitems</code></td><td>Opens the menu to claim items (from item rewards or /sk item give) that did not fit into the player's inventory initially.</td><td>auraskills.command.claimitems</td></tr><tr><td><code>/sk help</code></td><td>Shows the command help page.</td><td>auraskills.command.help</td></tr></tbody></table>

## Admin Commands

The following commands are only accessible to players with op by default.



<table data-full-width="true"><thead><tr><th>Syntax</th><th>Description</th><th>Permission</th></tr></thead><tbody><tr><td><code>/sk reload</code></td><td>Reloads the plugin config, messages, menus, loot tables, etc. Some options may require a restart.</td><td>auraskills.command.reload</td></tr><tr><td><code>/sk skill setlevel &#x3C;player> &#x3C;skill> &#x3C;level></code></td><td>Sets a skill to a level for a player.</td><td>auraskills.command.skill.setlevel</td></tr><tr><td><code>/sk skill setall &#x3C;player> &#x3C;level></code></td><td>Sets all skills to a level for a player.</td><td>auraskills.command.skill.setlevel</td></tr><tr><td><code>/sk skill reset &#x3C;player> [skill]</code></td><td>Resets all skill to level 1 for a player, or a specific skill if <code>skill</code> is provided.</td><td>auraskills.command.skill.reset</td></tr><tr><td><code>/sk xp add &#x3C;player> &#x3C;skill> &#x3C;amount> [silent]</code></td><td>Gives skill XP to a player in a skill.</td><td>auraskills.command.xp.add</td></tr><tr><td><code>/sk xp set &#x3C;player> &#x3C;skill> &#x3C;amount> [silent]</code></td><td>Sets a player's skill XP to an amount in a skill.</td><td>auraskills.command.xp.set</td></tr><tr><td><code>/sk xp remove &#x3C;player> &#x3C;skill> &#x3C;amount> [silent]</code></td><td>Removes skill XP from a player in a skill. Does not decrease a player's skill level, only the XP progress of the current level.</td><td>auraskills.command.xp.remove</td></tr><tr><td><code>/sk multiplier [player]</code></td><td>Shows a player's current XP multiplier based on their cummulative auraskills.multiplier.* permissions.</td><td>auraskills.command.multiplier</td></tr><tr><td><code>/sk backup save</code></td><td>Saves a backup of the current skill data to the backups folder.</td><td>auraskills.command.backup.save</td></tr><tr><td><code>/sk backup load &#x3C;file></code></td><td>Loads a backup from the backups folder. You must specify the exact file name, including the file extension. This will override the current skill data, so it is advised to take a backup before loading one.</td><td>auraskills.command.backup.load</td></tr></tbody></table>

### Modifier Commands

`/sk modifier add <player> <stat> <name> <value> [silent] [stack]` - Adds a stat modifier to a player. Setting `silent` to true will not send a feedback message. Setting `stack` to true will add the stat modifier even if the `name` is already used by appending a number to the end.

`/sk modifier remove <player> <name> [silent]` - Removes a specific stat modifier from a player

`/sk modifier list [player] [stat]` - Lists all or a specific stat's modifiers for a player

`/sk modifier removeall [player] [stat] [silent]` - Removes all stat modifiers from a player

`/sk item modifier add <stat> <value> [lore]` - Adds an item stat modifier to the item held, along with lore by default

`/sk item modifier remove <stat> [lore]` - Removes an item stat modifier from the item held, and the lore associated with it by default

`/sk item modifier list` - Lists all item stat modifiers on the item held

`/sk item modifier removeall` - Removes all item stat modifiers from the item held

`/sk armor modifier add <stat> <value> [lore]` - Adds an armor stat modifier to the item held, along with lore by default

`/sk armor modifier remove <stat> [lore]` - Removes an armor stat modifier from the item held, and the lore associated with it by default

`/sk armor modifier list` - Lists all item armor modifiers on the item held

`/sk armor modifier removeall` - Removes all armor stat modifiers from the item held

### Requirement Commands

`/sk item requirement add <skill> <level> [lore]` - Adds an item requirement to the item held, along with lore by default

`/sk item requirement remove <skill> [lore]` - Removes an item requirement from the item held, and the lore associated with it by default

`/sk item requirement list` - Lists the item requirements on the item held

`/sk item requirement removeall` - Removes all item requirements from the item held

`/sk armor requirement add <skill> <level> [lore]` - Adds an armor requirement to the item held, along with lore by default

`/sk armor requirement remove <skill> [lore]` - Removes an armor requirement from the item held, and the lore associated with it by default

`/sk armor requirement list` - Lists the armor requirements on the item held

`/sk armor requirement removeall` - Removes all armor requirements from the item held

### Profile Commands

`/sk profile skills <player>` - Views the skill levels of another player. `player` is the username of the player you want to view skills for. This player does not have to be online.

`/sk profile stats <player>` - Views the stat levels of another player.

### Admin Commands

`/sk save` - Saves skill data

`/sk updateleaderboards` - Updates and sorts the leaderboards

`/sk transfer <playerFrom> <playerTo>` - Copies the player data from one player to another. `playerFrom` and `playerTo` must be valid UUIDs. The player data from `playerFrom` is not reset. Players do not have to be online for this to work.

`sk resethealth` - Removes all AureliumSkills player health and luck attributes. Useful if you are uninstalling the plugin. This command can only be run through console and there should be no players online to work properly.

### Mana Commands

`/mana [player]` - Shows how much mana you or another player has

`/mana add <player> <amount> [allowOverMax] [silent]` - Adds mana to a player

`/mana remove <player> <amount> [silent]` - Removes mana from a player. Mana cannot go below 0, any extra is ignored.

`/mana set <player> <amount> [allowOverMax] [silent]` - Sets the mana of a player.

### Item Registry Commands

`/sk item register <key>` - Registers the held item to a unique key

`/sk item unregsiter <key>` - Unregisters an item

`/sk item give <player> <key> [amount]` - Gives a registered item to a player. If the player does not have enough inventory space, the item will be added to the player's unclaimed items.
