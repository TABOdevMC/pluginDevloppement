# Guide Complet - Développement d'un Plugin Minecraft Paper avec Maven

> Ce tutoriel est destiné aux développeurs souhaitant créer un plugin **Paper** moderne en Java.
>
> **Version recommandée :** Paper 1.21+
>
> Technologies utilisées :
>
> - Java 21
> - Maven
> - API Paper
> - Brigadier (Command API de Paper)
> - Adventure API
> - Events
> - Scheduler
> - Configuration YAML

---

# Sommaire

1. Création du projet Maven
2. Structure du projet
3. Configuration Maven
4. plugin.yml
5. Classe principale
6. Les Events
7. Les commandes
8. Scheduler
9. Configuration YAML
10. Adventure API
11. Les joueurs
12. Les inventaires
13. Les items
14. Les mondes
15. Les entités
16. Les particules
17. Les sons
18. Les BossBars
19. Les Scoreboards
20. Les Permissions
21. PersistentDataContainer
22. Imports utiles
23. Bonnes pratiques

---

# 1. Création du projet

Créer un projet Maven.

Arborescence recommandée :

```
MonPlugin/
│
├── src/
│   └── main/
│       ├── java/
│       │   └── fr/
│       │       └── monplugin/
│       │           ├── Main.java
│       │           ├── commands/
│       │           ├── listeners/
│       │           ├── managers/
│       │           ├── services/
│       │           └── utils/
│       │
│       └── resources/
│           └── plugin.yml
│
├── pom.xml
│
└── .gitignore
```

---

# 2. Configuration Maven

## pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>fr.monplugin</groupId>
    <artifactId>MonPlugin</artifactId>
    <version>1.0</version>

    <properties>

        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>

        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>

    </properties>

    <repositories>

        <repository>

            <id>papermc</id>

            <url>https://repo.papermc.io/repository/maven-public/</url>

        </repository>

    </repositories>

    <dependencies>

        <dependency>

            <groupId>io.papermc.paper</groupId>

            <artifactId>paper-api</artifactId>

            <version>1.21.1-R0.1-SNAPSHOT</version>

            <scope>provided</scope>

        </dependency>

    </dependencies>

</project>
```

---

# 3. plugin.yml

Créer un fichier :

```
src/main/resources/plugin.yml
```

Contenu :

```yaml
name: MonPlugin
version: 1.0
main: fr.monplugin.Main

api-version: '1.21'
```

---

# 4. Classe principale

```java
public final class Main extends JavaPlugin {

    @Override
    public void onEnable() {

    }

    @Override
    public void onDisable() {

    }

}
```

---

# Singleton du plugin

Très pratique pour accéder au plugin partout.

```java
public class Main extends JavaPlugin {

    private static Main instance;

    @Override
    public void onEnable() {

        instance = this;

    }

    public static Main get() {

        return instance;

    }

}
```

Utilisation :

```java
Main.get();
```

---

# Logger

```java
getLogger().info("Plugin démarré !");
```

---

# Sauvegarder la configuration

```java
saveDefaultConfig();
```

---

# 5. Les Events

Créer une classe :

```java
public class PlayerListener implements Listener {

}
```

Puis enregistrer :

```java
@Override
public void onEnable() {

    Bukkit.getPluginManager().registerEvents(

            new PlayerListener(),

            this

    );

}
```

---

# PlayerJoinEvent

```java
@EventHandler
public void onJoin(PlayerJoinEvent event){

    Player player = event.getPlayer();

}
```

---

# PlayerQuitEvent

```java
@EventHandler
public void onQuit(PlayerQuitEvent event){

}
```

---

# PlayerInteractEvent

```java
@EventHandler
public void onInteract(PlayerInteractEvent event){

    Player player = event.getPlayer();

    if(event.getAction() == Action.RIGHT_CLICK_BLOCK){

    }

}
```

---

# BlockBreakEvent

```java
@EventHandler
public void onBreak(BlockBreakEvent event){

    Block block = event.getBlock();

}
```

---

# BlockPlaceEvent

```java
@EventHandler
public void onPlace(BlockPlaceEvent event){

}
```

---

# EntityDamageEvent

```java
@EventHandler
public void onDamage(EntityDamageEvent event){

}
```

---

# EntityDamageByEntityEvent

```java
@EventHandler
public void onDamage(EntityDamageByEntityEvent event){

    if(event.getDamager() instanceof Player player){

    }

}
```

---

# Annuler un Event

```java
event.setCancelled(true);
```

---

# 6. Les commandes

> **À partir de Paper 1.21, il est recommandé d'utiliser la Command API basée sur Brigadier.**

Exemple simple :

```java
Commands.literal("heal")

    .requires(source -> source.getSender().hasPermission("heal.use"))

    .executes(context -> {

        Player player = (Player) context.getSource().getSender();

        player.setHealth(20);

        return Command.SINGLE_SUCCESS;

    });
```

---

# Suggestions

```java
.suggests((context, builder) -> {

    builder.suggest("survival");
    builder.suggest("creative");

    return builder.buildFuture();

})
```

---

# Arguments

```java
Commands.argument("player", EntityArgument.player())
```

```java
Commands.argument("amount", IntegerArgumentType.integer())
```

```java
Commands.argument("message", StringArgumentType.greedyString())
```

---

# Permissions

```java
.requires(source -> source.getSender().hasPermission("admin.use"))
```

---

# 7. Scheduler

Exécuter plus tard :

```java
Bukkit.getScheduler().runTaskLater(

        Main.get(),

        () -> {

        },

        20L

);
```

---

Toutes les secondes :

```java
Bukkit.getScheduler().runTaskTimer(

        Main.get(),

        () -> {

        },

        0L,

        20L

);
```

---

Async

```java
Bukkit.getScheduler().runTaskAsynchronously(

        Main.get(),

        () -> {

        }

);
```

---

# 8. Configuration YAML

Lire

```java
String name = getConfig().getString("name");
```

```java
int coins = getConfig().getInt("coins");
```

```java
boolean enabled = getConfig().getBoolean("enabled");
```

---

Modifier

```java
getConfig().set("coins", 150);

saveConfig();
```

---

# 9. Adventure API

Message

```java
player.sendMessage(Component.text("Bonjour"));
```

---

Couleur

```java
Component.text("Hello")

        .color(NamedTextColor.GREEN);
```

---

MiniMessage

```java
MiniMessage.miniMessage().deserialize(

        "<green>Hello <yellow>World"

);
```

---

# 10. Les Joueurs

Nom

```java
player.getName();
```

UUID

```java
player.getUniqueId();
```

Vie

```java
player.setHealth(20);
```

Nourriture

```java
player.setFoodLevel(20);
```

XP

```java
player.giveExp(500);
```

Téléportation

```java
player.teleport(location);
```

Gamemode

```java
player.setGameMode(GameMode.CREATIVE);
```

---

# 11. Inventaire

Récupérer

```java
Inventory inventory = player.getInventory();
```

Ajouter un item

```java
inventory.addItem(item);
```

Vider

```java
inventory.clear();
```

---

Créer un GUI

```java
Inventory gui = Bukkit.createInventory(

        null,

        27,

        Component.text("Menu")

);
```

Ouvrir

```java
player.openInventory(gui);
```

---

# 12. Les Items

Créer

```java
ItemStack sword = new ItemStack(Material.DIAMOND_SWORD);
```

---

Modifier

```java
ItemMeta meta = sword.getItemMeta();

meta.displayName(

        Component.text("Épée Légendaire")

);

sword.setItemMeta(meta);
```

---

Enchantement

```java
meta.addEnchant(

        Enchantment.SHARPNESS,

        5,

        true

);
```

---

Lore

```java
meta.lore(List.of(

        Component.text("Très puissante"),

        Component.text("Unique")

));
```

---

# 13. Les Mondes

Récupérer

```java
World world = Bukkit.getWorld("world");
```

Temps

```java
world.setTime(6000);
```

Météo

```java
world.setStorm(true);
```

Spawn

```java
Location spawn = world.getSpawnLocation();
```

---

# 14. Les Entités

Spawn

```java
Zombie zombie = world.spawn(

        location,

        Zombie.class

);
```

Nom

```java
zombie.customName(

        Component.text("Boss")

);
```

IA

```java
zombie.setAI(false);
```

---

# 15. Les Particules

```java
player.spawnParticle(

        Particle.FLAME,

        player.getLocation(),

        100

);
```

---

# 16. Les Sons

```java
player.playSound(

        player,

        Sound.ENTITY_PLAYER_LEVELUP,

        1f,

        1f

);
```

---

# 17. BossBar

Créer

```java
BossBar bossBar = BossBar.bossBar(

        Component.text("Boss"),

        1f,

        BossBar.Color.RED,

        BossBar.Overlay.PROGRESS

);
```

Afficher

```java
player.showBossBar(bossBar);
```

---

# 18. Scoreboard

```java
ScoreboardManager manager = Bukkit.getScoreboardManager();

Scoreboard scoreboard = manager.getNewScoreboard();
```

---

# 19. Permissions

```java
player.hasPermission("admin.use");
```

---

# 20. PersistentDataContainer

Créer une clé

```java
NamespacedKey key = new NamespacedKey(

        Main.get(),

        "coins"

);
```

Sauvegarder

```java
container.set(

        key,

        PersistentDataType.INTEGER,

        100

);
```

Lire

```java
Integer coins = container.get(

        key,

        PersistentDataType.INTEGER

);
```

---

# 21. Les imports utiles

## Bukkit

```java
import org.bukkit.Bukkit;
import org.bukkit.Location;
import org.bukkit.World;
import org.bukkit.Material;
import org.bukkit.Sound;
import org.bukkit.Particle;
import org.bukkit.GameMode;
```

---

## Joueurs

```java
import org.bukkit.entity.Player;
```

---

## Entités

```java
import org.bukkit.entity.*;
```

---

## Inventaires

```java
import org.bukkit.inventory.Inventory;
import org.bukkit.inventory.ItemStack;
import org.bukkit.inventory.meta.ItemMeta;
```

---

## Events

```java
import org.bukkit.event.Listener;
import org.bukkit.event.EventHandler;

import org.bukkit.event.player.*;
import org.bukkit.event.block.*;
import org.bukkit.event.entity.*;
import org.bukkit.event.inventory.*;
```

---

## Adventure

```java
import net.kyori.adventure.text.Component;

import net.kyori.adventure.text.format.NamedTextColor;

import net.kyori.adventure.text.minimessage.MiniMessage;

import net.kyori.adventure.bossbar.BossBar;
```

---

## Configuration

```java
import org.bukkit.configuration.file.FileConfiguration;
```

---

## Scheduler

```java
import org.bukkit.scheduler.BukkitRunnable;
```

---

## PersistentData

```java
import org.bukkit.NamespacedKey;

import org.bukkit.persistence.PersistentDataContainer;

import org.bukkit.persistence.PersistentDataType;
```

---

# 22. Bonnes pratiques

- Utiliser **Java 21** avec Paper.
- Préférer la **Command API Paper (Brigadier)** aux anciennes commandes Bukkit.
- Utiliser **Adventure API** au lieu de `ChatColor`.
- Éviter les traitements lourds sur le thread principal.
- Toujours vérifier les valeurs `null`.
- Organiser le projet par packages (`commands`, `listeners`, `services`, `utils`, `managers`...).
- Stocker les données persistantes avec `PersistentDataContainer`.
- Limiter les accès répétés à la configuration en mettant les valeurs importantes en cache.
- Utiliser les permissions plutôt que des vérifications sur le pseudo d'un joueur.
- Écrire des classes courtes avec une responsabilité unique.

---

# Conclusion

Vous disposez maintenant d'une base solide pour développer un plugin **Paper** avec **Maven**.

Les principaux concepts abordés sont :

- ✅ Projet Maven
- ✅ Configuration `pom.xml`
- ✅ `plugin.yml`
- ✅ Classe principale
- ✅ Events
- ✅ Scheduler
- ✅ Configurations YAML
- ✅ Adventure API
- ✅ Joueurs
- ✅ Inventaires
- ✅ Items personnalisés
- ✅ Entités
- ✅ Mondes
- ✅ Particules
- ✅ Sons
- ✅ BossBars
- ✅ Scoreboards
- ✅ Permissions
- ✅ PersistentDataContainer
- ✅ Imports les plus utilisés

Cette base couvre la majorité des fonctionnalités utilisées dans les plugins Paper modernes et constitue un excellent point de départ avant d'aborder des sujets plus avancés comme les commandes Brigadier, les GUI complexes, les systèmes RPG ou les mini-jeux.
