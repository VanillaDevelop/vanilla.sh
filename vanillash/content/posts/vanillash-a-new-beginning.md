+++
date = '2025-02-19T20:00:00+01:00'
draft = true
title = 'Vanilla.sh - A New Beginning'
tags = ['Hugo', 'HTML', 'CSS', 'Vanilla.sh']
+++
## A New Page
Like most developers, I am not a stranger to creating websites and discarding them a few weeks later. It usually comes down to convenience. Who really has time to keep a website updated with new content, features, and projects over a long period of time, right?

At the same time, developers want customizability. Have you ever tried to adapt a WordPress theme (unless you already know the technology stack, in which case you don't count)? Or have you ever almost fallen for the SquareSpace ads that keep popping up on YouTube video essays, just to talk to their support and learn that you can't even create a template page for your blog posts?

Even for a software developer, balancing convenience and customizability isn't easy, especially for a hobby project. For an ongoing project, you want something that can process new pages quickly and easily, separates design from content, and also allows you to make it your own. I've tried many things, from WordPress, to Drupal, to SquareSpace, to writing my own CMS using Django, and to just writing pure HTML using Bootstrap Studio. But the balance was always off.

Enter ["Hugo"](https://gohugo.io/), a static site generator that uses markdown to generate pages and easily lets you create your own theme to insert those pages into. Using Hugo, I wrote the design for this page fully from scratch. Now, I can create pages by simply writing markdown, while still being able to retroactively make changes to the design for all past, present, and future pages. The hugo-powered ["Vanilla.sh"](https://vanilla.sh/) website is my attempt at creating a sustainable long-term personal space, with content that can be easily placed into a different design when the need arises.

## A New Layout
As mentioned, I wrote the Hugo theme for this page, called "vanillash", completely from scratch. In the process, I used partials and shortcodes, Hugo building blocks that mimic similar concepts you might see in a modular frontend framework like React or Angular. Now, I won't pretend to have gone in-depth into these whatsoever. I barely learned enough about the syntax to achieve what I wanted to achieve, and I think in this case, that's fine. I'm not gonna become a Hugo engineer, I'm not gonna build 10 more pseudo-CMS websites for random companies, I'm just using this as a means to an end. So I don't expect to learn many more revelations about the sorcery that powers this machine. But I did get to play around with CSS a bit again, and that's neat. In the end, trying to fix some random WordPress header in a laggy WYSIWYG editor to look like 80% of how I want it to look just isn't as satisfying.

## A New Pipeline
I am using GitHub actions to directly deploy the master branch of this project's repository to the live Vanilla.sh website. This relatively simple CI/CD pipeline follows the same approach I took with my "Cockpit" project, although it directly deploys to a web host rather than GitHub Pages.

Generally, deployment pipelines and also containerization and container orchestration are topics that I might want to look at in more detail in the future. While I am generally much more interested in the development aspect of things, the ability to quickly and reliably bring a project online and push changes to the production environment without much manual setup is not only nice to have, but arguably crucial, especially in web projects.

## A New Beginning
At the end of the day, I can't fully predict how much content I will post on this website. As of now, it's relatively simple, with only a home page and a basic blog post setup. I hope to add several components to this website eventually, such as articles, project showcases, game resources, and more. If you're reading this 3 years from now, and it still looks the same - well, that means I probably failed.

In any case, I expect the blog posts to be a bit of a side project. I don't tend to blog _that_ much, and mostly only when I feel like I have something interesting to say. Although, I might try to add some pages where I go over problems I faced during the development of my projects, so I have a sort of "reference database" of issues that took a lot of time to solve. Maybe that would also help other people.

That's it for this initial blog post. Happy to be here and happy to have you. Welcome to Vanilla.sh!