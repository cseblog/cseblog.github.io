---
title: Things need to be settled before starting code when we start a project
layout: post
author: Andrew HQT
category: Tieng Viet
published: true
---
### Things need to be settled before starting code when we start a project

When we decide to start a project means that we (bosses) already know what we need in generally. 
If I'm a boss then below things I will care:

- Firm a team: PO -> Dev Team -> Scrum Master
	- Choose PO, setting clear vision for him, asking him propose a draft bussiness requirement
	- To firm Dev Team, I will choose on Tech Lead carefully and asking him firm a good dev team
	- Then choose a Scrum Master

If I was a boss, above steps are things I would do to start my project. But If I was a tech lead below steps are what I will disscus to my star dev team:

- Drawing a general architech diagram, the diagram has to be detailed enough to:
	-  Answer all of questions from PO's bussiness requirements
	-  Which technologies or frameworks we may use to implement services
	-  Asking star devs starting POC all uncertantly asnwers which are not proved clear by arguments
	
- After setting foundation of architechture, starting settle team developing culture:

	- How to branching in Git or Git convention ? -> Documenting the agreement
	- Setting code checkstyle rules -> implementing in development 
	- How team do unit test -> Should we writing unit testing description (input/output) before coding
	- How to logging convention / How to code exception in general -> which framework and convention should we apply for logging
	- Which IDE team should use -> Inteliji, Eclipse, Netbean.... all team members should use the same tool to make documment cohesion. 
	- Namming Convention -> How to name of a service, any generally rules
	- How code review process is defined? Should be peer review? or crossed team review? or only team lead review?
	- How team do collaboration -> Pair programming? or XP or solo ownership??

After all above things set up, then we start the sprint 1 and starting analyse sprint's user stories, coding, testing and fixing bugs. You may be care one more thing is QA. How QA do testing all of features. They may reusing Unit testing but it is okay. The star dev team only need to create Unit test focus on main bussiness logic. 
