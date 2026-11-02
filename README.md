# User guide for my ultimate plugin
This plugin allows you to create minigames by creating configuration files in the
config folder `/config/BeatdownUltimatePlugin/`. Plugin heavily relies on triggers,
which are boxes that allow for triggering different features. I **heavily**
recommend using proper software for editing the configuration files such as
Visual Studio Code.


## Commands
To access all the commands you use `/mg`. Here's the list of all the available options:

### Minigame commands
* `/mg list` - shows you the list of your registered minigames
* `/mg reload` - reloads all the minigames and triggers from the files
* `/mg printJson <name>` - debugging command, allows you to see how the plugin read
your minigame file
* `/mg lobby <name> <targets>` - teleports players to the lobby location of the
minigame
* `/mg start <name>` - starts the minigame
* `/mg stop` - manually stops the running minigame

### Kit commands
Kits are not being saved in a file but rather in your world. If you happen to
regenerate your world, then the kits will be lost.
* `/mg kits list` - list of created kits
* `/mg kits save <name>` - saves your current inventory in a form of kit under the
provided name
* `/mg kits remove <name>` - removes the kit
* `/mg kits load <name>` - replaces your current inventory with the kit
* `/mg kits give <name> <targets>` - gives the kit to the players listed

### Triggers commands
* `/mg showtriggers` - mostly a debug option but very useful, 
renders the triggers to the players
* `/mg hidetriggers` - hides the triggers again


## Configuration files
Now this is where you'll spend most of your time with the plugin. It's important
that you'll familiarize yourself with presented concepts. You'll find a list of
available options for triggers as well as minigames. Now it is super important
that you understand, that EVERY SINGLE OPTION IS **REQUIRED**. You have to specify
it with a valid value. A lot of those options won't be used in every situation,
but they're there in case you need them. If you don't specify an option, the
plugin will throw an error and tell you which option you're missing or which
one is poorly configured in the server console. But first some basics.

### JSON format
I'll lead with an example of how a JSON configuration file could look like:

```json
{
    "someRandomTrigger": {
        "actionType": "GameMode",
        "bounds": [
            "-7 -60 28",
            "-13 -58 28"
        ],
        "message": "Creative gamemode!",
        "kit": "",
        "location": "65.0 37.0 23.0 90.0 0.0",
        "buttonMode": false,
        "buttonPosition": "110 20 407",
        "gameMode": "CREATIVE",
        "potionEffect": {
            "type": "minecraft:jump_boost",
            "duration": 20,
            "amplifier": 2
        },
        "allowOnlyOneTriggerPerGame": false,
        "allowMultipleTriggersPerPlayer": true,
        "waitForFullTeamToTrigger": false
    }
}
```

JSON is all about objects and their properties. Curly brackets `{ }` represent
objects. The properties are made of the name of the property and a value.
You separate properties using a coma `,`.  The names are **always** 
written in double quotation marks `" "`. The values on the other hand are 
different. There are different types of values such as:
* `"fl;eajldka"` - a string value. It is also closed in quotation marks just as
the name of properties.
* `2137` - a number. You don't use any quotation marks going forward from here.
* `true` - a boolean value. Either true or false.
* `{ }` - an object. Yes. The value of a property can be another object.
This object can then have its own properties.
* `[ ]` - this is a list. A list contains only values, no names. You separate
those values with a coma `,`

JSON doesn't care about lines as well. If you really want to, you can have the 
entire configuration in one line, as long as the syntax is correct but it is
not recommended.

Now you can try and analyze the previous example with your newly gained knowledge.

### The actual configuration files
Your file tree will look like this:

```
BeatdownUltimatePlugin
    \_ minigames
        \_ DEFAULT.json
        \_ minigame1.json
        \_ minigame2.json
    \_ globalTriggers.json
```

As you can see, inside your configuration folder, that is 
`/config/BeatdownUltimatePlugin`, you have a folder named `minigames` where all
your minigames files will be located. The file `DEFAULT.json` will always be there,
and it will be regenerated every time the plugin loads the minigames. This file
is for you. You can use it to check the default configuration of any options in
case you forgot, how to spell them or if you just need some placeholder. This is
where you can take them from. Important note: this file is not formatted the
way you're used to. All the options there are packed in one line. This is where 
Visual Studio Code will save the day. You can just press `SHIFT + ALT + F` and the
file will be formatted they way it's supposed to. You can also use this shortcut 
to quickly format your own files. You also have a file called 
`globalTriggers.json`. If this file doesn't exist, you can create it.
I'll start our endeavor with explaining the `globalTriggers.json` file.

#### Triggers

In this part I'll explain how the `globalTriggers.json` needs to be configured.
You've already seen the example of how this file can look like. It's the initial
example that I've shown before. Here you can specify triggers that will work
even outside a minigame. They will always be active. This is how you configure
a trigger:
* `"someRandomTrigger": {` - name for your trigger.
* `"actionType": "GameMode",` - type of the trigger. 

Here's the list of available types:

*  `DummyPointSystem` *- this is so that this trigger can be used for a point
system.
*  `Death` - kills the player on entry.
*  `LoadKit` - gives the player a specified kit.
*  `SpawnPoint` - sets a spawnpoint to a specified block.
*  `Teleport` - teleports player to a specified block.
*  `GameMode` - sets a gamemode of the player to the one specified.
*  `Potion` - grants a player a potion effect.
*  `ResetArena` *- resets the arena of the current game.

The ones with a star * can't be used in a global trigger. They only work within
a setting of an active minigame.

* `"bounds": ["-7 -60 28", "-13 -58 28"],` - this is where you specify the bounds
of the trigger.
* `"message": "Creative gamemode!",` - this is the message that will be shown to
a player when they cross the trigger. You can leave that empty as `"message" : "",`
to not show any message.
* `"kit": "",` - if the `actionType` is set to `LoadKit` this value is used as a
name of the kit to load.
* `"location": "65.0 37.0 23.0 90.0 0.0",` - if the `actionType` is set to
`SpawnPoint` or `Teleport` this is the value that will be used. It is made of
3 coordinate numbers and 2 numbers that correspond to the viewing angle. You
can use them to make the player look certain way when teleported. To quickly
copy the location and the viewing angles use `F3 + C` in game.
* `"buttonMode": false,` - you can also make your trigger be triggered by a button,
or a bell or literally any block, really. You just make the trigger use the
`buttonMode` and later specify the position of the block. The trigger will get
triggered if a player right-clicks the block at those coordinates.
* `"buttonPosition": "110 20 407",` - position used when `buttonMode` is set to
`true`
* `"gameMode": "CREATIVE",` - if the `actionType` is set to `GameMode` this is
the value used for the game mode
* `"potionEffect": { "type": "minecraft:jump_boost", 
"duration": 20, "amplifier": 2 }` - if the `actionType` is set to `Potion` this
is the potion effect that will be applied. I believe the properties of the object
that is a value of the property `"potionEffect"` are rather straight forward. 
Important note tho, the `type` has to use the `NamespacedKey` syntax, so basically
`minecraft:effect`.
* `"allowOnlyOneTriggerPerGame": false,` - this trigger can only be triggered once
per entire game.
* `"allowMultipleTriggersPerPlayer": true,` - whether to allow players to trigger
this trigger multiple times per game.
* `"waitForFullTeamToTrigger": false` - whether to wait for the entire team to
activate this trigger before actually taking action.

These were all the options available for triggers. The last 3 options do not
work as global triggers. They require an active game to work. If you want to 
add multiple triggers you do so by separating them with a coma `,` since they 
are literally property objects. Look example:
```json
{
  "trigger1": {
    "all your properties go here": ""
  },
  "trigger2": {
    "all your properties go here": ""
  },
  "trigger3": {
    "all your properties go here": "" }
}
```

#### Point systems
These could be a bit complicated at first. But you'll understand. You use point
systems everywhere where you need to grant players points. You also use them
to specify game ending condition. Example:
```json
{
  "finish": {
    "criteria": "Dummy",
    "max": 50,
    "minusPerStep": 20,
    "triggerName": "finishTrigger",
    "finishAfterTriggering": true,
    "allowMultipleTriggersPerPlayer": false,
    "waitForFullTeamToTrigger": false
  }
}
```
Let's see what those lines mean:
* `"finish": {` - you should be familiar by now. This is the name of the point
system. You follow it up by opening an object, which is a value of that property.
* `"criteria": "Dummy",` - criteria as to when the point system should be triggered.

Available criteria are as follows:

* `OnKill` - the point system gets triggered when a kill occurs
* `OnSurvival` - gets triggered when someone dies of any cause.
* `Dummy` - doesn't get triggered automatically by anything. You'd use it to make
a trigger activate this point system.
* `LastTeamStanding` - when there's only one team standing.

Rest of the options:

* `"max": 50,` - maximum amount of points that this point system grants
* `"minusPerStep": 20,` - the amount of points subtracted from maximum per player
who already finished. Allows you to create a system where the first player to
finish gets the most amount of points.
* `"triggerName": "finishTrigger",` - the name of the trigger that activates
this point system.
* `"finishAfterTriggering": true,` - "finishes" the player after he triggers
the point system. What happens after is determined by the minigame config.
* `"allowMultipleTriggersPerPlayer": false,` - should the point system allow
triggering itself multiple times by one player
* `"waitForFullTeamToTrigger": false` - should the point system wait for the
entire team to trigger this point system to actually give points and possibly
finish the team.

These were all the options. Enough appetizers, time for the main course.

#### Minigame configuration
There are way more options here. You can use triggers here and also add point 
systems. I'll show you an example from `DEFAULT.json`.

```json
{
    "name": "default",
    "gameMode": "ADVENTURE",
    "matchFormat": "FreeForAll",
    "playerDeathResult": "TeleportToSpawnPoint",
    "gameEndCondition": "AllPlayersFinished",
    "lobbyLocation": "0.0 0.0 0.0 0.0 0.0",
    "startLocations": [
        "0.0 0.0 0.0 0.0 0.0"
    ],
    "instanceXOffset": 0,
    "startDelay": 0,
    "endDelay": 0,
    "timeLimit": 0,
    "numberOfGames": 1,
    "kit": "",
    "breakBlocksOnStart": false,
    "saveAndResetArena": true,
    "disableItemDamage": false,
    "areaBreakOnStartList": [
        [
            "0 0 0",
            "0 0 0"
        ]
    ],
    "arenaBoundaries": [
        "0 0 0",
        "0 0 0"
    ],
    "pointSystems": {
        "someRandomPointSystem": {
            "criteria": "OnKill",
            "max": 0,
            "minusPerStep": 0,
            "triggerName": "",
            "finishAfterTriggering": false,
            "allowMultipleTriggersPerPlayer": true,
            "waitForFullTeamToTrigger": false
        }
    },
    "triggers": {
        "someRandomTrigger": {
            "actionType": "Death",
            "bounds": [
                "0 0 0",
                "0 0 0"
            ],
            "message": "",
            "kit": "",
            "location": "0.0 0.0 0.0 0.0 0.0",
            "buttonMode": false,
            "buttonPosition": "0 0 0",
            "gameMode": "ADVENTURE",
            "potionEffect": {
                "type": "minecraft:luck",
                "duration": 0,
                "amplifier": 1
            },
            "allowOnlyOneTriggerPerGame": false,
            "allowMultipleTriggersPerPlayer": true,
            "waitForFullTeamToTrigger": false
        }
    }
}
```

This one's way bigger but half of it we've already discussed. You can see at the
bottom there are fields for triggers and for point systems. Alright, let's get to
it.

* `"name": "default",` - name of the minigame. 
* `"gameMode": "ADVENTURE",` - gamemode in which the minigame puts the player upon
the start of the minigame
* `"matchFormat": "FreeForAll",` - match format of the game.

Here are the options for `matchFormat`:

* `FreeForAll` - everyone gets put in one container. There is an option later to
spawn different teams at different places but the team members stay together.
* `TeamVsTeam` - this one uses the instancialization. It makes it so two teams
play against each other until everyone has played with everyone. To make things
faster it copies the map multiple times and allows multiple matches to happen
at once.
* `Individual` - this one is similar, except people are playing in their own
containers. Every player individually has their own map to play on.
* `IndividualTeams` - this one is the same as the previous one, except the teams
are put with their team members together in one container.

It's important to note, that if you choose to use `TeamVsTeam` or `Individual` 
or `IndividualTeams`, the point systems might not work as you expect. 
When you set up the point system to give players more points if they finish 
first, rather later, everyone will get max points, since what those options do
is create separate matches for every instance, for every copied container. If
you want to utilize this feature of the point systems you have to use `FreeForAll`
and copy the arenas yourself. Next option is:

* `"playerDeathResult": "TeleportToSpawnPoint",` - what happens to the player after
they die.

Options are as follows:

* `TeleportToSpawnPoint` - teleports the player to their spawn point
* `GameModeSpectator` - turns the player's gamemode into spectator

Next minigame option is:

* `"gameEndCondition": "AllPlayersFinished",` - what is the criteria for the game
to be ended.

Here are the options:

* `LastTeamStanding` - ends the game when there's only one team left that
didn't finish.
* `AllPlayersFinished` - ends the game when all players have finished.

Ok, more minigame options:

* `"lobbyLocation": "0.0 0.0 0.0 0.0 0.0",` - allows you to specify the location
of the lobby area of the minigame. This is the chill area, where you can for example
spectate the map before the match begins. `/mg lobby <name> <targets>` utilize this
exact property.
* `"startLocations": ["0.0 0.0 0.0 0.0 0.0"],` - you put starting locations here.
Notice, that it's a list. You can put as many coordinates as you want in here.
If there are more than one, then the teams will get spread between them. You
probably want one start location for every team that will be playing in one match.
* `"instanceXOffset": 0,` - remember when I was talking about instancialization?
This is the offset between those containers. You have to make it big enough, so
your areas do not intersect each other, or you'll ruin your map. This option only
applies when you use match formats, that utilize instancialization.
* `"startDelay": 0,` - delay in seconds before the walls go down. When you start the
match, players will get teleported to the starting location. They can then move
around there and do whatever. After this time has passed, the walls that you
specified later will go down and the players will be free, or whatever. It also
shows a nice countdown for every player before that happens.
* `"endDelay": 0,` - this is the delay between the game ending, and the minigame
teleporting everyone back to the lobby, or starting a new match. It does not have
a countdown.
* `"timeLimit": 0,` - time limit of the game. When time runs out, the game ends.
* `"numberOfGames": 1,` - the number of games that will be played in a row. 
If your minigame ends too quick, you might want to use this option to allow the
players to play multiple times against each other, before the whole match ends.
* `"maxPlayerHealth": 20,` - maximum health that the players will be put on for the
entirety of the minigame.
* `"overrideDamageValue": -1,` - make players deal a set amount of damage to each
other. Set it to -1 to disable that option.
* `"kit": "",` - the kit that will be given to players at the beginning of the game.
* `"breakBlocksOnStart": false,` - this allows you to use the "walls" feature I've
talked about. After the `startDelay` ends, if this option is enabled, the areas,
that you'll specify later will get destroyed. You can use it to contain players,
before the match really begins.
* `"saveAndResetArena": true,` - when this option is true, the arena that you'll
specify later will get saved when the game starts and then reset when the game ends.
This option is required, if you want to use any triggers, that reset the arena.
* `"disableItemDamage": false,` - for the time of this minigame all items have
infinite durability, when this option is enabled.
* `"infiniteSaturation": false,` - enables infinite saturation for the players
during the minigame.
* `"areaBreakOnStartList": [[ "0 0 0", "0 0 0" ]],` - you specify the areas that
will get destroyed when the `startDelay` ends. Notice, this is a list of lists.
You can have multiple areas destroyed if you specify them after a coma `,`.
* `"arenaBoundaries": [ "0 0 0", "0 0 0" ],` - the boundaries of the arena. In case
you want to use the `saveAndResetArena` option, this is where you specify the
bounding box of the arena. This is just a regular list of two coordinates.
* `"pointSystems": { }` - this is where you specify the point systems that you want
use in your minigame. You can have as many as you want. Look above if you forgot,
how to configure them.
* `"triggers": { }` - these are your triggers for this minigame. These triggers will
only be active when the minigame starts. 

### How would you go about creating your own minigame
* Duplicate the `DEFAULT.json` file and rename it to whatever you wish.
* Start changing stuff.
* Test your stuff. If you want to quickly reload the minigame files you can use
`/mg reload`.
* Repeat until you're satisfied.

## End
That wraps it up. I wish you succeed in your minigames creation quest. Good luck
