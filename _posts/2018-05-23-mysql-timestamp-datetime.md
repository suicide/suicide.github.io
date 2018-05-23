---
title: MySQL Timestamp at the beginning of time 
layout: post
titleImage: /assets/clevergirl.jpg
tags: [mysql, time, db, scala, slick]
---

I just started using slick with mysql in a new project and I came acress something that just made me meeeh hard...

So I had the following column defined:

{% highlight scala %}
def time = column[Instant]("TIME")
{% endhighlight %}

... and yes... I defined a custom mapping for Instant, fyi: 

{% highlight scala %}
implicit val instantColumnType: JdbcType[Instant] with BaseTypedType[Instant] = MappedColumnType.base[Instant, Timestamp](
    { i => Timestamp.from(i) },
    { d => d.toInstant }
  )
{% endhighlight %}

After having coded on my boring scala, finatra, slick  app for a couple of hours without even compiling because I was just so fucking wired in and everything was just
so goddamn going kind of ok... I compiled without any errors (!true story) and had everything running with my shiny new mysql8 db because I am so cutting
edge I often give myself paper cuts trying to feel something as I am so far gone....... yeah... and then:

{% highlight scala %}
com.mysql.cj.jdbc.exceptions.MysqlDataTruncation: Data truncation: Incorrect datetime value: '1970-01-01 00:00:00.0' for column 'TIME' at row 1
{% endhighlight %}

in the goddamn console... I was like: WADU? Y ME? :(

After some googles and overflowing stacks the actual solution is to define the column not as a MySQL TIMESTAMP but a DATETIME. And that is just the shortcut if you do 
not actually care about the timezone or whatever and just want to store some time that you got from somewhere... which is the case for me... yay!

this is it:

{% highlight scala %}
def time = column[Instant]("TIME", SqlType("DATETIME"))
{% endhighlight %}


But what is the problem? [this](https://stackoverflow.com/a/3467163/469254) helped. I tried to store a time that was before the beginning of mysql timestamp time.
Just by 1 fucking second but u kno... time is relative... and money... and sexy? Even the [mysql docs](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) 
reflect this little thingy that is just obvious...

But the wikipedia page on [timestamp](https://en.wikipedia.org/wiki/Timestamp) and [unix time](https://en.wikipedia.org/wiki/Unix_time) state that it counts the 
seconds since 1970-01-01 00:00:00 UTC... so DOES MYSQL NOT COUNT FROM 0 ??!?111? fu

