+++
date = '2025-09-05T10:00:00+02:00'
title = 'Testing Logging with Thymeleaf'
tags = ['Java', 'Thymeleaf', 'ELK']
+++

## Introduction
In my last post, I described how I set up a dockerized ELK stack on my VPS and how I would use it for centralised logging. To test its functionality and how it would integrate into my development, I wrote a small Java-based web application for link shortening over the course of a few weeks or so.

After setting up my ELK-stack with a log retention policy of 30 days (more than enough for me to have a bit of an overview of how my apps are being used), securing the TCP port (as per my last article), and simplifying the logstash pipeline a bit, the setup was finally ready to receive some data.

As a small side note, life has been getting a bit more real in these last few months, and I simply am not able to keep up with coding to the extent I used to for the time being. This is also the reason why this blog post has been somewhat delayed, despite the project being done for several weeks now. That doesn't mean I will stop writing posts or doing side-projects, but the reality is that I might have to scale back from ambitious ideas like Cockpit to simpler and more manageable project ideas. But, enough excuses. Let's get into some insights!

## So, What Is It?
As mentioned, the new application is a link shortener. Since I believe the messaging behind its branding will very easily get lost in translation when it leaves the target demographic, and because its feature set is not very elaborate, I am choosing not to further highlight it on my portfolio page. It will probably be open-sourced at a later time.

In short, you put a link into the system, and it will generate or retrieve a shortened version of that link, identified via a short, unique sequence of characters. Think [tinyurl.com](https://tinyurl.com/). That's really more or less all it does. It has some architectural features as a result of being mostly a showcase project:

- PostgreSQL database on [neon.com](https://neon.com) stores the links.
- A small Caffeine cache ensures that the number of total link clicks is periodically updated, but not checked on every request to the main page, where this number is displayed.
- Of course, logging to a local Logstash instance with MDCs - as you will see shortly.
- Using Thymeleaf, conditional rendering is used to hide some functionality from unwanted visitors - some more information following on this as well.

## Thymeleaf... Sucks?
At my previous workplace, we used a JSF-based stack for our legacy application. Because of that, I forced [Nya.GG](https://nya.gg) into a JSF stack as well, even though Spring applications have not been served with JSF by default for a while now. That came with its own set of hiccups, mostly related to typical legacy nonsense from JSF and PrimeFaces. You can read more about those hiccups [here](https://vanilla.sh/posts/nyagg-imagehosting/).

However, the one really nice feature when you're writing a small application using JSF is that you can directly call server-side logic from your templates using built-in elements like commandButtons. I was surprised to learn that Thymeleaf can not do this. I believe [this StackOverflow question](https://stackoverflow.com/questions/58070607/how-to-set-a-button-onclick-event-and-link-to-thymeleaf-controller) highlights a user who expected similar functionality, and was probably similarly surprised to learn that the way to do this is just...well, manipulating the browser location via JavaScript.

Thymeleaf exists in a very weird space - it doesn't provide a fully client-side single-page application like we've come to expect from frameworks like Vue, React, or Angular, but it also doesn't have the tight coupling between the UI and the business logic that JSF has. As a result, I don't really feel like there are many reasons to use Thymeleaf for any new software project in 2025. However, being able to hide information from unauthorised clients is still a distinct advantage compared to SPAs. In this application, some scripts for submitting specific types of requests are hidden from the client unless certain preconditions are met. 

## The Hunt For Information
Of course, a "CRUD" application that just maps links to other links is not really anything revolutionary. And as I've stated, the main goal of this experiment was to get my ELK stack up and running. The internet is not really the same as it was a decade ago. Or, maybe it is, and I've just become more observant. But these days, bots very frequently probe any web application they can find for vulnerabilities, and do so in quite surprising or unexpected ways.

As I've noted in a previous article, noise from manual TCP requests quickly started to flood my Kibana when I exposed the TCP port to Logstash. Despite certificate-based authentication, I decided to shut down the port and rely on local networking for feeding into Logstash instead. I've also noticed increasing amounts of traffic from Cloudflare analytics, which don't really match up with the actual observed usage of the web apps.

[Nya.GG](https://nya.gg) was a big reason why I wanted to increase tracing through logs. Due to a bug, the login bean was initially not view-scoped, leading to exposed usernames of attempted logins. I was rather surprised to actually find a variety of usernames typed into the box over a span of a couple of days, including variations of my own. However, as logs were only stored in a text file on my VPS, I had no way of quickly estimating in a structured way what was happening on the project. In fact, I had to make sure to plug some holes in the [Nya.GG](https://nya.gg) code before even publishing this article, as I wanted to avoid encouraging illicit behaviour. At this point, I think the site should be robust enough to withstand common attack patterns.

In my new link-shortening tool, I had an early surge in requests shortly after publishing it. Interestingly, some requests to create a new link were made to URLs that sounded plausible, but didn't really exist. I don't want to leak those URLs here, but think _"firstname.country"_. The shorthands were never actually accessed either. I wonder if some kind of agentic AI work was involved due to the very generic-sounding domain names, or if it was simple penetration testing. You'd think if an actual user wanted to create a link mapping, they would at least test if it works afterwards. Regardless, these types of behaviours are now seemingly commonplace with any new published application.

All that is to say that personal web projects nowadays have to deal with attack vectors that I was probably partially oblivious to back then, but also more automated traffic than I would have imagined for what are essentially "for fun" projects. Outside of this blog, which itself is already hard to find, these projects are never advertised, so it is interesting to observe these behaviours. 

The new application now logs directly into Logstash via the local network. To provide this functionality during development, I use SSL tunneling to the VPS hosting the ELK stack. During development, a different environment is set, which is propagated to the ELK stack as a mapped diagnostic context (MDC) variable, so the logs can easily be differentiated from production logs. Other MDCs that are logged are the source IP and user agent of the browser, to differentiate traffic from different users.

## The Deployment Pipeline
The new web application follows a similar deployment approach as the previous ones, which is using a deployment user who owns the web applications on my VPS, building the application, and copying it via SSH, where it is started as a service and served through an nginx reverse proxy.

As I deploy more projects on this server, I am starting to be more concerned about possible interactions between applications. In the future, two applications could be built on different Java versions, at which point deploying them on the same server becomes somewhat of a hassle, if nothing else. Additionally, certain project-specific requirements might make it more difficult to redeploy the application elsewhere in the future, should that be necessary. For example, [Nya.GG](https://nya.gg) requires a Java runtime, and for a future extension for video editing, FFmpeg also needs to be available, and it is generally too large to be reasonably shipped as part of the current GitHub actions workflow. On top of that, I want to more easily enable both local- and cloud-based deployment of future applications.

For these reasons, while the current approach has worked fine so far, I will try to retroactively adapt all my applications that are currently deployed on my server to use containerization using Docker or Podman, and change the deployment pipelines accordingly. This might take a while, come with its own lessons learned, and will likely have its own blog post. Until then, stay tuned, and enjoy the projects!
