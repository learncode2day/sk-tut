# Custom Commands

Skript allows you to easily create custom commands in your scripts, like the following:

\# A simple "broadcast" command for broadcasting the text argument.

\# This is accessible only to users with the "skript.example.broadcast" permission.

command /broadcast \<text>:

permission: skript.example.broadcast

description: Broadcasts a message to everyone including console.

That's a simple example, but you can do much more complex things with custom commands! That "/broadcast" command only shows a few of the options you have available when creating a custom command.

Here's the full list of features that you can use in your commands. They're all optional, except for the trigger section. We'll explain each one individually below.

command /\<command name> \<arguments>:

prefix: # command prefix, defaults to "skript"

aliases: # alternate command names

executable by: # players or console

usage: # the message that explains how to use the command

description: # the command's description

permission: # the permission needed to use the command

permission message: # the message sent to users without the correct permissions

cooldown: # a timespan, during which the command can't be used again

cooldown message: # the message sent when the command is used again too fast

cooldown bypass: # the permission necessary to bypass the cooldown

cooldown storage: # what variable to store the cooldown in

\# The code to run goes here.

The command name is what comes immediately after `command`. It can consist of any characters you want, except for space. Additionally, the / in front of the command is optional. This means `command /broadcast` and `command broadcast` are the same. Here are a few examples:

broadcast "Command: /test-command"

broadcast "Command: /nickname"

broadcast "Command: //set!"

​

[Arguments](https://docs.skriptlang.org/expressions.html#ExprArgument)

follow the command name, and are separated by spaces. You can make arguments optional by surrounding them with square brackets `[]`, like this: `command /kill [all entities]`.

However, the real power in arguments comes from type arguments. These allow a command to take in a

[type](https://docs.skriptlang.org/classes.html)

, like `command /kill <player>`. Type arguments must be surrounded by angle brackets `<>` and can also be made optional by surrounding them with square brackets: `[<player>]`.

The argument can then be referenced in the trigger section by `arg-1` or `argument 1` or a

[number of other ways](https://docs.skriptlang.org/expressions.html#ExprArgument)

. You can also name type variables like so: `<name:type>`, which can then be referenced as a local variable: `{_name}`. Here's the full pattern for arguments:

\<name:type=%default value%>

Arguments can be used in a lot of different ways, so I'll provide some examples ranging from the simplest possible to more complex uses.

\# this command can be run by "/test" or by "/test command".

broadcast "Command: /test or /test command"

\# sends the name of the given player. "of" is optional.

command /name \[of] \<player>:

\# here 'to player' refers to the player executing the command

send "Player's name: %arg-1%, %argument 1%, or %player-argument%" to player

\# heals the player in the argument. If no player is given, it will heal the sender.

command /heal \[\<player=%player%>]:

\# this can be used like /kill zombies, /kill creepers and animals in radius 100,

\# or /kill monsters in the radius 6.

\# the radius will default to 20 if isn't entered.

command /kill \<entity types> \[in \[the] radius \<number = 20>]:

executable by: players # this will be explained later on

loop entities in radius arg-2 of player:

\# broadcasts the text given in the command. If no text is given,

\# it will broadcast the default value of "default text".

\# note that the text is referenced as {\_text input}, rather than arg-1 or similar.

command /broadcast \[\<text input:text="default text">]:

\# Using optional commands for flags

\# /give-item stone sword with name Sword in the Stone and with lore Haha, get it?

\# /give-item heart of the sea with lore \&bA murky blue orb.

command /give-item \<item> \[with name \<text>] \[\[and] with lore \<text>]:

set name of {\_item} to formatted arg-2

set lore of {\_item} to formatted arg-3

A prefix is something **all** commands have, and it goes before the command when Minecraft registers them. You do not have to type this out when executing commands, but you can. The prefix, by default, is `skript`, which means if you leave this blank your command (with its prefix) will look like `/skript:commandName`.

\# This command can be run by "/test" or "/testing\_prefix:test"

\# This command can be run by "/test" or "/skript:test"

If two commands have the same name but different prefixes, only one will be registered.

Aliases are alternate names for your command. For example, the command `/teleport` could have an alias `/tp`. Like in the command name, the forward slash (`/`) is optional.

\# this command can be run by "/teleport" or by "/tp".

command /teleport \<number> \<number> \<number>:

teleport player to location(arg-1, arg-2, arg-3)

Aliases will not overwrite commands registered by other plugins. Say another plugin registers `/spawn`, and you have the following command:

teleport player to spawn of world

If you run `/spawn`, that other plugin's command will run. You'll need to register a new command with that name and have it run your first command.

Who can execute this command. The options are `players`, `console`, or `players and console`.

The description of the command. Other plugins can get/show this with `/help`, like `/help teleport`.

The permission required to execute this command. The message sent to players without the proper permission can be customized with the `permission message:` field.

permission message: Only admins can shut down the server!

This field takes a timespan that the player must wait out before executing the command again. The cooldown can be canceled with `cancel the cooldown` (

[documentation here](https://docs.skriptlang.org/effects.html#EffCancelCooldown)

). Like with the permissions, you can change the default cooldown message with the `cooldown message:` field. The remaining time of the cooldown can be displayed with `%remaining time%` Additionally, you can store the cooldown in a variable with `cooldown storage:`, in order to store the cooldown even when the server restarts.

cooldown message: You can only vote once a day!

cooldown storage: {vote::cooldown::%uuid of player%\}

add 1 to {vote::players::%uuid of player%\}

There are also a number of expressions you can use to interact with the cooldowns of commands. You can get the remaining time with `remaining time`, the elapsed time with `elapsed time`, and the total time with `cooldown time`. You can also get the bypass permission with `bypass permission`.

If you've enabled `keep command last usage dates` in your `config.sk` file, you can get the last time the player used the command with `last usage date`.

You can see the full syntax for these expressions

[here](https://docs.skriptlang.org/expressions.html#ExprCmdCooldownInfo)

.

\# The same vote command but with an improved cooldown message.

\# Sorry about the really long line, can't do much about how it's displayed.

cooldown message: You can only vote once a day! You last voted at %last usage%, and it's only been %elapsed time%. Please wait another %remaining time%.

cooldown storage: {vote::cooldown::%uuid of player%\}

cooldown bypass: vote.cooldown.bypass

add 1 to {vote::players::%uuid of player%\}

This section is where all the code the command should run is located. I'm sure you're familiar with how it works from the previous examples, but in case you're still unsure, some more examples of commands will be displayed here. You can see these example commands and more in the `/plugins/Skript/scripts/-examples/commands.sk` file in your server.

description: Give yourself some items.

executable by: players # test

permission: skript.example.item

cooldown message: You need to wait %remaining time% to use this command again.

cooldown bypass: skript.example.cooldown.bypass

if player has permission "skript.example.item.all":

if (TNT, bedrock, obsidian, mob spawner, lava, lava bucket) does not contain loop-item:

send "%loop-item% is blacklisted and cannot be spawned." to player

command /home \<text> \[\<text>]:

description: Set, delete or teleport to your home.

usage: /home set/remove \<name>, /home \<name>

permission: skript.example.home

set {homes::%uuid of player%::%arg-2%\} to player's location

send "Set your home \<green>%arg-2%\<reset> to \<grey>%location of player%\<reset>" to player

send "You must specify a name for this home." to player

else if arg-1 is "remove":

delete {homes::%uuid of player%::%arg-2%\}

send "Deleted your home \<green>%arg-2%\<reset>" to player

send "You must specify the name of this home." to player

send "Correct usage: /home set/remove \<name>" to player

else if {homes::%uuid of player%::%arg-1%\} is set:

teleport player to {homes::%uuid of player%::%arg-1%\}

send "You have no home named \<green>%arg-1%\<reset>." to player
