# Summary

- [Basics of Nightbot](#basics-of-nightbot)
- [Commands](#commands)
- [Javascript](#javascript)
- [Shoutouts (and Twitch channel information)](#shoutouts-and-twitch-channel-information)
- [Counters](#counters)
- [Quotes](#quotes)
- [Extra sources](#extra-sources)
- [Consider supporting me](#consider-supporting-me)

<br />
<br />

# Basics of Nightbot

([Back to top](#summary))

"_[Nightbot](http://nightbot.tv/) is a chat bot for Twitch, YouTube, and Trovo that allows you to automate your live stream's chat with moderation and new features, allowing you to spend more time entertaining your viewers._"

<br />
<br />

# Commands

([Back to top](#summary))

You can manage Nightbot from the [dashboard](https://nightbot.tv/dashboard) or through the [`!commands`](https://docs.nightbot.tv/commands/commands) command.

<br />

My commands make _heavy_ use of [Nightbot Variables](https://docs.nightbot.tv/commands/variableslist). And most of them can **_NOT_** be easily managed from the dashboard.

<br />

> TIP: Unlike StreamElements, Nightbot doesn't care about what your command trigger is. You can use anything as a trigger. Like an emoji 😈, `aword`, or the usual `!prefixcommand`.

<br />
<br />

# Javascript

([Back to top](#summary))

Nightbot also offers a Javascript interpreter in the `$(eval)` command. Meaning whatever actual (reasonable and short) Javascript you use will actually be executed.
<br />

Like:

```javascript
$(eval 1+1) // 2
```

<br />

Here's a simple coin toss example:  
`!commannds add !coin $(user) tossed a coin and got $(eval const r = ['heads 🎃', 'tails 🐈']; r[Math.floor(Math.random() * r.length)])`

<br />
<br />

# Shoutouts (and Twitch channel information)

([Back to top](#summary))

Nightbot offers you a wide range of ways to gether information from a channel. It even has it's own Twitch API calls. All to help you offer a personalized experience.
<br />
<br />

This is an example of your everyday shoutout command:  
`!commands add !so /me Check out $(eval "$(touser)".toLowerCase().replace("@",""))! They were last playing "$(twitch $(touser) "{{game}} - {{title}}")" TPFufun`
<br />
<br />
And this is all the information you can extract from the `$(twitch)` command:
|Variable|Returns|
|---|---|
|{{createdAt}}|date/time user account was created|
|{{createdLength}}|length of time user has been created (years, months, days, hours, minutes, seconds)|
|{{displayName}}|display name|
|{{followers}}|current number of channel followers|
|{{fps}}|stream fps (if live)|
|{{game}}|stream game|
|{{name}}|username|
|{{resolution}}|stream resolution (if live)|
|{{status}}|offline, live, or playing a playlist|
|{{tags}}|stream tags|
|{{title}}|stream title|
|{{uptimeAt}}|date/time user went live (or started playing a playlist)|
|{{uptimeLength}}|length of time user has been live (or started playing a playlist) (years, months, days, hours, minutes, seconds)|
|{{url}}|channel page link|
|{{views}}|current number of channel views|
|{{viewers}}|current number of live viewers|

<br />

> NOTE: I removed unstable variables (like subscriber count, as it doesn't apply to unaffiliated channels). Check the full list [here](https://docs.nightbot.tv/commands/variableslist).

<br />
<br />

# Counters

([Back to top](#summary))

Nightbot does math based on the amount of times a command was used. You can't just create random counters (like StreamElements does).  
In order to count something more specific, you need a `mod-level` command and use that command as counter for a public command.
<br />
<br />

For a simple counter command, you can just print it's own counter. Like this:  
`!commands add f /me $(user) has paid respects ($(count) respects given)`
<br />
<br />
<br />

If you seek something more advanced (like tracking your progress into a task), you'll need to combine two commands (and some variables).
<br />

Before creating an advanced counter, you need to create a public command (as in, the command viewers will use to track your counter).  
This command's output will be replaced by it's counter's output. Think about it as the message that appears while the counter is 0 (zero).  
`!commands add !koroks YA HA NO! We didn't find any Koroks... yet!`
<br />
<br />

Now, we create the counter (_the command that will hold the math_), with a 10 seconds cooldown:  
`!commands add !korok -ul=moderator -cd=10 -a=!commands edit !koroks YAHAHAAAA! 🥬 We got $(count)/900 Koroks!`
<br />
<br />

> The `-a` flag means the command will call another command (_inception_). Passing whatever text that comes afterwards as argument (in this case, `edit !koroks YAHAHAAAA! 🥬 We got $(count)/900 Koroks!`).
>
> Meaning Nightbot will act as it's own channel moderator and edit another command. In this example, it will update the `!koroks` command to now display the text `YAHAHAAAA! 🥬 We got {number}/900 Koroks!`.

<br />
<br />

# Quotes

([Back to top](#summary))

> NOTE: In the future, I'll add a bonus section here for hosting your own quotes system. With source code.

<br />

Nightbot does **NOT** offer a quotes system. Meaning we have to use external services (or custom servers) for our quotes.

I like to use [Twitch.Center](https://twitch.center/), when I'm creating quotes for a new streamer. And then export them into a personalized solution.
<br />
<br />
<br />

## Automatic installation

Just visit [https://twitch.center/customapi/quote/nightbot](https://twitch.center/customapi/quote/nightbot) and follow the instructions.
<br />
<br />
<br />

## Manual installation

First, we need to generate the token for your channel.  
Visit the [https://twitch.center/customapi/quote/generate](https://twitch.center/customapi/quote/generate) URL and save all URLs the page gave you. They should have `/quote`, `/addquote` and `/delquote` as a part of them.

<br />
<br />

> NOTE: No authentication is required. The service will just create random tokens. Meaning you'll lose your quotes, if you ever lose your token. But it also opens the way for interesting interactions. Like two channels having the same quotes pool.

<br />
<br />

List of commands:

- Getting a random quote  
  `!commands add !quote $(urlfetch https://twitch.center/customapi/quote?token=YOUR_TOKEN)`
- Adding a quote (and making it mod only):  
  `!commands add !addquote -ul=moderator $(urlfetch https://twitch.center/customapi/addquote?token=YOUR_TOKEN&data=$(querystring))`
- Deleting a quote (and making it mod only):  
   `!commands add !delquote -ul=moderator $(urlfetch https://twitch.center/customapi/delquote?token=YOUR_TOKEN&data=$(querystring))`
- Editing a quote (and making it mod only):  
   `!commands add !editquote -ul=moderator $(urlfetch http://twitch.center/customapi/editquote?token=YOUR_TOKEN&data=$(querystring))`

<br />
<br />

Available modifiers:

- You can add `&no_id=1` to the url to disable numbering on quotes.
- You can delete ALL quotes by adding `&clear=1` to !delquote command.
- You can silence non-error responses (“Quote added successfully”) with `&silent=1`.
- Using `!quote list` will give you an URL with all your quotes. Easy when migrating to a new service.

<br />
<br />

# Extra sources

([Back to top](#summary))

- Check Nightbot's Custom API forum [here](https://community.nightdev.com/c/nightbot/custom-apis/). You'll find a lot of nice stuff people created.

<br />
<br />

# Consider supporting me

([Back to top](#summary))

- [Buy me a coffee](https://www.buymeacoffee.com/mazeakin)
- [Follow me on Twitter](https://twitter.com/mazeakin)
- [Follow me on Twitch](https://twitch.tv/mazeakin)
- [Join my Discord](https://discord.gg/eYfSNQT)
- [Get sub emotes on my old channel](https://twitch.tv/gataquadrada)
