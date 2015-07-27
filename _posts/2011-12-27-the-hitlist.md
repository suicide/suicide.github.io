---
title: The Hitlist
layout: default
titleImage: /assets/forever-alone.jpg
---

So in order to do the slenium jmx test thingy i talked about in the last post i needed to have some simple test app. 
so i thought just take 1 hour and do it… out of 1 hour (like usually) became like 5 hours… thanks to eclipse and 
maven (again, like usually) and the hideous of trying out spring 3.1 with java based configuration instead of using 
a web.xml… more on that topic later…

first: here is the awesome app [https://github.com/suicide/hitlist](https://github.com/suicide/hitlist) (yeah using git… 
how awesome is that? not at all…)

The app is basically a simple crud app with 1 “entity” and one service. The entity is not stored in a database but in a 
map within the service… best if you take a look yourself… just like 5 classes. however this should suffice to test the 
selenium + jmx test setup.

So why did it take so fucking long to setup this this time?

1. eclipse  + maven: so i used indigo for the first time for something “bigger” with a “propper” setup. So after 
installing all my plugins (spring tools, more unit, [m2e was already there as it is not part of eclipse], git, s
ubclipse…. blah) i created the app and wanted it to start in tomcat 7 (yeah servlet 3.0 … :( ). but there was no 
“run on server”. Where was it? last week i had the same problem at work and accused our student worker of being stupid 
:P (so even it was not his fault… still…)… so now i have the same problem… but i am not stupid… so something must be 
horribly wrong with eclipse and maven. so some google magic… voila: 
[http://stackoverflow.com/questions/6356421/maven-tomcat-projects-in-eclipse-indigo-3-7](http://stackoverflow.com/questions/6356421/maven-tomcat-projects-in-eclipse-indigo-3-7) 
apparently wtp is not installed 
by default… you have to get it through the awesome market place… ok… lame…
so after “update project configuration” i got it up and running

2. java configuration instead of web.xml: so with spring 3.1 and servlet 3.0 you can use java configuration instead of 
the web.xml. so worth a try… so some reading and some copy pasta from the javadoc almost ready…. some changes in the pom 
and some try and error and just giving up because i was bored (as you can see in the code) and it was not important right 
now…. so actually not much of a problem

hmm ok as i think about it… i dont know why it took so long… maybe shouldnt have watched tv and lurked the web… damn

so whats up next? write some selenium test and use jmx to add test data to the system…
