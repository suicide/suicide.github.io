---
title: The Pain of Test Data in Integration Tests
layout: post
titleImage: /assets/genius.jpg
tags: [java, testing]
---

So imagine you have some kind of a web app that you’d like to test using selenium or canoo webtest… well you better do 
not use webtest because it looks great at first… but its a fucking pain later… especially if you created your tests 
using the xml definitions. You have no way to validate if your configuration is valid… and you just have hundreds of 
those files…

however selenium is different… you can run the tests using junit… so its easy to integrate them into your nightly build. 
But there are enough posts around that cover how you can use selenium…

I’d like to think about test data… the data you want to inject into your application for your tests to run on.

There are some requirements towards test data that are not easy to fulfill:

* Sometimes you need large masses of test data (for example if u want to test your paging)
* Sometimes you just need a small set of data
* your test data has to be valid
* the data needs to be easy to handle
* the data needs to be easy to extend

This is all that i could think of right now… but we also have different roles we need to consider if we want to design 
our test data: developers and testers… <del>damn testers…</del>

As a developer i want to write my selenium test as fast and easy as possible… just like a unit test. but that is not 
so easy as we are trying to do an integration test… so there are so damn mocks… just the real thing… at least somehow. 
And a real tester does have a big concept for his tests and his test data… well at least i hope he does… and he probably 
has masses of test data that is specified to the last bit so he can test each use case with this data set. (Thats just 
what i imagine from a test… so prove me wrong…)

So is there a way to bring these to worlds together? I dont know… but as a developer i’d like to follow the KISS 
principle… for you dummies: keep it simple stupid

First of all: I create test suites for each component of my application and each test suite is provided with its own 
set of test data… well maybe there is some basic data each test needs that we need to provide globally but thats up 
to the app. So this way we keep the test data sets simple. we only provide the data we need for the test just as you 
would do in a unit test. for some tests we just need 1 or 2 test object and others need 20 objects but not the same as 
other tests. This way we keep the data sets from interfering with each other (changing data for test A will cause test 
B to fail because the some fucking objects pop up and we have to just change some counter from 8 to 9 just because of 
that without any sense of knowing if this is even ok..)

Ok but what if we need like 100 similar test objects to do something? – well first i am thinking of creating them just 
in java… we’ll see…

And how to insert the data into the system? and how can we guarantee that the data is valid? SQL inserts? But what if 
we do now have a SQL DB? – How a about providing an API to insert the data through the application itself… maybe a 
REST api or a webservice… but if our app does not have one? build one?…. what about JMX? we can export out service 
interfaces to the tests and insert the data directly through our services. and with Spring it is just some small 
configuration we need to add and voila… our selenium test client can access the services and insert data and even 
use the applications entities or model objects. and if the application is not in test mode we just do not load this 
configuration.

Sounds easy? yep… thats what i am thinking… so lets try it… I will build a simple web app and test it via selenium and 
inject data through jmx
