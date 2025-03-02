+++
title = 'Home'
+++

# About

{{< feature-grid >}}
{{< div class="col-twothirds">}}
Hi! I'm Georg. I'm a software developer from Austria, spending my free time tinkering with new technologies and building my own projects.

On this website, you can expect:

- A summary of my recent work
- Occasional updates about my personal projects
- Hobby-related resources
- Write-ups on topics I find interesting

{{< /div >}}
{{< div class="col-third">}}
{{< profile-picture src="/images/Profile.png" alt="Profile Picture" subtitle="Me on a good day" >}}
{{< /div >}}
{{< /feature-grid >}}

{{< timeline >}}
{{< timeline-item title="B.Sc. Computer Science" date="2021" >}}
Obtained my bachelor's degree in computer science from the University of Innsbruck, with a thesis focused on machine learning and data analysis.
{{< /timeline-item >}}
{{< timeline-item title="M.Sc. Software Engineering" date="2023" >}}
Obtained my master's degree in software engineering from the University of Innsbruck, with a focus on data science and scientific research.
{{< /timeline-item >}}
{{< timeline-item title="Thesis Publication" date="2024" >}}
My master's thesis was published in Findings of ACL 2024 and awarded the "Best Master's Thesis" award of the Department of Computer Science.
{{< /timeline-item >}}
{{< timeline-item title="Full-Stack Developer" date="2025" >}}
Working as a full-stack software developer in online banking, maintaining legacy systems and building new customer-facing solutions.
{{< /timeline-item >}}
{{< /timeline >}}

# Skills

I tend to pick up new skills and technologies quickly, but I don't always stick with them. Over the years I've played around with several dozen different programming languages, frameworks, and libraries. Recently, I've pivoted to a smaller toolkit, so I can spend more time mastering the intricacies of each tool and building things, rather than re-learning the same concepts with different accents.

{{< feature-grid >}}
{{< feature-item
    title="Flutter"
    image="/images/Flutter-logo.png"
    class="col-half" >}}
As a huge fan of rapid prototyping and building clean UIs for my applications, I've recently fallen in love with Flutter for building cross-platform frontend applications. In the future, I hope to pivot most of my frontend development to Dart and Flutter.
{{< /feature-item >}}

{{< feature-item
    title="Java"
    image="/images/Java-logo.png"
    class="col-half" >}}
I have a lot of exposure to Java EE for backend development in my current position, so Spring is a natural fit for quickly bootstrapping APIs. Additionally, Java-based servers can generally be deployed relatively painlessly. Hence, Java is a staple in my toolkit for backend development.
{{< /feature-item >}}

{{< feature-item
    title="Python"
    image="/images/Python-logo.png"
    class="col-half" >}}
Python is king when it comes to machine learning and data science. I've used it extensively during my master's program, and I still use it for data analysis and machine learning projects to this day. For visualizing and sharing results, Jupyter Notebooks are a great tool.
{{< /feature-item >}}

{{< feature-item
    title="C-based Languages"
    image="/images/C-logo.png"
    class="col-half" >}}
While not part of my daily toolkit, C-based languages like C, C++, and C# are a staple in various topics that interest me, such as game development and microcontrollers. Although I've played around with these languages in the past, they are mostly supplementary to my main toolkit.
{{< /feature-item >}}
{{< /feature-grid >}}

# Current Projects

Despite my better judgement, I tend to work on several projects at a time. Below is a selection of projects I've worked on recently or am currently working on.

{{< feature-grid >}}
{{< card-item
    title="Cockpit"
    subtitle="Flutter, Firebase"
    image="/images/Cockpit.svg"
    class="col-third"
    links=View|https://vanilladevelop.github.io/cockpit >}}
#### Order from Chaos

Cockpit is a life dashboard built in Flutter with a Firebase backend, focused on providing ways to improve your productivity at a glance. The first module, "Stream of Consciousness", is a tool to quickly capture and review your thoughts, and is live now.
{{< /card-item >}}

{{< card-item
    title="Nya.GG"
    subtitle="Java, Spring, Thymeleaf"
    image="/images/Nya.svg"
    class="col-third">}}

#### Catgirl-Powered Hosting

Nya.GG is a simple image and link distribution platform, built using Java Spring with a Thymeleaf frontend and a PostgreSQL database. It's meant to be a more customizable alternative to platforms like Imgur. Nya.GG is currently under development.
{{< /card-item >}}

{{< card-item
    title="Vanilla.sh"
    subtitle="Hugo, HTML, CSS"
    image="/images/Vanilla.svg"
    class="col-third"
    links=View|https://vanilla.sh >}}

#### Elegance in Simplicity

Vanilla.sh is a simple, elegant portfolio of my work. Additionally, it's a place where I can publish hobby-related resources and write-ups. It's built using Hugo, a static site generator, with a custom theme, and is continuously deployed via a CI/CD pipeline.
{{< /card-item >}}
{{< /feature-grid >}}
