+++
date = '2025-03-22T20:00:00+01:00'
title = 'Nya.GG - Imagehosting'
tags = ['Spring Boot', 'JSF', 'Java', 'Nya.GG']
+++
## Nya.GG
In recent weeks, I've been working on Nya.GG in order to finally find a use for my most important domain name. A few days ago, I finally found the time to finish and deploy the minimum viable product build, a simple web application for administering user roles, creating API keys, and uploading and retrieving images via a simple API. The project is now published on [Nya.GG](https://nya.gg/). In this article, I want to talk a bit about the technology stack and my thoughts on the project so far.

## Project Setup
The setup for this project in itself goes against quite a few of my recent core principles. Instead of the more modern Flutter framework, I am using a rather old frontend technology stack in the form of JSF. In contrast, the backend is a Spring Boot-based API, which is more in line with what I want to use Java for moving forward. User data is stored on a PostgreSQL database hosted on [Neon.tech](https://neon.tech), and images are stored on Amazon S3.

Much of this technology stack models a slightly more modern version of what I use at work as a Java full-stack developer. I wanted to know how these tools would hold up in a personal project, and improve my understanding of annotations and features provided by Spring versus those developed by our architecture team. However, moving forward, I would likely decouple the front- and backend more, as is customary in a modern web application, and use an authentication provider like Keycloak rather than writing my own authentication system.

## Authentication: Reinventing The Wheel
This time around, I opted against using a third-party authentication provider, such as Auth0 or Firebase. Instead, I decided to implement my own session-based authentication system based on Spring. On registration, the user's hashed and salted password is stored in the database. On login, two things happen:
- A session-scoped, serializable Spring component holds information about the session that can be easily accessed through dependency injection by other services that need to check the current user's authentication and authorization status.
- A custom authentication token, which overwrites the Spring Security AbstractAuthenticationToken, is provided to the SecurityContext of the spring application. This authentication token enables the security configuration to allow or deny specific HTTP requests depending on the user's authentication status and roles.

Overall, it's a relatively simple setup that makes heavy use of the tools provided by Spring. I've managed to learn a thing or two about how Spring does CSRF as well, by implementing a CSRF repository that can be injected into forms and stays compatible with session serialization and deserialization.

## JoinFaces, PrimeFaces, and JSF
[JoinFaces](http://joinfaces.org/) is a project that enables Jakarta Faces (formerly JavaServer Faces) to run on Spring Boot. In newer Spring Boot projects, it appears that the default rendering engine is instead [Thymeleaf](https://www.thymeleaf.org/). As mentioned previously, the reason I wanted to incorporate JSF is mainly for its comparability to my work tech stack, otherwise, it seems like Thymeleaf would do the job just fine.

One quirk I noticed from the integration of JSF with Spring Boot is that it allows us to mix and match both JSF and Spring dependency injection with annotations like @scope("view") versus @ViewScoped. This can get a bit confusing if you're not already used to the concrete logic of these annotations. I've also had some issues with serialization and deserialization, where the type of annotation could impact the (in)ability of the framework to properly re-inject transient services into session-scoped beans after deserializing a stored session. In general, I've tried to stick to Spring annotations rather than JSF, and with some minor hiccups, it seems to have worked out mostly fine.

Another way in which I've managed to excellently self-sabotage this project is by adding [PrimeFaces](https://www.primefaces.org/) into the mix. PrimeFaces is a UI component library for JSF, my personal experience with which mainly amounts to having to try and figure out why an inherently bugged date picker component wasn't working way back in a university group project. On the surface, it's not a terrible product. It's meant to simplify building UIs, but I've not had too great of an experience with it. For starters, it gets kind of confusing mixing the different PrimeFaces and JSF components, but that may be due to my personal lack of experience using this suite of tools. I've also found that it's rather complicated to understand which components work well with each other, although the documentation for PrimeFaces is really good, so this is probably mostly a result of my trial-and-error approach rather than a systematic approach to reading through the documentation.

One problem that I've consistently had with PrimeFaces since my university days though, is that it tends to "swallow" errors and not notify the developer, making it much harder to debug issues when they arise. I also don't particularly like how unwieldy the generated HTML becomes when using PrimeFaces, which leads to the customizability of the website and its components being much more challenging than it needs to be, despite the styling tips mentioned in the documentation. Overall, would I use PrimeFaces again? Maybe, if I had to do another JSF project and had the time and willpower to properly look through the documentation. But I would probably stick to Thymeleaf or just avoid writing a Java-based server-side rendering frontend to begin with, to be honest...

## Amazon S3
Where do you store data in 2025? In the cloud, of course! Well, I guess I didn't want all my image hosting to rely exclusively on a 5-euro Hetzner web server (which, on top of that, is also technically in the cloud...), so I added S3 to the mix.

Now, AWS as a "solo developer" is always kind of a pain. Starting with them randomly banning my account one or two years ago, to the console that requires a PhD degree to operate it, to the best practices of having to set up a whole IAM center with groups and roles just to avoid giving an API root access to your account.

When setting up AWS, I tried following the [instructions for the Java 2.x AWS SDK](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/setup.html), which also recommend configuring an IAM identity center to use single sign-on. Of course, this does not really work in production due to having to manually refresh the keys by logging in every once in a while. Instead, I believe there is some wild setup using "IAM Roles Anywhere" to access AWS credentials programmatically from outside sources, but honestly, unless someone pays me to do an AWS course, I'll just stick to an access key, thank you very much.

To minimize the load on S3, requests to my app cross-check the database to see whether the file is actually expected to exist on S3, and there is some very basic caching implemented using [Caffeine](https://github.com/ben-manes/caffeine) to reduce the load of consecutive requests for the same URL, which might occur initially after sharing an image publicly.

## Deployment
Nya.GG uses a similar deployment pipeline as Vanilla.sh through GitHub actions which builds the repository and uploads the .jar file to my web server, where the deployment user maintains the application as a service. I am by no means a deployment expert, but what I am trying to do is keep the deployment user away from sudo permissions, which in this case meant having to look up how to run the service as the user and keeping the user's systemd instance alive throughout server restarts using lingering.

The spring boot app is then served through Nginx as a reverse proxy. Since Nya.GG relies heavily on subdomains (similar to other image-hosting platforms like [s-ul.eu](https://s-ul.eu/)), I also had to set up my SSL certificate for wildcard subdomains. Generally, I use [Let's Encrypt](https://letsencrypt.org/) to generate these certificates. While Let's Encrypt's tool, Certbot, can automatically generate certificates without manual user intervention using an HTTP-01 Challenge, this is not the case for wildcard certificates, which I had to manually set up with an ACME (DNS-01) challenge. Supposedly, this can be circumvented by providing Certbot with API access to the DNS settings for the domain, which I might set up in the future to avoid having to manually renew these certificates.

## Conclusion
I do think this is enough of an overview of Nya.GG so far! Give the website a try, and don't be afraid to shoot me an e-mail with some feedback if you want! I will continue to finalize the image hosting feature in the near future and iron out some bugs, before presumably moving back to Cockpit or a similar related project that I've been cooking up.

~ Vanilla