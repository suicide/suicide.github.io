---
title: Object rendering JSP taglib
layout: post
titleImage: /assets/facebook-fan.jpg
tags: [java, jsp, oo]
---

As of now I am a super awesome CoreMedia developer! Well ok I am not quite there yet, but I attended the Web 
Application Developer, Advanced Web Application Developer and the CAE Caching training over the last couple of weeks. 
I still have to prove myself in a real CoreMedia project, which might be happening sooner than later (at least I am told so).

Anyway, one thing that is quite nice in the CoreMedia CAE is the object oriented view dispatcher and the cm taglib, 
especially the cm:include tag. This whole mechanism makes use of the document model inheritance hierarchy 
(which is kinda gross for a poor web developer who is used to work with Pojos). If you dont know what I am talking 
about: all your Content Beans (you can roughly think of them as your entities) are related to each other through this 
inheritance hierarchy. The following awesome almost UML diagram (I used openOffice drawing for the first time) will 
make everything much more clear:

![Simple document model][simple-model]
Document Model

Ok when you use the object oriented view dispatcher it looks up JSP templates for your objects. So if you return an 
instance of an Article in your Spring controller it would try to find a Article.jsp. If it cannot find such a template 
the dispatcher will continue by searching for a Teasable.jsp and further to Document.jsp and at last try to find an 
Object.jsp. So it tries to find a JSP template that is named after the class of your object, an interface your object 
implements or a super class (just in that order). After i heard this I was like: ok that nice but I can also just 
return the name of my jsp... that is not much of an improvement.

Now come the trick: the cm:include tag works kind of the same way. Meaning you pass a object to the tag and it includes 
the appropriate template. Combined with with “views” you can easily create huge template structures (by passing a view 
string “render” the tag would look for a “Article.render.jsp”). So you can say tiles and its gross configuration and 
structure good bye!

So my question now is: Is there such a object include taglib or even a view dispatcher out there that works kind of 
the same?

[simple-model]: {{ site.url }}/assets/simple_document_model.png
