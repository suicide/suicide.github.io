---
title: Selenium + JMX Conclusion
layout: post
titleImage: /assets/blown.png
tags: [java, programming, testing]
---

So I kind of “finished”… well I just got bored with the topic :P you can see the results at [github](https://github.com/suicide/hitlist).

Above all I can just say that the combination of selenium with jmx works fine although i havent tested very 
complex test cases and the data i injected was kind of trivial. However the setup was easy, so was the handling 
in the tests, as you just use the same api as you application. writing the page objects is a little pain but there 
are some solutions for that out there…

However there are some limitation to this construct using jmx. It looks like you are just using your api on your 
objects but for example the objects you want to use as method arguments have to implement Serializable. This kind 
of ugly if you actually don’t want them to be or there is no other reason for them to be serializable. Also if you 
make changes on the given objects in your api methods, those are not transmitted back to the test client through jmx. 
So if you need some values returned, you actually need them to be returned (see PersonService.save() in the example 
app). I also had to add api methods my app usually would not need (deleteAll method). So it is not actually just 
activating jmx and the fun can start… you have to adapt your app in order to make this construct work.

 

Open Questions:

* Whats up with the performance? In the simple tests the test execution was way faster than even starting the browser. 
But what if you need to insert masses of data
* This works fine for simple apps that have not too much data (a simple jdbc/jpa or something app that is actually 
just a crud app). What about apps that have more complex data or data storages? for example a coremedia content 
repository that might contains gigabytes of data, the data also contains application configuration and you do not 
even know how the content storage works internally… or what if your data need further processing or analyzing for your 
app to work
* what happens to sessions that depend on the test data that you might trash on each test. is there in selenium some 
kind of reset? other than restarting the browser?

More remarks:

* for complex data you would need more than a “deleteAll” method. You would probably create data initialization 
spring beans either in your app or your test client
* In the example, all test cases use the same spring configuration. If you have different test suites each would 
use another configuration including spring beans for data intialization
* in the example app i raped the maven test fork (src/test) for the selenium tests. in a real setup i would have 
created a new app just for the selenium tests. the test fork is just for actual unit test (at least in my option). 
you then have to include your app as a dependency in your test app or create a maven modules for your app code 
(the api you want to use) and include this one
* ….
