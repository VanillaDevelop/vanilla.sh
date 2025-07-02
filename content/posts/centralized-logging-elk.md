+++
date = '2025-07-02T20:00:00+02:00'
title = 'Centralized Logging with Dockerized ELK - And Other Updates'
tags = ['SysAdmin', 'ELK', 'Docker', 'AI']
+++

## Introduction
My last couple of posts were about project management and personal setups (like my switch to Linux). On the latter, I want to give a little update, but mostly, I want to talk about actual code stuff I've been working on and future projects.

## Linux Stuffs
On the topic of Linux, I switched from Ubuntu to Mint. I've found that for a daily driver, Mint is much more stable and avoids many of the issues I've had to face with Ubuntu outright. That whole nonsense of having to re-compile a package because of mouse stutter? Not happening on Mint. Different refresh rates for my monitors? Working immediately as intended. German hotkey combinations being picked up properly in all applications? Yep! Login prompt on every monitor? Damn, not even Windows had that. Where I've had problems with Ubuntu in the past (famously bricking it about thrice in one university year on my laptop), Mint kinda just works. I'm sure this could be considered a "skill issue" by Linux veterans. I do have a spare PC that I want to mess around on with Arch, eventually, but I think it's a reasonable decision to stick with something that just works for my main machine.

Most of the other stuff from my previous post still holds. The only change I've made is switching my makeshift file sync setup from CRON-based rsync calls to [InSync](https://www.insynchq.com/), which is a one-time paid tool that gives a lot more leverage and provides a neat UI to determine which folders to sync where, and in what manner.

Although, you should probably be a bit careful with deleting stuff on that tool...After initially setting my user's home directory as my base sync folder, then quickly realizing that means all my random configuration files will sync to the cloud, and trying to delete those files from the cloud...yeah.

It didn't help that InSync was really persistent about this, too, and kept instantly trying to delete my home directory as soon as I re-opened the app to stop it from doing just that, even after a full reinstall.

Anyway, I eventually managed to wrangle the tool and set it up properly, and it's been working nicely since. Although, now I'm a bit paranoid about having my stuff incidentally deleted. Not that it'd be a first, since something similar almost happened to me when I used Proton Drive.

I think in the near future I will set up some occasional one-way syncing of the full drive to another medium, to prevent these types of accidents from occurring, since by now I have quite a collection of personal data that I don't want to lose.

## AI Stuffs
I am constantly trying to figure out how to integrate AI into my coding without having it take over to the point where my personal skills regress. Usually, I have been sticking to a more lightweight code completion in the form of GitHub Copilot. Although I personally preferred cursor back in the day (not really sure how well it performs these days), Copilot mostly does fine in nudging me into the correct direction syntactically without overwhelming me with random suggestions.

Recently, Claude launched a new tool that is rather interesting, called [Claude Code](https://www.anthropic.com/claude-code). It functions as a CLI tool that connects to different IDEs to get additional context. Notably, that includes JetBrains IDEs (thank god). I am currently experimenting with this tool to see if it can supplement low agentic work or answer more complex questions without context switching, sort of as a replacement to what IDEs like Cursor or Windsurf provide. I've found it to be moderately useful so far, although like many LLMs it still frequently gets stuck on a stubborn train of thought or tries to gaslight me about functionality that doesn't actually exist.

My experiment with Gemini, which I started after my last blog post as a result of switching to Google Drive, has shown mixed results. In most cases, it works fine as a ChatGPT replacement, but it doesn't seem to work very well on non-code-related tasks, and internet search is a mixed bag. I'm not fully convinced of its capabilities and might be looking to replace it again soon.

In the future, I want to get back on the "technical" AI track that I've somewhat neglected since my University days. Mostly, that means getting back into the inner workings of LLMs and seeing how things have evolved in the last year and a half, but I might also mess about with other types of models, like image generation. Although I'm not really a fan of AI art, I think it's good to see how these types of tools develop, rather than staying ignorant of their capabilities. If nothing else, this is always useful to avoid falling prey to disinformation.

## Containerized ELK
On the technical side, something that I've been working on for a while now is getting a containerized ELK stack up and running on my VPS. It sounds pretty simple, and maybe it is, but it required some faffing around for me. Mostly to figure out how to properly integrate a .env file and how to set up the logstash pipeline.

First, I tried exposing an HTTP port with basic authentication to ingest information from external applications. In any case, this probably wouldn't have been a very good idea because by not using HTTPS, there are a variety of pretty basic attacks that could be used to compromise sensitive information. However, I soon learned that using such a channel in a regular logging framework like logback isn't really that well-supported anyway. So I went back to TCP, but naturally, I didn't want to have an exposed TCP port for the world to see. So I quickly looked up how to generate some long-running certificates that I could use as credentials, and imṕlemented them in a test application to see if it would work. They did, after some messing around, so I went to bed happy!

### You Couldn't Behave, Now Nobody Gets TCP! >:c
However, when I woke up the next day and started playing around with the application logs a bit, I noticed something weird. On top of my application logs, I saw a bunch of JSON parse failures in my Kibana. Lo and behold, it looks like a bunch of bots kept trying to get into my TCP port every couple of hours. It's quite interesting, because I've hosted web projects in the past with no issues, but this time around, both my server itself and my web applications seem to get quite a bit more traffic, presumably some organic, but mostly artificial.

Although the certificate setup should have been relatively safe, I decided to shut the TCP port off to the public for good and instead use SSH tunneling for local development, since most applications will be either eventually deployed on the same server or to a different one with a static IP regardless, so having the port open is overkill.

Unsurprisingly, after shutting the TCP port off in the firewall, I received no more malformed traffic. While this setup works decently well now, I will look a bit further into how and for how long the logs are stored before considering this setup to be fully complete. Once it is complete though, I aim to use it as a centralized logging destination for all future projects.

## New Projects?
There are some projects that I have been working on over the last couple of months. The primary one has unfortunately lost some of its lustre since its use case is currently not that relevant to me anymore, but I am pretty close to finishing it, so I want to get that out of the way soon. This will likely be followed by some restoration and maintenance of previous work and some smaller-scale utility applications before I look for my next bigger project.

That's it from me for this time around. Let's see how long it takes for the next one.
