---
title: How to set up a project team
layout: post
author: Andrew HQT
categories: Engineering
published: true
tags: English
---
### How to set up a project team

![startup-life]({{site.baseurl}}/images/startup-life.png)<br/>

When we decide to start a project means that we (bosses) already know what we need in general. If I were a boss then below are things I will care about:

- Firm a team: PO (Product Owner) -> Dev Team -> Scrum Master
    - Choosing PO, setting a clear vision for him, asking him to propose draft business requirements. This guy has to understand clearly the business, problems of customers, or stakeholders. 
    - To build a dev team, I will choose on tech lead carefully and asking him to firm a good dev team. A good tech lead will help you select and attract more good engineers. 
    - After all, I will find a Scrum Master. Why do we have to find Scrum Master in the end? Because Scrum Master is a critical role that helps to connect all people, all parties, inside and outside of the dev team. But he is not a team leader, he will not force a team to follow his own way, his role is to adapt to the above current team, trying to support the team, communicating with other sides, trying to track short-term goals, so we have to choose him later to make sure he aligns with the tech team, not tech team align to him. 

If I were a boss, the above steps are things I would do to start my project. But If I was a tech lead below steps are what I will discuss with my star dev team:

- Drawing a general architect diagram, the diagram has to be detailed enough to:
    
    -  Answer all of the questions from PO’s business requirements
    -  Which technologies or frameworks we may use to implement services
    -  Asking star devs starting POC (Proof Of Concepts) all uncertainly answers which are not proved clearly by arguments and debates
    
- After setting the foundation of architecture, starting to settle the team developing culture:

    - How to branching in Git or Git convention? -> Documenting the agreement
    - Setting code checkstyle/SonarLint rules -> implementing in development
    - How team do unit tests -> Should we writing a unit testing description (input/output) before coding
    - How to do automation testing -> should we start at the beginning or later on
    - How to logging convention / How to code exception in general -> which framework and convention should we apply for logging
    - Which IDE team should use -> IntelliJ, Eclipse, Netbean…etc. All team members should use the same tools to make document/code consistency. 
    - Naming Convention -> How to name of a service, and general rules
    - How coding review process is defined? Should be peer review? or crossed team review? or only team lead review?
    - How the team does coordination -> Pair programming? or XP or solo ownership??

After all the above things are set up, the final things that need to be considered are laptops, network, infrastructure...etc then we can start sprint 1 and starting analyze sprint’s user stories, coding, testing, and fixing bugs. 

You may be care one more thing is QA. How QA does testing all of the features. They may re-use Unit testing but it is okay. The star dev team only needs to create a Unit test focus on main business logic.

- From all of them, we should encourage the culture of "open debating, arguing and learning" in the team. Keep team spirit high.
