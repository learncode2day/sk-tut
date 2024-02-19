# Conditionals

Conditions are ways to control what code in your program executes and what doesn't. In other words, they allow you to do something if another thing passes some kind of check. Traditionally the check is something can be narrowed down to true or false answer, like a yes or no question. For example, "Do you have a pen on you?". That can be answered with a simple yes or no (true or false).

Conditions can take a few different forms in Skript, each of which will be outlined below with their own pros and cons.

Conditions in Skript are pretty straight forward: most things you may want to check have their own dedicated condition. Some others might not have a dedicated condition, and instead use a kind of "generic" condition with an expression. For example, let's say that you wanted to check if a player is allowed to fly. You have two options:

if player's fly mode is true

The first uses the dedicated condition in Skript (notice how it has no `is`, `=`, `contains` etc etc.); the second uses the `fly mode` expression with a "generic" condition, in this case `is/is equal to`.

Generic conditions are used when a dedicated condition does not exist or you have a more specific thing you check for. Things like `is / is not`, `= / !=`, `is between / is not between`, `> / <` are all generic, they just simply check two objects and can be used in the same way as normal:

The traditional way conditions are used is the following:

if player's y-coordinate is greater than 10:

Here,`<condition>` is the "question" you are asking. If the condition passes it will run the code inside the section, then continue on running the code after the if section. Keep note of how the indentation changes!

If the condition does not pass, then the code inside the section will be skipped and the code below will be ran.

What if you want to check some other condition if your first condition fails? Say maybe your check for whether the player's y-coordinate is greater than 10 didn't pass, but now you want to check if it's less than -20. Then you can utilize an `else if`. It operates just like a normal `if`:

else if \<other condition>:

if player's y-coordinate is greater than 10:

else if player's y-coordinate is less than -20:

Notice how the `else` is at the same indentation level as the top `if`. If the first condition passes then the code within the first section will run and the code in the second section will be skipped. Now if the first condition does not pass, then the reverse applies; the first section is skipped and the second section is ran.

If you need to perform a catch-all, perhaps when you don't know all the possible outputs or you just need to catch everything that does not pass, `else` comes in handy. It can either be applied after an `if` or after an `else if`:

else if \<other condition>:

if player's y-coordinate is greater than 10:

else if player's y-coordinate is less than -20:

As you can likely guess, the `else` will only fire if the previous condition(s) did not pass, it behaves as a catch-all. Keep in mind that since `else` are a catch-all, you **cannot** chain them like so:

Inline ifs are a neat thing, but are generally discouraged as they make code harder to read at a glace and can be easily overlooked. Inline ifs are created by not including the leading `if` and the ending colon, which means you also don't indent the subsequent lines:

Note that inline ifs will skip all of the following code in their section. When they're placed at the root level of an event or command, they will effectively stop all execution if they don't pass. If placed at the root level of a loop, they'll skip to the next value like `continue` does if they don't pass.

Inline ifs also allows you to chain conditions without indenting, like the following:

player's gamemode is survival

Keep note that there is no way to use `else if` or `else` with inline ifs.

As you can probably see, it can make code harder to read -especially if used often- but can be useful to limit the amount of code that runs in loops or events.

The only purpose of the `do if` effect is to run an effect if a condition passes all in one line, an example being:

send "hello" if distance between player and {spawn} <= 10

Notice how there is no indentation differences, colons, and how the effect comes first and then the condition.

Keep note that there is no `else if` or `else` options with this method.

So what happens if you want to check a bunch of conditions? While you _can_ use Inline Ifs or create a huge indentation pyramids, there is a much better way. Skript's `if any` and `if all` statements allow for multiple conditions in _the same condition_. This is super useful when you have tons of conditions like so:

player's gamemode is survival

player is wearing a turtle helmet

player is holding a trident

player's tool is enchanted with riptide

name of player is "Aquaman"

send "You passed all the conditions!" to player

send "You didn't meet all the conditions! to player

Notice the `else` statement! These multi-conditions can include `else if` and `else` statements inside of them!

This works well because we can check multiple conditions without losing code quality. But what if we only need a single condition to be met out of many? For example, what if we want **both** admins and builders to have build permission? In that case, we can use `if any` like this:

player's gamemode is creative

name of player is "BobTheBuilder"

send "You placed a block!" to player

send "You don't have permission to place blocks!" to player

When **at least** one condition is met, the player will be able to place the block. However, if none of the conditions are met, the event will be cancelled.

Ternaries are similar to the `do if` effect, but they're expressions instead. They're designed to return one thing if a condition passes and a different thing if it does not pass. A simple but straight forward example looks something like:

send "hello player" if player is not op, else "hello admin"

If it seems a bit hard to understand, let's highlight the expression itself:

"hello player" if player is not op, else "hello admin"

This is saying the following:

Return the text `hello player` if the player is op. If they are not op (the condition fails), then return `hello admin`.

Ternary can be used to return any object or sets of objects, not just limited to strings:

\<object> if \<condition>, else \<object>

A quite common use case for ternaries in Skript is to toggle a boolean variable, ie if a variable is set to `true`, then set it to `false` and vise versa:

set {myVariable} to true if {myVariable} is false, else false

See if you can understand how and why that works.
