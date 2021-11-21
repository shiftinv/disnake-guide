# Creating commands

!!! Note

    This page is a follow-up, and the base code used is from the previous page ([Initial files](./101-initial-files.md)). The code can be found on the GitHub repository [here]({{ guiderepo }}/tree/main/docs/extra-code-samples/intial-files).

Discord also allows developers to register [slash commands]({{ devdocs }}/interactions/application-commands), which provides users a first-class way of interacting directly with your application. These slash commands shall be covered by the guide [here](../200-interactions/202-slash-commands.md), in the **Interactions** section.

## A note on prefix commands

Bot commands that are initiated when a keyword is used along with a specified prefix (such as `!` or `$`) are known as **prefix commands** (are also often referred to as **text commands**). 

!!! Warning "Message Intent - Privileged"

    It is to be noted that handling prefix commands requires the **message intent**, which allows the bot to get content and data of messages sent by users. This intent has recently been privileged, i.e., it needs to be manually enabled for the bot application, and its requirement will be reviewed if you eventually apply for your bot's verification.

    You can read more about the message intent [here][message-intent-article].

Therefore, to minimize the permissions your bot has to use, we will be convering prefix commands under the **Popular Topics** section, and advancing with the basics of slash commands in this article; more advanced topics will be covered in the [**Interactions**](../200-interactions/202-slash-commands) section.

## Registering commands

This section covers the bare minimum to get you started with registering slash commands. Once again, you can refer the [this page](../200-interactions/202-slash-commands) for an in-depth coverage of topics, including guild commands, global commands, options, option types, autocomplete and choices.

Now, we shall continue with the base code used in the previous section.

``` python linenums="1" title="main.py"
import disnake
from disnake.ext import commands

bot = commands.Bot()

@bot.event
async def on_ready():
    print("The bot is ready!")

bot.run(YOUR_BOT_TOKEN) 
```

The first step is to use the `@bot.slash_command` coroutine, along with an `async` function in order to define the code for your slash command. Below is a script demonstrating the same (focus on the use of `inter`, which is short for `interaction`).

``` python linenums="1" title="main.py" hl_lines="10-12"
import disnake
from disnake.ext import commands

bot = commands.Bot()

@bot.event
async def on_ready():
    print("The bot is ready!")

@bot.slash_command()
async def ping(inter):
    ...

bot.run(YOUR_BOT_TOKEN) 
```

The `inter` passed into the function is analogous to context, or `ctx` used in prefix commands - it passes through all information relative to the interaction - data regarding the user who initiated the command, as an example. It is also necessary for replying to the use of the command.

??? Note "Using `ctx` vs. `inter`"

    If you have experience with coding bots with [`discord.py`]({{ dpydocs }}), you would be familiar with using `ctx` as an abbreviation for passing context into the function. This guide will primarily be using `inter`, as it is short for `interaction` and refers to [`disnake.ApplicationCommandInteraction()`]({{ disnakedocs }}/api.html?highlight=applicationcommandinteraction#applicationcommandinteraction). Of course, you're open to using your preferred abbreviation in code.

### Registering commands in specific guilds

Note that servers are referred to as "guilds" in the Discord API and disnake library. On running the above code, the slash command will be registered globally, and will be accessible on all servers the bot is in. The caviat being that global registration of slash commands can take upto 1 hour (Refer [Discord's docs]({{ devdocs }}/interactions/application-commands#create-global-application-command)). 

When you're trying to test your changes to code in real time, it can be immensely useful to have the command's function update with your code changes right away. Thus, you can use the `guild_ids` argument for the command to be instantaneously registered in a list of specified servers. (We recommend including your separate development server in this list.)

``` python linenums="1" title="main.py" hl_lines="10-12"
import disnake
from disnake.ext import commands

bot = commands.Bot()

@bot.event
async def on_ready():
    print("The bot is ready!")

@bot.slash_command(
    guild_ids = [1234, 5678] # Your server IDs go here.
) 
async def ping(inter):
    ...

bot.run(YOUR_BOT_TOKEN) 
```

???+ Tip "Using `test_guilds` in `commands.Bot()`"

    When you have multiple commands registered under the same test guilds, it is convenient 

Now that you're all set with registering the slash command, you can proceed to respond to the initiated command.

## Replying to commands

You can reply to a slash command initiated by the user, using `inter.response.send_message()`. It is analogous to using `ctx.send()`, in that you can respond to the interaction with embeds, files, buttons/select menus or just plain text.

``` python linenums="1" title="main.py" hl_lines="14"
import disnake
from disnake.ext import commands

bot = commands.Bot()

@bot.event
async def on_ready():
    print("The bot is ready!")

@bot.slash_command(
    guild_ids = [1234, 5678]
) 
async def ping(inter):
    await inter.response.send_message("Pong!")

bot.run(YOUR_BOT_TOKEN) 
```

<br>
    <p align = "left">
        ![](../assets/img-creating-commands/001.png){ width="60%" }
    </p>
<br>



[message-intent-article]: https://support-dev.discord.com/hc/en-us/articles/4404772028055