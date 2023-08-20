---
description: Welcome to the AuraSkills wiki!
---

# AuraSkills

**AuraSkills** (formerly **Aurelium Skills**) is a Minecraft plugin that adds skills, stats, abilities, and other RPG-related features. It was developed for the Spigot and Paper server platforms and can be downloaded for free on SpigotMC, Hangar, Modrinth, and Polymart. The plugin is fully configurable and customizable, enabling use on a wide-range of server types from small SMPs to large, custom MMORPG networks.

This wiki contains documentation on how to set up, configure, and use the plugin. Support from the developer is provided on the Discord server, where users can also give suggestions, report bugs, and get announcements. AuraSkills is open-sourced on GitHub.

{% hint style="warning" %}
This wiki is for AuraSkills 2.0+ only. For the old wiki (AureliumSkills Beta 1.3.x), click [here](http://127.0.0.1:5000/o/-Mf1Cqap-T455k8cLLbf/s/-Mf1ApP15HhRtnWXpe0T/).
{% endhint %}

## Overview

Players level up skills by gaining skill XP through general Minecraft tasks, such as Farming, Mining, Fighting, or Enchanting. Increasing levels for each skill gives the player stat buffs, unlocks and levels up passive and mana abilities, and other customizable rewards. Using `/skills`, players can view all the relevant information about skills and gameplay in fully-configurable inventory GUI menus. Certain Fishing and Excavation abilities drop custom loot, which can be customized and extended to other skills through loot tables. Players can compete with each other through leaderboards and rankings. Custom items can also be created that give stat modifiers when held or worn, skill requirements to use, and XP multipliers. Numerous commands and permissions allow server admins to manage players and control access to features.

## Skills

There are 15 skills included in AuraSkills which level up as players gain skill XP through various XP sources. By default, each skill has two stats that increase every 1 or 2 skill levels. Most skills also have 5 passive abilities that unlock at levels 2-6 and level up every 5 skill levels. Some skills have a mana ability, which is a special ability that must be activated by the player, costs mana, and has a cooldown.

The 15 skills are Farming, Foraging, Mining, Fishing, Excavation, Archery, Defense, Fighting, Endurance, Agility, Enchanting, Alchemy, Sorcery, Healing, and Forging.

Skills can be viewed using `/skills` or by using the command for an individual skill, such as `/farming`, `/mining`, etc.

The `skills.yml` file is where skills are configured, including enabling/disabling skills, changing max levels and other skill-related options, as well as which abilities and mana abilities they have.

## Configuration

There a multiple configuration files in the `plugins/AuraSkills` directory that are used to configure the plugin.

### Main Config

> Main article: [Main Config](main-config.md)

The main `config.yml` file is used for general or miscellaneous config options related to storage/database, external plugin hooks, languages, action bar, boss bar, worlds/regions, modifiers, requirements, and more.

