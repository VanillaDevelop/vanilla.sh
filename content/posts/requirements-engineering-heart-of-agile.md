+++
date = '2025-05-01T20:00:00+01:00'
title = 'Requirements Engineering: The Heart of Agile?'
tags = ['Agile', 'Requirements Engineering', 'Blog']
+++

## Introduction
Today I want to do something a bit different from my regular development updates, and just talk about a topic that has become increasingly relevant to my work: Requirements Engineering. Unfortunately, it is a topic that is very underappreciated in the industry and my workplace in particular. 

I had the chance to visit a requirements engineering workshop recently, and I want to share some of my thoughts on the topic here.

## "As a product owner, I want..."
First off, what even is a requirement? From a developer perspective, it's most often a ticket, a unit of work, which is presented to us in the form of some template, like the user story template:

> "As a [persona], I want to [software goal], so that [result]."

In our workshop, we were also presented with other templates, such as ISO 29148, EARS, and MASTeR. But the idea is often the same: Formulate the requirement as a simple statement. The user story template in particular forces requirements engineers to focus on one of the fundamental principles of requirements engineering: **value orientation**, by specifying the _result_ and verbalizing the _value_ that is generated through that result.

This can be a powerful thought exercise when used correctly. However, in many cases, it simply is not applied appropriately. Consider these three tickets:
- As a customer, I want to be able to print my report, so that I can access it offline at a later date.
- As a customer, I want to have the ability to communicate with my representative via a secure chat system.
- As a product owner, I want to know how our internal data control flow works, so that we can adapt it later.

The first ticket sounds nice, doesn't it? It's at the correct size where a developer can reasonably decide how to implement it, and quickly check the implementation against the requirement.

The second one - hold on a second. There are so many questions here. What type of chat are we looking to build? Is it real-time or asynchronous? Is the data store persistent? What about data safety regulations? What type of UI is required for the representative? Such a ticket, in my opinion, is an indication that the requirements engineer is not well-versed enough in the operational side of the business to appropriately gauge the scope of a ticket. This is a problem, and a big one at that.

And what about the third ticket? Why does the product owner not understand the product already? Is there a lack of documentation? This may be the case for legacy systems, but an analysis ticket like that does not actually follow value orientation. Customer-driven requirements which would lead to this part of the system being refactored should naturally be accompanied by a documentation process, so this issue should fix itself by the time it becomes relevant. However, I've seen instances where the customers would not actually provide requirements, leading to these types of tickets that facilitate a "self-driving agile team" in which the product owner hallucinates requirements based on the existing documentation.

It is my genuine belief that product owners should have enough of a technical understanding of the system to appropriately gauge the scope and viability of tickets. Otherwise, they cannot communicate and negotiate with the customer in good faith. However, this does not always seem to be the case, and it severely hurts the trust between the development team and the customer. A related concept, which I hate with a passion, is "problem space vs. solution space". Here, management roles will often take credit for identifying the _problem_, while deferring the implementation of its _solution_ to the development team. The principle, if applied correctly, is well-intentioned, placing the power of the specific technical implementation with the developer. However, if misused, it can quickly become ammunition for requirements engineers to refuse to play their part in finding the appropriate solution that matches customer needs, and allows for a lack of accountability if the problem itself is not well-defined by developer standards.

## Context is Key
Let's face it, developers are not known for their love of big bright creative rooms, flashcards, and mind maps. We're technical, logic-driven humans. Yet I will say that one concept from the requirements engineering workshop stuck with me: The context analysis. Our instructor did a good job hammering home its importance. For a new project, where we may only start with a vision or a small set of defined goals, a well-executed context analysis will make every subsequent decision much easier.

In requirements engineering, a context analysis leads to a nuanced understanding of the environment in which the system will operate, including:
- Stakeholders
- Actors
- External Systems
- Processes
- Documents
- Events

... and other sources which may interact with the system. Analyzing the system in this manner from a variety of viewpoints and perspectives really forces a person to think about all its nuances in detail and can be a great reference for building future requirements.

Now, here's the catch, from a practical perspective: Humans forget things. And they like to be lazy when it comes to piecing together things that they used to understand at some point. In theory, my company has a very neat, tidy, and semi-automated system for creating a context analysis that is complete, logically sound, and structured... In practice, however, I can no longer count on one hand the number of times when a 2-day requirements engineering workshop has ended with a few blurry pictures of a wall full of flashcards unceremoniously posted in an otherwise empty Confluence article. And that's a problem.

In the context (heh) of this chapter, we were also shown a variety of diagrams. University students will be well aware of most of these, including use-case-, context-, activity-, class-, and state machine diagrams. Referring to my previous point, I suppose the biggest issue is that I still haven't seen them much outside of university...

## Documentation (and Validation)
After some not-so-subtle critiques of our "better halves", I will also take some blame on the developer side of things: Developers do not like to document things. Like, at all. It takes time, it becomes outdated fast, and why document a feature when you can just look it up in the code, right?

On the other hand, it is simply not realistic to maintain a "digital twin" of a full-stack legacy monolith in the form of documentation. Most developers realize this, and opt for the easy way out - not doing it at all.

But at the end of the day, that's not the right approach. And there are some good reasons why developers should be documenting. Not least of which is the very real possibility that one day, half of your workforce might disappear in a freak accident and have to be replaced by young, naive, junior developers, who will then do the same thing 2 years down the line, perpetuating the vicious cycle.

Rather, maybe moving away from generic terms like "documenting the changes" and towards a more goal-oriented checklist that can be adapted into the definition of done would be a good idea. This checklist could include basic sanity checks on whether a major feature was changed in a significant way while leaving only brief comments on minor features. Of course, management and roles that naturally deal more with documentation can also facilitate the process more easily by keeping the space tidy and well-organized, to avoid duplicate and outdated entries. Here, too, it helps if requirements engineers and product owners are aware of the system's technical state, otherwise, this quickly becomes infeasible.

## Reality is Often Disappointing
I want to close this article with a more realistic, and maybe slightly more sobering outlook on requirements engineering. And I did find it quite funny that the workshop also touched on these things, meaning at least we're all aware that life is not a cakewalk. My key takeaways from this workshop were the following:

**1. Requirements engineering is an _active_ role.**  
It is the responsibility of the requirements engineer to ensure that requirements are well-rounded, understood by the whole team, and balanced between both stakeholder wishes and the technical capabilities and capacity of the team. Half-baked requirements or irresponsible promises don't just become the development team's problems, they are the result of errors in the process that should be ironed out over time.

**2. Active listening is a necessary skill.**  
While I'd like my requirements engineers and product owners to have technical knowledge, I don't expect any customers to do so. Customers don't always know what they want - they might just offer a solution in good faith, but really be trying to solve a completely different issue. Get to the root of the problem, and ask why they want things done a certain way. Make sure both parties are on the same page before proceeding. It'll save a lot of trouble down the line.

**3. Leverage elicitation techniques.**  
Probably the most powerful repertoire of tools available to a requirements engineer is elicitation techniques. The methods by which requirements can be generated are plentiful, and adapting them to the situation at hand is one of the most important things a requirements engineer can do. I don't just say this as a platitude - there _will_ be internal factors that influence how you _can_ construct requirements, and it won't be possible to just solve every problem with the run-of-the-mill 2-hour meeting. Use interviews, questionnaires, brainstorming, field observation and more where appropriate, and results will be much more plentiful.

**4. Plan around office culture.**  
In a role that's mostly about communication, you should quickly become aware of the office culture. Some people might respond to e-mails faster, some might take a call. Some might be able to join a meeting on quick notice, while others will accept an invitation two weeks out and still end up not attending. Really, this isn't so much a requirements engineering tip - it's a tip for any corporate role. But in requirements engineering, these skills can make or break a project. As a developer, watching from the sidelines as a customer-facing role attempts to set up the fourth meeting with a person who failed to show up for the last three can be nerve-wracking, to say the least.

**5. Validate and be transparent.**  
Finally, please, please, please, don't prove I'm right, and just be transparent with the customers. Software developers generally enjoy creating. We enjoy making useful products. We don't want to be driven into an identity crisis because the project scope continuously changes due to a mismatch between the vision of the team and that of the customer. At the end of the day, work is much more fulfilling when you are creating something with purpose. So make sure requirements are actually required. Make sure they have a purpose. Make sure the customer benefits. That'll improve everyone's life.

_Postscript: You might notice that in the writing of this blog post, I sometimes conflate the two roles of requirements engineer and product owner, as well as their responsibilities. This simply stems from the fact that these two roles are fulfilled by the same person in my workplace. Some points may not apply if you are only responsible for one of these roles, but putting yourself in the position of the other may still improve your overall perspective, similar to how this workshop led to me discovering new things about requirements engineering that I didn't know about before._
