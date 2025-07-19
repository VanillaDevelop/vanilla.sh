+++
date = '2025-07-21T20:00:00+02:00'
title = 'Testing Logging with Thymeleaf: 
tags = ['Java', 'Thymeleaf', 'ELK']
draft = true
+++

## As Was Foretold
In my previous blog post, I mentioned my ELK stack and how I would use it for centralized logging. To test its functionality and how it would integrate into my development, I wrote a small Java-based web-application for link shortening over the course of a week or so. 

After setting up my ELK-stack with a log retention policy of 30 days (more than enough for me to have a bit of an overview of how the apps are being used), securing the TCP port (as per my last article), and simplifying the logstash pipeline a bit, the setup was finally ready to receive some data.

## Thymeleaf... Sucks?
At my workplace we use a JSF-based stack for our legacy application, and because of that, I forced [Nya.gg](https://nya.gg) into a JSF stack as well, even though Spring applications have not been served with JSF by default for a while now. That came with its own set of hiccups, mostly related to typical legacy nonsense from JSF and PrimeFaces. 

However, the one really nice thing when you're writing a small application using JSF, is that you can directly call server-side logic from your templates using built-in elements commandButtons. I was surprised to learn that Thymeleaf can not do this. I believe [this StackOverflow question](https://stackoverflow.com/questions/58070607/how-to-set-a-button-onclick-event-and-link-to-thymeleaf-controller) highlights a user who expected similar functionality, and was probably similarly surprised to learn that the way to do this is just...well, manipulating the browser location 