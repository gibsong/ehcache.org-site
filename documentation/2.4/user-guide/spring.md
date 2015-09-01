---
---
# Using Spring and Ehcache

 

Ehcache has had excellent Spring integration for many years. More recently there are two new ways of using Ehcache with Spring

## Spring 3.1
Spring Framework 3.1 added a new generic cache abstraction for transparently applying caching to Spring applications.
It adds caching support for classes and methods using two annotations:

### @Cacheable
Cache a method call.
In the following example, the value is the return type, a Manual. The key is extracted from the ISBN argument using the id.

<pre>
@Cacheable(value="manual", key="#isbn.id")
public Manual findManual(ISBN isbn, boolean checkWarehouse)
</pre>

### @CacheEvict
Clears the cache when called.

<pre>
@CacheEvict(value = "manuals", allEntries=true)
public void loadManuals(InputStream batch)
</pre>

Spring 3.1 includes an Ehcache implementation. See the [ Spring 3.1 JavaDoc](http://static.springsource.org/spring/docs/3.1.0.M1/javadoc-api/org/springframework/cache/ehcache/package-summary.html).

It also does much more with SpEL expressions. See <http://blog.springsource.com/2011/02/23/spring-3-1-m1-caching/> for an excellent blog post covering this material in more detail.

## Spring 2.5 - 3.1: Ehcache Annotations For Spring
This open source, led by Eric Dalquist, predates the Spring 3.1 project. You can use it with earlier versions of Spring or you
can use it with 3.1.

### @Cacheable
As with Spring 3.1 it uses an @Cacheable annotation to cache a method. In this example calls to findMessage are stored in a cache
named "messageCache". The values are of type `Message`. The id for each entry is the `id` argument given.

<pre>
@Cacheable(cacheName = "messageCache")
public Message findMessage(long id)
</pre>

### @TriggersRemove
And for cache invalidation, there is the @TriggersRemove annotation.
In this example, `cache.removeAll()` is called after the method is invoked.

<pre>
@TriggersRemove(cacheName = "messagesCache",
when = When.AFTER_METHOD_INVOCATION, removeAll = true)
public void addMessage(Message message)
</pre>

See <http://blog.goyello.com/2010/07/29/quick-start-with-ehcache-annotations-for-spring/> for a blog post explaining its use
and providing further links.