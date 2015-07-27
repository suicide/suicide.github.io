---
title: Langauge conditional caching in symfony
layout: default
---

So I finally found a topic that was worth writing about: language conditional caching in symfony 1.4!

The symfony tutorial (Practical symfony) desribes i18n and caching in detail. So I thought: this is gonna be a piece of cake -> but NO ITS NOT :( . In their tutorial they set the language through a parameter in the url, like http://somewhere.com/en/something for english and http://somethere.com/de/something for german. But what if you keep you culture setting stored in the session?! Well then you get a little fucked.

Without caching everything works just fine. You can switch between languages and everything. But if you activate caching on an action or a partial everything is still ok on the first request. But if you change the language, something weird happens: you still see the FIRST language. Ok you think… maybe the browser cache… WRONG! its symfony’s cache. Just look in the cache folder of your project. There it is: all in english or whatever.

So what now? Ditch caching? that would be the easy way… I like the hard way… if you know what I am thinking about :P … ok first some google (or reading symfony documentation)… what do we find? Something to fix the whole thing for partials and components at least. You can do something like this in your template:

{% highlight php %}
include_component('news', 'latestPosts', array('sf_cache_key' => $sf_user->getCulture()));
{% endhighlight %}

Ok what does that do? It changes the key symfony uses to cache that component. So you don’t use that generated key anymore… big deal eh? Nice and easy… just great… NO! its not… I’d like to keep as much of the caching stuff as possible out of the templates. That way no damn templating idiot can break the caching.

#### NOW THE INTERESTING PART

So I took a closer look at the sfCache and the sfFileCache class: They use prefixes on the caches that usually contain parts of the url like the server, app, module, action, partial and component. YEAH! gotcha! ok but how do I change the prefix? Well the cache is configured in the factories.yml and there is also an option to change the prefix! Almost done! but again NO! FUCK… I can change the prefix but how do i change it dynamically with the user language? With spring I’d have an idea on that… but symfony… humpf… but then an IDEA popped! FILTERS

The symfony documentation showed how to configure the cache with a filter to do some weird caching stuff (didn’t read it all… but its in the gentle introduction thing). So I created a LanguageCacheFilter class (awesome name… right?!)… So lets have a look at it. This is the execute method:

{% highlight php %}
public function execute(sfFilterChain $filterChain) {
  if ($this->isFirstCall()) {
    $context = $this->getContext();
    $user = $context->getUser();
    // if cache is disabled (dev environment) this object does not exist
    if ($context->getViewCacheManager() != null) {
      $cache = $context->getViewCacheManager()->getCache();
      $cache->setOption('prefix', $cache->getOption('prefix') . '/'.$user->getCulture());
      // works only for sfFileCache
      $cache->setOption('cache_dir', $cache->getOption('cache_dir') . '/'.$user->getCulture());
    }
  }
  // execute next Filter in chain
  $filterChain->execute();
}
{% endhighlight %}

Pretty simple… I just change the prefix and the cache_dir for sfFileCache (because it is initialised sometime before the filter is executed) and add the language from the current user object. If you use sfApcCache or something you would prolly not need the cache_dir option, but maybe something else… okn ow just add it to the filter et voila!

{% highlight php %}
language:
  class: LanguageFilter
languageCache:
  class: LanguageCacheFilter
cache:     ~
{% endhighlight %}

So i execute the languageCache before the cache itself is executed… because… you know… and after I get the language from another languageFilter from the cookie. Ok its maybe not the perfect solution, because that filter is executed on every damn request… and that sucks… but this is the place for me because i have the language and the cache at the same time and pretty much at the beginning of the whole execution.

Ok that was my solution to the problem... IF YOU HAVE A BETTER ONE… GIVE IT TO ME! :)
