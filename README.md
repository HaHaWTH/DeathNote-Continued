<img align="right" width="128" height="128" src="https://github.com/Steel-Dev/DeathNote/blob/main/icon_enlarged.png?raw=truee">

# DeathNote

Death Note is a minecraft plugin developed for 1.16-1.17+, it introduces a custom item that allows users to input another player's name and that player will die. Additionally, they can provide a time of death, along with a cause of death.

Obviously, this is inspired by the anime Death Note. I only wanted to bring it to life in Minecraft.

## How to use
It's simple, just give yourself the Death Note with the command /deathnote give (you can provide a player, as well, if you want)

to view all Afflictions/causes of death, just crouch and right click while holding the note.

To use the note, right click normally while holding it, and then you can start typing.

There are 3 ways of using it - 

1) simply inputting the players name, they will die of the default affliction - which is a Heart Attack. 

2) Adding 'by' after their name and a cause of death, they will die of that cause if it is valid, if its invalid it will go to the default cause.

3) Adding 'in' after the name, or cause of death, and then a timespan (e.g 10 minutes) the player won't be afflicted until that time passes.

Examples:

Herobrine -> Herobrine will die of a heart attack.

Herobrine in 10 minutes -> Herobrine will die of a heart attack in 10 minutes.

Herobrine by fire -> Herobrine will die of fire.

Herobrine by fire in 10 minutes -> Herobrine will die of fire in 10 minutes.

Note: when a player is inflicted, they will not be able to break/place/use anything. Because staying true to the anime, they must die if their name is written in the note. Upon dying, or leaving the server, however, they will no longer be afflicted.

## Screenshots & other media

Coming soon

## Resource Pack
The plugin has it's own resource pack (but, it does not force it) - if you'd like to use it for the custom book texture & gui texture, you can download it [here.]()

## But wait, that's not all, there's more!
Death Note comes with it's own internal API you can hook into with your own plugins and register your own Afflictions! That's right! You can make addons for this plugin.

### How to use the API

Using the API is simple, first you need to add Death Note as a dependancy to your addon - and as a depend in your plugin.yml.

Maven: coming soon

Gradle: coming soon

And then you can get right to registering your afflictions!

Here is an example affliction:

```java
AfflictionManager.register(new Affliction("void", // The ID/Key of the affliction
                    "&8Void", // The display name of the affliction
                    Arrays.asList("void"), // The list of trigger words
                    "The target will be teleported into the void.", // The description (can be blank)
                    "%s fell out of the world", // The custom death message (can be blank)
                    getMain(), // The main plugin class of your addon to tell Death Note who is registering it
                    player -> { // A consumer lambda to define what the affliction will do
                        Location loc = player.getLocation();
                        loc.setY(-60);
                        player.teleport(loc);
                    }));
```

And that's it! You can mass-register as well, and don't forget to call `AfflictionManager#refreshAfflictionsBook()` after registering your Afflictions so the Affliction book is updated with your new entries. (this is to prevent it constantly refreshing the book, it should only be refreshed when needed)

The above forces the addon to only function if Death Note is present, if it is not, it won't load - however, you can of course make it optional - for instance, if the plugin is present, register your Afflictions, if not, do nothing or whatever else.

Usually, I do it this way in my main class:

```java
public Plugin deathNotePlugin;

@Override
public void onEnable() {
  if (getDeathNotePlugin() != null) {
    deathNotePlugin = getDeathNotePlugin();
    if (deathNotePlugin.isEnabled()) {
      getServer().getLogger().info("DeathNote found and enabled! Registering custom afflictions.");
      // Plugin is present and enabled, register afflictions here
    } else {
      getServer().getLogger().info("DeathNote present, but disabled. Skipping registering custom afflictions.");
      // Plugin is present, but disabled
    }
  } else {
    getServer().getLogger().info("DeathNote not present, skipping registering custom afflictions.");
    // Death Note not found
  }
}

Plugin getDeathNotePlugin() {
  return getPluginManager().getPlugin("DeathNote");
}
```

I do it this way to prevent any errors if the plugin isn't present (this could be useful if your addon isn't strictly just for DeathNote)

If you want to, might also be a good idea to make a Utility function to check if DeathNote is present and enabled, so you can run this check before using anything from DeathNotes api to prevent any errors.

```java
public static boolean isDeathNoteEnabled(){
  return MAINHERE.deathNotePlugin != null && MAINHERE.deathNotePlugin.isEnabled();
}
```

Of course these are just examples.

Happy death-writing!
