---
title: Workmeter - Track your working hours
layout: default
titleImage: /assets/workmeter.jpg
---

Over the last year some friends and I had been working on a little project we called workmeter. 
You can find it at [http://workmeter.net](http://workmeter.net)

The idea for the project came to us when each of us had to track his working hours manually either to figure out how 
much money he made busting tables or how much overtime he made by sitting at work all night lurking the <del>internetz</del> 
“working”. So we were like: lets do a quick software project (lol what fools we were back then…) on just one weekend 
without a concrete concept (we actually called that “agile”) with that cool php framework symfony 1 (no judging here… 
well maybe a little bit). Ok that kind of a shitty idea… as I said earlier it did not take us one weekend but a little 
more than a year to finish it up and release a “stable” alpha version. And after releasing a second alpha version it 
really works just great without any complaints now for months.

![How do you shower?][shower]

So what does it do? It tracks your working hours and can either show you how much money you have earned on a day, 
within a week, month or year if you are paid by the hour. If you like to track your overtime, it can do that for you. 
You just write down from when to when you worked on a day and workmeter will show you some statistics how much you 
worked and if you worked enough. You can also write down what you did in that time and copy that into your company’s 
internal time tracking tool. It has so many great features (thats why it took so long to finish and why it was so 
hard to do in php because testing this stuff was just horrible and symfony was not fitting for complicated stuff like 
crunching numbers on masses of data and stuff)… ok so many features I cannot write the all down here, but here are 
some more: great statistics (yes, there are DIAGRAMS!) planing your vacations and your sick days…

Ok I hope you go an checkout workmeter.net

At last a screenshot of this fine webapp:

![Workmeter Screenshot][wm-screenshot]

[shower]: {{ site.url }}/assets/shower.png
[wm-screenshot]: {{ site.url }}/assets/workmeter-screenshot.png
