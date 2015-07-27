---
title: Solr, MS SQL Server and the damn date format
layout: default
titleImage: /assets/ffffuuuu-hang-nail-comic-rage-comic-hangnail.png
---

So I thought I had a simple task at work for the last couple of days: get the delta import feature of Solr to work!

And of course everything took just 10 times longer than expected, but I should be used to that by now. 
After all I am already in this business for like 5 years… whatever.

After upgrading our Solr installation the delta update was working fine on my local mysql datasource. 
So I upgraded and reconfigured one of our test installations using MS SQL server and voila -> indexing failed, 
SqlException. Ok the date format was not correct. Well who would have guessed that sql server is doing everything 
different from everybody else…

So a little googling on how to change the accepted date format in sql server -> no luck here (well actually there was 
so much stuff to read that I just gave up at once). Next step: check the !awesome Solr documentation. Ok there is a 
formatDate function that just uses SimpleDateFormat (yay). So just hack everything in that awesome sql query 
(${dataimporter.functions.formatDate(dataimporter.last_index_time, ‘yyyy-dd-MM HH:mm:ss’)} or something like that 
[but check that stupid dateformat sql server seems to get]) and go! meh…. a god damn null pointer 
(as if i didnt have enough of those <del>bastards</del> before upgrading Solr) [It seems like the functions "namespace" 
is empty or does not exist... whatever i dont care anymore]. And after hours of searching the internets, 
the great documentation and even reading the code… nothing.

So back to sql server and doing some quick and dirty (t-)sql <del>shit</del> coding… CONVERT(datetime, ‘${dataimporter.last_index_time}’, 120)

Ok thats it… just wanted to blow off some steam… just spend almost a complete day on this… just to do things properly… damn
