# Guide Complet - Développement d'un Plugin Minecraft Paper

> Ce tutoriel est destiné aux développeurs souhaitant créer un plugin **Paper** moderne en Java.
>
> **Version recommandée :** Paper 1.21+
>
> Nous utiliserons :
>
> - Java 21
> - Gradle (ou Maven)
> - API Paper
> - Brigadier (CommandAPI de Paper)
> - Events
> - Scheduler
> - Configuration YAML
> - Components Adventure

---

# Sommaire

1. Création du projet
2. plugin.yml
3. Classe principale
4. Les commandes avec Brigadier
5. Les Events
6. Les Schedulers
7. Configurations
8. Adventure API
9. Les Players
10. Les Mondes
11. Les Inventaires
12. Les Items
13. Les Particules
14. Les Sons
15. Les Entités
16. Les Blocs
17. Les BossBars
18. Les Scoreboards
19. Les Permissions
20. Les Metadata / PersistentDataContainer
21. Les utilitaires indispensables
22. Les imports utiles à connaître

---

# 1. Création du projet

Arborescence :

```
src
 ├── main
 │    ├── java
 │    │      fr.monplugin
 │    │          Main.java
 │    │          commands/
 │    │          listeners/
 │    │          utils/
 │    └── resources
 │           plugin.yml
```

---

# build.gradle

```gradle
plugins {
    id 'java'
}

group = 'fr.monplugin'
version = '1.0'

repositories {
    mavenCentral()

    maven {
        url = "https://repo.papermc.io/repository/maven-public/"
    }
}

dependencies {
    compileOnly("io.papermc.paper:paper-api:1.21.1-R0.1-SNAPSHOT")
}

java {
    toolchain.languageVersion.set(JavaLanguageVersion.of(21))
}
```

---

# plugin.yml

```yaml
name: MonPlugin
version: 1.0
main: fr.monplugin.Main
api-version: '1.21'
```

---

# Classe principale

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

# Accéder au plugin partout

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

---

# 2. Les Events

Tous les listeners implémentent :

```java
public class PlayerListener implements Listener {

}
```

Puis :

```java
getServer().getPluginManager().registerEvents(new PlayerListener(), this);
```

---

# Event Join

```java
@EventHandler
public void onJoin(PlayerJoinEvent event){

    Player player = event.getPlayer();

}
```

---

# Event Quit

```java
@EventHandler
public void onQuit(PlayerQuitEvent event){

}
```

---

# Event Interact

```java
@EventHandler
public void onInteract(PlayerInteractEvent event){

    Player player = event.getPlayer();

    if(event.getAction() == Action.RIGHT_CLICK_BLOCK){

    }

}
```

---

# Event Block Break

```java
@EventHandler
public void onBreak(BlockBreakEvent event){

    Player player = event.getPlayer();

    Block block = event.getBlock();

}
```

---

# Event Place

```java
@EventHandler
public void onPlace(BlockPlaceEvent event){

}
```

---

# Event Damage

```java
@EventHandler
public void onDamage(EntityDamageEvent event){

}
```

---

# Event Damage Player

```java
@EventHandler
public void onDamage(EntityDamageByEntityEvent event){

    if(event.getDamager() instanceof Player player){

    }

}
```

---

# Annuler un event

```java
event.setCancelled(true);
```

---

# 3. Les commandes avec Brigadier

Paper possède un système moderne basé sur Brigadier.

En 1.21+, il est conseillé d'utiliser les commandes Paper natives plutôt que les anciennes méthodes `CommandExecutor`.

Exemple simplifié :

```java
public class ExampleCommands {

    public static LiteralCommandNode<CommandSourceStack> create() {

        return Commands.literal("heal")

                .requires(source -> source.getSender().hasPermission("heal.use"))

                .executes(context -> {

                    Player player = (Player) context.getSource().getSender();

                    player.setHealth(20);

                    return Command.SINGLE_SUCCESS;

                })

                .build();

    }

}
```

Puis enregistrer la commande lors de l'initialisation prévue par Paper.

---

# Arguments

```java
Commands.argument("joueur", EntityArgument.player())
```

```java
Commands.argument("nombre", IntegerArgumentType.integer())
```

```java
Commands.argument("texte", StringArgumentType.greedyString())
```

---

# Suggestions

```java
.suggests((context, builder) -> {

    builder.suggest("hello");
    builder.suggest("world");

    return builder.buildFuture();

})
```

---

# Permissions

```java
.requires(source -> source.getSender().hasPermission("admin.use"))
```

---

# 4. Scheduler

Exécuter plus tard

```java
Bukkit.getScheduler().runTaskLater(plugin, () -> {

},20L);
```

---

Toutes les secondes

```java
Bukkit.getScheduler().runTaskTimer(plugin, () -> {

},0L,20L);
```

---

Async

```java
Bukkit.getScheduler().runTaskAsynchronously(plugin,()->{

});
```

---

# 5. Configuration YAML

Créer

```java
saveDefaultConfig();
```

Lire

```java
String name = getConfig().getString("name");

int number = getConfig().getInt("coins");

boolean enabled = getConfig().getBoolean("enabled");
```

Modifier

```java
getConfig().set("coins",50);

saveConfig();
```

---

# 6. Adventure API

Message

```java
player.sendMessage(Component.text("Bonjour"));
```

Couleur

```java
Component.text("Hello")
.color(NamedTextColor.GREEN);
```

Gras

```java
.decorate(TextDecoration.BOLD);
```

---

# MiniMessage

```java
MiniMessage.miniMessage().deserialize(

"<green>Hello <red>World"

);
```

---

# 7. Joueurs

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

Food

```java
player.setFoodLevel(20);
```

XP

```java
player.giveExp(100);
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

# Inventaire

```java
player.getInventory();
```

Ajouter

```java
player.getInventory().addItem(item);
```

Clear

```java
player.getInventory().clear();
```

---

# 8. Items

Créer

```java
ItemStack sword = new ItemStack(Material.DIAMOND_SWORD);
```

Meta

```java
ItemMeta meta = sword.getItemMeta();

meta.displayName(Component.text("Epic Sword"));

sword.setItemMeta(meta);
```

Enchantement

```java
meta.addEnchant(
        Enchantment.SHARPNESS,
        5,
        true
);
```

---

# Lore

```java
meta.lore(List.of(

Component.text("Ligne 1"),

Component.text("Ligne 2")

));
```

---

# 9. Inventaires personnalisés

```java
Inventory inv = Bukkit.createInventory(

null,

27,

Component.text("Menu")

);
```

Ouvrir

```java
player.openInventory(inv);
```

Event

```java
InventoryClickEvent
```

---

# 10. Les Mondes

```java
World world = Bukkit.getWorld("world");
```

Spawn

```java
world.getSpawnLocation();
```

Changer météo

```java
world.setStorm(true);
```

Temps

```java
world.setTime(6000);
```

---

# 11. Particules

```java
player.spawnParticle(

Particle.FLAME,

player.getLocation(),

100

);
```

---

# 12. Sons

```java
player.playSound(

player,

Sound.ENTITY_PLAYER_LEVELUP,

1,

1

);
```

---

# 13. Les Entités

Spawn

```java
Zombie zombie = world.spawn(location, Zombie.class);
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

# 14. Les Blocs

```java
Block block = location.getBlock();
```

Changer

```java
block.setType(Material.DIAMOND_BLOCK);
```

---

# 15. BossBar

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

# 16. Scoreboard

```java
ScoreboardManager manager =
Bukkit.getScoreboardManager();
```

Créer

```java
Scoreboard scoreboard =
manager.getNewScoreboard();
```

---

# 17. Permissions

```java
player.hasPermission("admin.use");
```

---

# 18. PersistentDataContainer

Permet de stocker des données dans :

- Items
- Entités
- Joueurs

Créer une clé

```java
NamespacedKey key =
new NamespacedKey(plugin,"coins");
```

Sauvegarder

```java
container.set(

key,

PersistentDataType.INTEGER,

50

);
```

Lire

```java
container.get(

key,

PersistentDataType.INTEGER

);
```

---

# 19. Les utilitaires utiles

Obtenir un joueur

```java
Player player =
Bukkit.getPlayer("Pseudo");
```

Tous les joueurs

```java
Bukkit.getOnlinePlayers();
```

Broadcast

```java
Bukkit.broadcast(

Component.text("Hello")

);
```

Logger

```java
getLogger().info("Hello");
```

---

# 20. Les imports indispensables

## Bukkit

```java
import org.bukkit.Bukkit;
```

```java
import org.bukkit.Location;
```

```java
import org.bukkit.World;
```

```java
import org.bukkit.Material;
```

```java
import org.bukkit.Sound;
```

```java
import org.bukkit.Particle;
```

---

## Joueurs

```java
import org.bukkit.entity.Player;
```

```java
import org.bukkit.GameMode;
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
```

```java
import org.bukkit.inventory.ItemStack;
```

```java
import org.bukkit.inventory.meta.ItemMeta;
```

---

## Events

```java
import org.bukkit.event.Listener;
```

```java
import org.bukkit.event.EventHandler;
```

```java
import org.bukkit.event.player.*;
```

```java
import org.bukkit.event.block.*;
```

```java
import org.bukkit.event.entity.*;
```

```java
import org.bukkit.event.inventory.*;
```

---

## Components

```java
import net.kyori.adventure.text.Component;
```

```java
import net.kyori.adventure.text.format.NamedTextColor;
```

```java
import net.kyori.adventure.text.format.TextDecoration;
```

```java
import net.kyori.adventure.text.minimessage.MiniMessage;
```

---

## Scheduler

```java
import org.bukkit.scheduler.BukkitRunnable;
```

---

## BossBar

```java
import net.kyori.adventure.bossbar.BossBar;
```

---

## Configuration

```java
import org.bukkit.configuration.file.FileConfiguration;
```

---

## PersistentData

```java
import org.bukkit.persistence.PersistentDataContainer;
```

```java
import org.bukkit.persistence.PersistentDataType;
```

```java
import org.bukkit.NamespacedKey;
```

---

# Bonnes pratiques

- Utiliser Adventure au lieu des anciennes chaînes colorées (`ChatColor`).
- Éviter les tâches synchrones lourdes.
- Préférer `PersistentDataContainer` aux métadonnées temporaires.
- Toujours vérifier les `null`.
- Organiser le code par packages (`commands`, `listeners`, `services`, `utils`).
- Séparer la logique métier des listeners et des commandes.
- Utiliser des permissions plutôt que des vérifications de nom de joueur.
- Favoriser les composants immuables (`Component`) pour les messages.
- Éviter les accès répétés à la configuration en mettant en cache les valeurs si nécessaire.

---

# Conclusion

Avec ces bases, vous pouvez développer la majorité des plugins Paper modernes :

- ✅ Commandes Brigadier
- ✅ Events
- ✅ Inventaires
- ✅ Items personnalisés
- ✅ Entités
- ✅ Particules
- ✅ Sons
- ✅ BossBars
- ✅ Scoreboards
- ✅ Configurations
- ✅ Scheduler
- ✅ PersistentDataContainer
- ✅ Adventure API
- ✅ Structure de projet propre

Ce socle couvre la plupart des mécaniques utilisées dans les plugins Paper modernes et constitue une excellente base pour créer des systèmes plus avancés (GUI interactives, mini-jeux, RPG, économie, quêtes, etc.).
