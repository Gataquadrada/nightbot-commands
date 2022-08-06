# Summary

- [Basics of Nightbot](#basics-of-nightbot)
- [Commands](#commands)
- [Javascript](#javascript)
- [Shoutouts (and Twitch channel information)](#shoutouts-and-twitch-channel-information)
- [Counters](#counters)
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
In order to count something, you need a `mod-level` command and use that command as counter for a public command.
<br />
<br />

Before creating a counter, you need to create a public command (as in, the command viewers will use to track your counter).  
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

# Consider supporting me

- [Buy me a coffee](https://www.buymeacoffee.com/mazeakin)
- [Follow me on Twitter](https://twitter.com/mazeakin)
- [Follow me on Twitch](https://twitch.tv/mazeakin)
- [Join my Discord](https://discord.gg/eYfSNQT)
- [Get sub emotes on my old channel](https://twitch.tv/gataquadrada)
