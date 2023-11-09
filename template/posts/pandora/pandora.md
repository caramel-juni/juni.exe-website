# Pandora

[The Discord Bot*](https://github.com/Cubie87/Pandora)

Yes the name is overused. No I won't stop using it.

Interestingly, I wrote Pandora, not because I wanted a discord bot, but because I wanted to learn python. Yep, that "programming language". To be honest, it's quite nice now that I've gotten used to it.

## Beginnings

Getting a basic bot online isn't too difficult, there's a bunch of guides available online, which demonstrate the basics of authenticating with a token, setting the bot status to online, and such. What I wanted to do was explore python's functionality a bit more, and so tried out dice rolling. The need for input validation, rng, and user interaction made it quite a good challenge as a "beginner python project" (noting that I've done programming in other languages previously), and you can still see the original, junior python style, code, in `diceRoller.py`, which I probably should refactor at some point, but likely won't. This was back in late 2021.

It was also awful, using a bunch of if/else statements all within the `on_message()` function, as I hadn't made use of `discord.py` and `discord.ext` fully yet. 

## 2022

I refactored this code around mid-late 2022 to properly use `discord.ext` and `ctx` (context) variables. I find it interesting that most of this was written by reading documentation as opposed to following examples. Both are valid learning techniques, but one is easier than the other, so I'm glad I got to experience both side of this.

#### Audio

This allowed me to also add audio playback features, as it was around this time that other discord bots like Rhythom and Groovy were being taken down through their YouTube API hooks. I didn't want Pandora to be vulnerable to this same dependency, so decided to utilise `yt-dlp` instead, to cache the files locally and then play the `.mp3` audio files, instead of streaming over YouTube's API.

#### ChatGPT

Nov 2022 - OpenAI's ChatGPT gets released. Within a few weeks, a friend messages me asking if integrating Chatbot functionality into Pandora would be possible. Lo and behold, ChatGPT was able to write an integration for ChatGPT, still visible in `pandora.py`. Apart from updating it to the newer `gpt3.5-turbo` model and the appropriate syntax for that (originally was `text-davinci-003`), it's still essentially a ChatGPT written ChatGPT integration.

## 2023

#### Docker

Late 2022/Early 2023 I wanted to experiment with Docker and containerising my services for my compute server. The main motivation was running two versions of minecraft, which required two different versions of Java, and hence, Docker. This didn't really pan out, so Pandora's dockerfile is mostly deprecated/legacy, but was still a fascinating experience to learn about Dockerising things.

Another thing that prevented the use of Docker for my workflow is the incessant desire to keep all data archivable for the long term. For Minecraft, this would be world regionfiles and userfiles, and Pandora would be the (now defunct) chatgpt logs that it left. Read/Writing to/from outside of a Docker container into the host is awfully painful to setup, so I didn't pursue it, though may again another day to learn more about it.

#### CTFtime

Around this time, I also started to get into CTFs, or Capture the Flag, Competitions. They're competitions where vulnerable systems, files, etc, are presented to competitors, who attempt to retrieve a defined string, usually in the style of `ctfNameHere{someTextHere}`, proving that they were able to exploit the system in some way. These are usually hosted in CTFtime, a platform which has a really nice API, and that I was quite happy to yoink and get Pandora to send over upcoming ones, with Discord timestamp tagged to shift to the correct unicode epoch, ensuring that whoever is looking at the message will get the correct start and end times regardless of their timezone.

#### Metro and Twitter

During academic examination season, I was quite concerned with the public transport links having issues, detours, or route cancellations. I had previously used Twitter to check for updates (cause the transport provider, despite having an API, _doesn't update it properly and instead refers people to use their social media accounts instead_ WHYYYYY?!?!?!?!?!), but it was becoming more and more inaccessible due to, ahem, administrative changes.

The solution? APIs babey! Hook Pandora into Twitter and let her forward through the updates! Problem is, Twitter charges for their API, so a third party had to be found. I originally used RapidAPI's twttr, but after difficulty with that (I think it had to do with changes to the returned request format, but also in terms of api difficulty, it wasn't there for me, especially when the actual content wasn't that far away), I moved onto nitter.

Nitter is amazing! You can host your own instance, there's a rss feed option, it's free open source, amazing! Anyway so just pull down the rss feed, read it as a python dict, and forward all posts to discord! Done :)

#### The Game

You lost the game `:)` (yes this is a feature of Pandora)

#### Discord Events

Discord events integration was added around this time. Why not. (Motivated by a friend wanting this feature). Just reading the Discord API (easy), and creating a valid `.ics` iCalendar file (difficult) based on the events. It took a while, but eventually got it working. Unfortunately it can't handle multi-line event descriptions, which I still need to at time of writing. Handling `\n` the character and `\n` the two separate characters, is going to be interesting.

#### Refactoring Code

Around this time I also took the time to tidy up the code a bit more, and moved almost all of the functions to their respective modules. It's still not pretty, but I think it's like, 90% of the way there for looking nice.


