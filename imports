# Imports utiles pour développer un plugin Paper

> Cette liste regroupe les imports les plus couramment utilisés lors du développement d'un plugin Minecraft Paper.

---

# Bukkit

```java
import org.bukkit.Bukkit;
import org.bukkit.Location;
import org.bukkit.World;
import org.bukkit.Material;
import org.bukkit.GameMode;
import org.bukkit.Sound;
import org.bukkit.Particle;
import org.bukkit.NamespacedKey;
```

---

# Plugin

```java
import org.bukkit.plugin.java.JavaPlugin;
import org.bukkit.plugin.Plugin;
```

---

# Joueurs

```java
import org.bukkit.entity.Player;
import org.bukkit.OfflinePlayer;
```

---

# Entités

```java
import org.bukkit.entity.Entity;
import org.bukkit.entity.LivingEntity;
import org.bukkit.entity.Mob;
import org.bukkit.entity.Animals;
import org.bukkit.entity.Monster;

import org.bukkit.entity.Zombie;
import org.bukkit.entity.Skeleton;
import org.bukkit.entity.Creeper;
import org.bukkit.entity.Spider;
import org.bukkit.entity.Enderman;

import org.bukkit.entity.Villager;
import org.bukkit.entity.ArmorStand;
import org.bukkit.entity.ItemDisplay;
import org.bukkit.entity.TextDisplay;
import org.bukkit.entity.BlockDisplay;
```

> Ou simplement :

```java
import org.bukkit.entity.*;
```

---

# Blocs

```java
import org.bukkit.block.Block;
import org.bukkit.block.BlockState;
import org.bukkit.block.Container;
import org.bukkit.block.Sign;
import org.bukkit.block.Chest;
```

---

# Inventaires

```java
import org.bukkit.inventory.Inventory;
import org.bukkit.inventory.InventoryView;
import org.bukkit.inventory.ItemStack;
import org.bukkit.inventory.PlayerInventory;
import org.bukkit.inventory.EquipmentSlot;
```

---

# ItemMeta

```java
import org.bukkit.inventory.meta.ItemMeta;
import org.bukkit.inventory.meta.SkullMeta;
import org.bukkit.inventory.meta.BookMeta;
import org.bukkit.inventory.meta.PotionMeta;
import org.bukkit.inventory.meta.FireworkMeta;
import org.bukkit.inventory.meta.LeatherArmorMeta;
```

---

# Enchantements

```java
import org.bukkit.enchantments.Enchantment;
```

---

# Potions

```java
import org.bukkit.potion.PotionEffect;
import org.bukkit.potion.PotionEffectType;
```

---

# Scheduler

```java
import org.bukkit.scheduler.BukkitRunnable;
import org.bukkit.scheduler.BukkitTask;
```

---

# Scoreboards

```java
import org.bukkit.scoreboard.Scoreboard;
import org.bukkit.scoreboard.ScoreboardManager;
import org.bukkit.scoreboard.Objective;
import org.bukkit.scoreboard.DisplaySlot;
import org.bukkit.scoreboard.Team;
```

---

# BossBars

```java
import net.kyori.adventure.bossbar.BossBar;
```

---

# Adventure API

```java
import net.kyori.adventure.text.Component;

import net.kyori.adventure.text.format.NamedTextColor;
import net.kyori.adventure.text.format.TextDecoration;

import net.kyori.adventure.text.minimessage.MiniMessage;

import net.kyori.adventure.title.Title;
```

---

# Events

```java
import org.bukkit.event.Listener;
import org.bukkit.event.EventHandler;
```

---

# Events Joueurs

```java
import org.bukkit.event.player.*;
```

Exemples :

```java
PlayerJoinEvent
PlayerQuitEvent
PlayerMoveEvent
PlayerInteractEvent
PlayerInteractEntityEvent
PlayerCommandPreprocessEvent
PlayerDropItemEvent
PlayerPickupArrowEvent
PlayerItemConsumeEvent
PlayerRespawnEvent
PlayerDeathEvent
PlayerTeleportEvent
PlayerToggleSneakEvent
PlayerToggleSprintEvent
PlayerSwapHandItemsEvent
```

---

# Events Blocs

```java
import org.bukkit.event.block.*;
```

Exemples :

```java
BlockBreakEvent
BlockPlaceEvent
BlockBurnEvent
BlockExplodeEvent
BlockFadeEvent
BlockGrowEvent
BlockIgniteEvent
BlockPhysicsEvent
```

---

# Events Entités

```java
import org.bukkit.event.entity.*;
```

Exemples :

```java
EntityDamageEvent
EntityDamageByEntityEvent
EntityDeathEvent
CreatureSpawnEvent
FoodLevelChangeEvent
ExplosionPrimeEvent
```

---

# Events Inventaire

```java
import org.bukkit.event.inventory.*;
```

Exemples :

```java
InventoryClickEvent
InventoryDragEvent
InventoryOpenEvent
InventoryCloseEvent
PrepareAnvilEvent
CraftItemEvent
```

---

# Events Monde

```java
import org.bukkit.event.world.*;
```

---

# Events Serveur

```java
import org.bukkit.event.server.*;
```

---

# Configuration

```java
import org.bukkit.configuration.file.FileConfiguration;
import org.bukkit.configuration.file.YamlConfiguration;
```

---

# PersistentDataContainer

```java
import org.bukkit.persistence.PersistentDataContainer;
import org.bukkit.persistence.PersistentDataType;
```

---

# Attributs

```java
import org.bukkit.attribute.Attribute;
import org.bukkit.attribute.AttributeModifier;
```

---

# Vecteurs

```java
import org.bukkit.util.Vector;
import org.bukkit.util.BoundingBox;
```

---

# Particules

```java
import org.bukkit.Particle;
```

---

# Sons

```java
import org.bukkit.Sound;
import org.bukkit.SoundCategory;
```

---

# Biomes

```java
import org.bukkit.block.Biome;
```

---

# Arbres

```java
import org.bukkit.TreeType;
```

---

# Structures

```java
import org.bukkit.generator.structure.Structure;
```

---

# Commandes Bukkit (ancienne API)

```java
import org.bukkit.command.Command;
import org.bukkit.command.CommandExecutor;
import org.bukkit.command.CommandSender;
import org.bukkit.command.TabCompleter;
```

---

# Collections Java

```java
import java.util.List;
import java.util.ArrayList;
import java.util.Map;
import java.util.HashMap;
import java.util.Set;
import java.util.HashSet;
import java.util.UUID;
import java.util.Optional;
```

---

# Java Util

```java
import java.util.Random;
import java.util.Objects;
```

---

# Temps

```java
import java.time.Duration;
import java.time.Instant;
import java.time.LocalDateTime;
```

---

# Math

```java
import java.lang.Math;
```

---

# IO

```java
import java.io.File;
import java.io.IOException;
```

---

# Bonnes pratiques

Évite les imports génériques (`*`) sauf lorsqu'ils concernent de très nombreuses classes d'un même package, par exemple :

```java
import org.bukkit.entity.*;
import org.bukkit.event.player.*;
import org.bukkit.event.entity.*;
```

Pour le reste, privilégie les imports explicites afin de rendre le code plus lisible et plus facile à maintenir.
