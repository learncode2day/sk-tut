# Global and Local

Variables, as mentioned previously, have two main "scopes" they can have. They can be global, where any script, any file, any event or function or command can access them. Or, they can be local, where only the event/function/command that created them can access them.

set {a-global-variable} to "hello, world"

set {\_a-local-variable} to "Hey!"

\# this broadcasts "hello, world" and "Hey!"

broadcast {a-global-variable}

broadcast {\_a-local-variable}

\# this broadcasts "hello, world", but not "Hey!"

broadcast {a-global-variable}

broadcast {\_a-local-variable}

As seen above, the global variable is the same no matter where it's accessed. The local variable, though, only is set to `"Hey!"` in the event it was defined in, the `on load`. In the command, it's not set to anything yet. This might seem annoying, but it's actually one of the strengths of local variables.

Say you have an event that includes some sort of wait. Let's say, a command that draws particles for 5 seconds. If you were to use a global variable to store the location, you'd need to somehow make the name of the variable unique for every single time the command is called, otherwise the particles move somewhere else if the command is called again within the 5 seconds.

set {location} to player's location

However, if we use `{_location}`, a local variable, we don't have to worry about this. Each time the command is called, the location variable is only usable inside that instance of the command. This means each time the command is called, it has its own unique location stored without needing to have a unique variable name.

set {\_location} to player's location

show smoke at {\_location}

To reiterate: Local variables are **limited to the place they were set**. You cannot get the value of the local variable outside of when you set it, it's only accessible in the same trigger that you defined it in. If you need to pass information between triggers, or over time, use global variables.

Global variables can also be extremely useful, even if they need to be unique. As shown earlier, global variables can be used as flags to pass information over time, or from one command/event to another:

\# running the /stop-loop command can stop this loop, despite being a different trigger.

while {stop-loop} is not set:

They're also great for holding onto information over longer periods of time, like data attached to a player:

set {last-join::%player's uuid%\} to now

add difference between {last-join::%player's uuid} and now to {playtime::%player%\}

Remember, global variables are **unique**. This means that if you want a variable to be different for each player, you'll have to make its name different for each player!

The easy way to do this is put the player's uuid in the variable, like so:

set {variable::%player's uuid%\} to "hello!"

#### When to Use Global vs Local <a href="#when-to-use-global-vs-local" id="when-to-use-global-vs-local"></a>

Let's summarize. Local variables are great at storing data that we only need for a short time, in a specific place. We don't have to worry about using unique names, or resetting it to some starting value, or anything like that. **Local variables should be used whenever possible.**

Global variables are great for storing data in the long term, like over server restarts. They're also great for allowing us to access data from wherever. We can set the variable in one command and access it in a completely different event, even in a different file. **Global variables should be used when needed, either for long-term storage or for communication across various parts of a script or throughout time.**
