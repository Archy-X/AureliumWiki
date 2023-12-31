---
description: How to migrate from AureliumSkills Beta 1.3 to AuraSkills 2.0
---

# Migration

{% hint style="danger" %}
Migration is currently experimental and not fully tested. Only test migration on a dev server and **not in production**.
{% endhint %}

With the 2.0 update, the plugin itself has been renamed from AureliumSkills to AuraSkills. This means that the name of the plugin folder is now `/AuraSkills` instead of `/AureliumSkills`.

While it is recommended to start with a clean config to get the many balancing changes in the recode, there is a system for migrating old config options to the new folder automatically. However, not everything will be able to be migrated, so manual work will still be required depending on how much you modified the configs.

If you want to migrate, keep your `/AureliumSkills` folder when running the new jar for the first time, so that when the files in `/AuraSkills` are generated, it can use the files in the old folder to migrate from.

{% hint style="danger" %}
Backup your `/AureliumSkills` folder and your database (if using MySQL for data storage) before migrating.
{% endhint %}

## Should you migrate?

If you are planning on starting a new season/world/server, you do not need to migrate. The new default config options more balanced and are recommended. Only migrate if you want to keep the abilities/stats/xp source values exactly the same as before. Messages and menus won't migrate (see below), so if you only changed those, you will still have to manually change them back.

## What will break

* Compatibilty with plugins that hooked into AureliumSkills before, since the plugin renaming means those plugins will no longer enable their hooks. There are many breaking API changes, so these hooks will have to be recoded anyway.
* Your previous permissions for the plugin set into plugins like LuckPerms won't work, since permission nodes have been renamed.

## What will not migrate

* Messages (significant format changes)
* Menus (significant format changes)
* PlaceholderAPI placeholders for %aureliumskills\_...% will still work for now, but you should change them to %auraskills\_...% eventually
* Custom XP sources added in the custom section of the old sources\_config.yml

## What will migrate automatically

* Most options in `config.yml` (skill-specific options have been moved to `skills.yml` and stat-specific options are in `stats.yml`)
* Rewards and loot tables (format mostly unchanged)
* Player data (now in `userdata` folder for YAML, new tables will be created for MySQL)
* XP values in `sources_config.yml` (moved to `sources` folder)
* `xp_requirements.yml` (format unchanged)
* `abilities_config.yml` (separated into `abilities.yml` and `mana_abilities.yml`)
* Item/armor modifiers when held
