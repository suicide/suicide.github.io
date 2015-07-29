---
title: Type Include Taglib
layout: post
titleImage: /assets/lonely-nerd.gif
tags: [java, programming]
---

So remember some time ago when I posted something about a tag lib that includes other JSPs be the type of the object 
passed to it? – Problably not… so look at [this]({% post_url 2011-05-19-object-rendering-jsp-taglib %}) and 
[this]({% post_url 2011-05-21-started-that-object-rendering-taglib %}). After months of doing nothing on this topic I finally did 
something! ..yay

I created a taglib that is very much like CoreMedia’s cm:include tag and published it on [github](https://github.com/hastybox/type-include-taglib) 
as well as pushed it to [maven central](http://search.maven.org/#artifactdetails%7Ccom.hastybox%7Ctype-include-taglib%7C0.2.0%7Cjar).

![Simple document model][simple-model]

So what is the purpose of this shitty JSP taglib? Well as it says it includes other JSPs based on the type of a given 
object. Lets imagine you have the following class diagram: Document Model

Lets have a view at the media -> image/video relationship. Imagine you have a Media object in your JSP but you do not 
actually know whether it is an image or a video. And obviously you cannot render them in the same way. The image needs 
an <img> tag and the video some flash or html5 shit. So, what do? – add a method to Media that says what kind of object 
it is or even worse a method that is able to render html? Or change you class model? Well, there are a thousand ways to 
mess this up. Wouldn’t it be nice to have something that just renders the correct template without you having to change 
your classes? That is exactly what this taglib does!

You pass your media object to the taglib and it looks at the actual type and tries to find a fitting JSP along your 
class hierarchy to render the object.

What does that exactly mean? – Ok stupid I’ll expain:

In the standard implementation of the taglib it will try to render or find JSPs by the following rules:

1. Search for JSP named after the actual type of the current object.
1. if none found, search for JSP named after the interfaces of the object.
1. if none found, search for JSP named after the super class of the object.
1. if none found, search for JSP named after the interfaces of the super class of the object.
1. and so on…

Here some examples: imagine you have the following JSPs defined to render your objects:

* Video.jsp
* Media.jsp
* Teasable.teaser.jsp
* Object.teaser.jsp
* and you have following code in some other JSP:

{% highlight xml %}
<typeinc:include self="${object}" />
{% endhighlight %}

object insanteof Video => renders Video.jsp

object instanceof Image => renders Media.jsp, because there is no Image.jsp

There is also a “template” parameter that lets you decide which template to render. If you have the following code in 
your JSP:

{% highlight xml %}
<typeinc:include self="${object}" template="teaser" />
{% endhighlight %}

object instanceof Image => renders Teasable.teaser.jsp

object instanceof String => renders Object.teaser.jsp

 

I hope somebody understands this shit I just posted, because I am very tired right now and have to get up really early 
tomorrow and i have to play battlefield 3 now, just because. For more details have a look at the readme on github at 
[https://github.com/hastybox/type-include-taglib](https://github.com/hastybox/type-include-taglib)

[simple-model]: {{ site.url }}/assets/simple_document_model.png
