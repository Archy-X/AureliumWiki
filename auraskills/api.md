---
description: Guide to the AuraSkills API
---

# API

The AuraSkills API allows developers to interact with the plugin, integrate with an existing plugin, or register content (custom skills, stats, abilities).

The API consists of just the `api` and `api-bukkit` sub-modules. This allows API methods to be stable across versions and prevents the use of unstable internal methods.

Javadocs are available [here](https://docs.aurelium.dev/auraskills-api-bukkit/).

## Getting Started

Release versions are published to the Maven central repository, while versions ending in -SNAPSHOT are published to the Sonatype Nexus Snapshot repository.

### Maven

{% code title="pom.xml" %}
```xml
<repository>
    <id>sonatype</id>
    <url>https://s01.oss.sonatype.org/content/repositories/snapshots/</url>
</repository>

<dependency>
    <groupId>dev.aurelium</groupId>
    <artifactId>auraskills-api-bukkit</artifactId>
    <version>2.0.0-SNAPSHOT</version>
</dependency>
```
{% endcode %}

### Gradle

Groovy DSL:

{% code title="build.gradle" %}
```gradle
repositories {
     mavenCentral()
     maven { url 'https://s01.oss.sonatype.org/content/repositories/snapshots/' }
}

dependencies {
     compileOnly 'dev.aurelium:auraskills-api-bukkit:2.0.0-SNAPSHOT'
}
```
{% endcode %}

Kotlin DSL:

{% code title="build.gradle.kts" %}
```gradle
repositories {
     mavenCentral()
     maven("https://s01.oss.sonatype.org/content/repositories/snapshots/")
}

dependencies {
     compileOnly("dev.aurelium:auraskills-api-bukkit:2.0.0-SNAPSHOT")
}
```
{% endcode %}

## Getting the API instance

Most API classes are accessible through the AuraSkillsApi interface accessed using the following static method.

```java
AuraSkillsApi auraSkills = AuraSkillsApi.get();
```

In very limited situations where the API requires Bukkit classes, such as the ItemManager and Regions API, you will have to obtain an instance of the AuraSkillsBukkit interface:

```java
AuraSkillsBukkit auraSkillsBukkit = AuraSkillsBukkit.get();
```

{% hint style="warning" %}
Any use of the `auraSkills` variable name below refers to the `AuraSkillsApi` instance.
{% endhint %}

## Interacting with players

Player skill information is available through the `SkillsUser` interface, which is obtained from the API instance:

```java
SkillsUser user = auraSkills.getUser(player.getUniqueId());
```

While the method accepts a uuid, only online players will have their skill data loaded. Offline players will still return a `SkillsUser` object, but will have all values set to default ones and modifying values will not work.

### Skills

You can get and set a user's skill level using the user methods and the `Skills` enum for default skills. For example:

```java
// Gets the user's Farming skill level. Use the Skills enum for all default skills.
int level = user.getSkillLevel(Skills.FARMING);

// Set the Fighting skill to level 10
user.setSkillLevel(Skills.FIGHTING, 10); 
```

Getting and adding skill XP is very similar:

```java
double xp = user.getSkillXp(Skills.FARMING);

user.addSkillXp(Skills.FARMING, 20.0);

user.addSkillXpRaw(Skills.FARMING, 15.0); // Ignores any XP multipliers

// Sets XP to 0, resetting progress for only the current skill level
user.setSkillXp(Skills.FARMING, 0.0);
```

### Stats

Getting a player's stat level is simple using the method and the `Stats` enum.

```java
// Gets the user's strength stat level. Use the Stats enum for all default stats.
double level = user.getStatLevel(Stats.STRENGTH);

// Gets the stat level only from permanent skill rewards (without modifiers).
double baseLevel = user.getBaseStatLevel(Stats.HEALTH);
```

Adding stat levels is more complex, since stat levels aren't stored directly. Instead, there a system of stat modifiers which associates a unique name to a stat increase so it can be tracked and removed later.

```java
// Adding a Wisdom stat modifier of value 10. The String argument should be a unique
// name that identifies the purpose of the modifier.
user.addStatModifier(new StatModifier("my_modifier_name", Stats.WISDOM, 10.0));

// Remove the modifier with just the name
user.removeStatModifier("my_modifier_name");
```

### Mana

Getting and setting mana is very simple:

```java
double mana = user.getMana();

// Check that the user has enough mana before removing it
if (mana >= 10.0) {
    user.setMana(mana - 10.0);
} else {
    // Handle not enough mana case
}

// Gets the user's max mana (determined by the Wisdom stat)
double maxMana = user.getMaxMana();
```

## Events

The API has many events to interact with the plugin. These are registered just like regular Bukkit events.

### List

The following is a list of available events. This may not always be up-to-date, so see the source code for all the events.

* `LootDropEvent` - Calls when the plugin drops/modifies item loot. Includes both multiplier abilities and Fishing/Excavation loot tables. Use `getCause()` to get the reason for the drop.
* `SkillsLoadEvent` - Calls when the plugin has fully finished loading, usually on the first tick of the server. Use this event when accessing anything related to skills, stats, etc on server startup instead of in `onEnable`, since skills won't be loaded yet then.
* `ManaAbilityActivateEvent` - Calls when a player activates a mana ability.
* `ManaAbilityRefreshEvent` - Calls when a player is able to use a mana ability again (cooldown reaches 0)
* `ManaRegenerateEvent` - Calls when a player regenerates mana naturally.
* `SkillLevelUpEvent` - Calls when a player levels up a skill.
* `XpGainEvent` - Calls when a player gains skill XP.
* `CustomRegenEvent` - Calls when a player regenerates health if custom regen mechanics are enabled.

## Custom content

To register custom content, you must first obtain a NamespacedRegistry that identifies your plugin and the directory from which to load content files (including skills.yml, rewards, sources, etc).

```java
AuraSkillsApi auraSkills = AuraSkillsApi.get(); // Get the API instance

// Getting the NamespacedRegistry with your plugin name and the plugin's folder
NamespacedRegistry registry = auraSkills.useRegistry("pluginnanme", getDataFolder());
```

The first argument of `useRegistry` is the name of your plugin registering the custom content (it will be forced to lowercase). This is the namespace part of all content that is identified by a NamespacedId, and is used in config files like skills.yml, abilities.yml, etc.

### Content directory

The second argument of `useRegistry` is a Java File object representing the content directory. Some things like sources and rewards cannot be registered through code alone and must be done through config files that are automatically detected and loaded from the content directory. A good choice is just the plugin's data folder, which is accessed from `Plugin#getDataFolder()`.

The format for content files is the same as the main AuraSkills plugin, as your content files are merged together with the main plugin's config files when loading skills, stats, and abilities. For example, you can reference `auraskills/` namespaced abilities in your skills.yml file, and `yourplugin/` namespaced abilities can be used in the AuraSkills skills.yml file.

### Stats

To create a custom stat, use the `CustomStat.builder` static method to get an instance of  `CustomStatBuilder`. You must pass in a `NamespacedId` using `NamespacedId.of` to identify the stat. Then register the stat with the `NamespacedRegistry`.

Stats on their own don't represent any game mechanics or attributes. Every stat must also have at least one trait that actually implements functionality. Use the `CustomTrait.builder` to get a `CustomTraitBuilder` to create a trait. This `CustomTrait` is then passed into `CustomStatBuilder#trait` to link it to the stat. You can also pass in a default trait from the `Traits` enum if you want to simply split up stats.

The example below shows creating a new dexterity stat with a dodge chance trait adding functionality to evade attacks. We create new classes and assign the trait and stat to static constants for easier access.

```java
public class CustomTraits {
    
    public static final CustomTrait DODGE_CHANCE = CustomTrait
            .builder(NamespacedId.of("pluginname", "dodge_chance")
            .displayName("Dodge Chance")
            .build();
        
}

public class CustomStats {

    public static final CustomStat DEXTERITY = CustomStat
            .builder(NamespacedId.of("pluginname", "dexterity")
            .trait(CustomTraits.DODGE_CHANCE, 0.5) // Dodge chance will increase by 0.5 per dexterity level
            .displayName("Dexterity")
            .description("Dexterity increases the chance to dodge attacks.")
            .color("<green>")
            .symbol("")
            .item(ItemContext.builder()
                    .material("lime_stained_glass_pane")
                    .group("lower") // A group defined in AuraSkills/menus/stats.yml
                    .order(2) // The position within that group
                    .build())
            .build();

}
```

You must then register the CustomTrait and CustomStat in your plugin's onEnable:

```java
AuraSkillsApi auraSkills = AuraSkillsApi.get();

NamespacedRegistry registry = auraSkills.useRegistry("pluginnanme", getDataFolder());

registry.registerTrait(CustomTraits.DODGE_CHANCE);
registry.registerStat(CustomStats.DEXTERITY);
```

{% hint style="warning" %}
Replace "pluginname" with the name of your plugin in lowercase. The name passed into `NamespacedId.of` must match the one passed in `AuraSkillsApi#useRegistry`.
{% endhint %}

#### Trait handlers

The above code only registers the existence and display elements of the stat without any gameplay functionality. If your stat only uses existing traits, this is all you need. However, if are creating a new trait like the example above, you must add additional code to actually implement your trait's functionality.

To implement your trait functionality, create a new class that implements `BukkitTraitHandler`:

```java
public class DodgeChanceTrait implements BukkitTraitHandler, Listener {

    private final AuraSkillsApi auraSkills;
    
    // Inject API dependency in constructor
    public DodgeChanceTrait(AuraSkillsApi auraSkills) {
        this.auraSkills = auraSkills;
    }

    @Override
    public Trait[] getTraits() {
        // Aan array containing your CustomTrait instance
        return new Trait[] {CustomTraits.DODGE_CHANCE};
    }
    
    @Override
    public double getBaseLevel(Player player, Trait trait) {
        // The base value of your trait when its stat is at level 0, could be a
        // Minecraft default value or values from other plugins
        return 0;
    }
    
    @Override
    public void onReload(Player player, SkillsUser user, Trait trait) {
        // Method called when the value of the trait's parent stat changes
    }
    
    // Example implementation of the trait's functionality (not complete)
    @EventHandler(ignoreCancelled = true)
    public void onAttack(EntityDamageByEntityEvent event) {
        if (!(event.getEntity() instanceOf Player)) return;
        
        Player player = event.getEntity();
        SkillsUser user = auraSkills.getUser(player.getUniqueId());
        
        // Gets the user's trait level
        double dodgeChance = user.getEffectiveTraitLevel(Traits.DODGE_CHANCE);
        
        if (ThreadLocalRandom.current().nextDouble() < dodgeChance) {
            // Dodge activated
            event.setCancelled(true);
            player.sendMessage("Dodged attack");
        }
    }

}
```

Finally, register your `BukkitTraitHandler` in onEnable:

```java
AuraSkillsApi auraSkills = AuraSkillsApi.get();

auraSkills.getHandlers().registerTraitHandler(new DodgeChanceTrait(auraSkills));
```

{% hint style="info" %}
Calling registerTraitHandler automatically registers any Bukkit events if the class implements Listener.
{% endhint %}

#### Configuration

To make your stat and trait configurable by users of your plugin, create a `stats.yml` file in your plugin's [content directory](api.md#content-directory) that you linked to the `NamespacedRegistry` used to register your traits/stats.

Here is an example `stats.yml` for the Dexterity/Dodge Chance example above:

```yaml
stats:
  pluginnamne/dexterity:
    enabled: true
    traits:
      pluginname/dodge_chance:
        modifier: 0.5 # Overrides the value passed in with CustomStatBuilder#trait
traits:
  pluginname/dodge_chance:
    enabled: true
    # Add configurable options here accessed using Trait#option... methods  
```

{% hint style="warning" %}
You are responsible for generating the `stats.yml` file in the user's folder using `Plugin#saveResource`.
{% endhint %}

### Skills

To create a custom skill, get an instance of the builder using `CustomSkill.builder` and pass in a `NamespacedId` using `NamespacedId.of`. Then register the skill with the `NamspacedRegistry`.

The following is an example for creating a Trading skill that gives XP when trading with villagers. We create a new `CustomSkills` class with a static constant to hold the reference to the skill instance for easier access.

```java
public class CustomSkills {

    public static final CustomSkill TRADING = CustomSkill
            .builder(NamespacedId.of("pluginname", "trading")
            .displayName("Trading")
            .description("Trade with villagers to gain Trading XP")
            .item(ItemContext.builder()
                    .material("emerald")
                    .pos("4,4")
                    .build())
            .build();
    
}
```

Then, register the skill in your plugin's onEnable:

```java
AuraSkillsApi auraSkills = AuraSkillsApi.get();

NamespacedRegistry registry = auraSkills.useRegistry("pluginnanme", getDataFolder());

registry.registerSkill(CustomSkills.TRADING);
```

#### Adding XP sources

To add ways to gain XP for a custom skill, create a sources file with the same format as the AuraSkills sources files. For our example, create a file at `sources/trading.yml`.

If your custom skill's XP sources use existing source types available in default skills (such as breaking blocks, killing a mob, tracking a statistic, etc), then you just need to add configured sources of the existing types to the file. See [Sources](sources.md) for how to add existing source types.

In our case, there is no existing source type that handles trading with villagers, so we will need to create our own using the API.

First, create a class that extends `CustomSource` to hold the configurable data of your source. In this case for simplicty, our only parameter will be the number of emeralds transacted in the trade, so we just need to add a configurable multiplier for this value. Keep the SourceValues constructor argument in your constructor and add your parameters to the end of it.

```java
public class TradingSource extends CustomSource {

    private final double multiplier;
    
    public TradingSource(SourceValues values, double multiplier) {
        super(values);
        this.multiplier = multiplier;
    }
    
    public double getMultiplier() {
        return multiplier;
    }

}
```

Then, you need to register your source type and create a parser to deserialize the source from configuration. Use `NamespacedRegistry#registerSourceType` to register the name and parser of your source.&#x20;

The first argument, name, is a lowercase name for your source used to construct it's NamspacedId used as the `type` key in the sources config. If you pass in `trading` as the name, you would use `type: pluginname/trading` in the sources config.

The second argument accepts an instance of `XpSourceParser` where the type parameter is your `CustomSource` class. You can create a new class, or use a lambda like in this example:

```java
SourceType trading = registry.registerSourceType("trading", (XpSourceParser<TradingSource>) (source, context) -> {
    double multiplier = source.node("multiplier").getDouble(1.0);
    return new TradingSource(context.parseValues(source), multiplier);
}
```

The `source` argument of the lambda is a `ConfigurationNode` from [Configurate](https://github.com/SpongePowered/Configurate/wiki/Node) that contains the keys and values from the configuration section of the source to load. The `context` argument is a `SourceContext` that contains useful parsing methods for enforcing required keys or getting pluralized values. You also need to use `context.parseValues(source)` to get the `SourceValues` object to pass as the first argument of your source type constructor. This parses things like the name, xp, and display\_name of the source for you, so you only need to implement parsing of your custom options.

To make your source actually give XP, you need to implement a leveler that listens to the proper Bukkit events or implements some other mechanism for gaining XP. You can create an instance of the `LevelerContext` class to help you create a leveler. It contains useful methods like checking blocked locations and players. To create an instance, you need to pass an instance of the API and the `SourceType` returned when you registered it.

After you register the source type, you still have to create a sources file for your skill to actually define the XP source. In this example, we create a `sources/trading.yml` file:

```yaml
sources:
  trade:
    type: pluginname/trading
    multiplier: 5
```

{% hint style="warning" %}
You are responsible for generating the sources file in the user's folder using `Plugin#saveResource`.
{% endhint %}

#### Adding rewards

Rewards are added by creating a rewards file in the same format as the AuraSkills rewards files. For this example, create a file at `rewards/trading.yml`. We make Trading give the Luck stat every skill level and the Dexterity stat created above every two levels:

```yaml
patterns:
  - type: stat
    stat: luck
    value: 1
    pattern:
      interval: 1
  - type: stat
    stat: pluginname/dexterity
    value: 1
    pattern:
      interval: 2
```

### Abilities

### Mana Abilities
