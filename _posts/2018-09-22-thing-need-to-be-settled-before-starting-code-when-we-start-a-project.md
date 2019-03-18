---
title: Things need to be settled before starting code when we start a project
layout: post
author: Andrew HQT
category: Engineering
published: true
---
### Things need to be settled before starting code when we start a project


When we decide to start a project means that we (bosses) already know what we need in general. If I’m a boss then below things I will care:

- Firm a team: PO -> Dev Team -> Scrum Master
	- Choose PO, setting a clear vision for him, asking him to propose a draft business requirement.
	- To firm Dev Team, I will choose on Tech Lead carefully and asking him to firm a good dev team.
	- Then choose a Scrum Master.

If I was a boss, the above steps are things I would do to start my project. But If I was a tech lead below steps are what I will discuss to my star dev team:

- Drawing a general architect diagram, the diagram has to be detailed enough to:
	-  Answer all of the questions from PO’s business requirements
	-  Which technologies or frameworks we may use to implement services
	-  Asking star devs starting POC all uncertainly answers which are not proved clearly by arguments
	
- After setting the foundation of architecture, starting to settle team developing culture:

	- How to branching in Git or Git convention? -> Documenting the agreement
	- Setting code checkstyle/SonarLint rules -> implementing in development
	- How team do unit test -> Should we writing unit testing description (input/output) before coding
	- How to logging convention / How to code exception in general -> which framework and convention should we apply for logging
	- Which IDE team should use -> IntelliJ, Eclipse, Netbean…. all team members should use the same tool to make document cohesion. 
	- Naming Convention -> How to name of a service, and general rules
	- How code review process is defined? Should be peer review? or crossed team review? or only team lead review?
	- How team do collaboration -> Pair programming? or XP or solo ownership??

After all above things set up, then we start the sprint 1 and starting analyse sprint’s user stories, coding, testing and fixing bugs. 

You may be care one more thing is QA. How QA do testing all of features. They may re-using Unit testing but it is okay. The star dev team only need to create a Unit test focus on main business logic.

- From all of them, we should encourage to the culture of "open debating, argumenting and learning" in the team. Keep team spirit high.
