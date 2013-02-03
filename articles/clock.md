---
layout: article
title: Clock
subtitle: User guide for the Clock class
categories:
- articles
tags: clock time instant now
---

The `Clock` class provides access to the current instant, date and time using a time-zone. 

Use of a Clock is optional. All key date-time classes also have a now() factory method that uses the system clock in the default time zone. The primary purpose of this abstraction is to allow alternate clocks to be plugged in as and when required. 
Applications use an object to obtain the current time rather than a static method. 
This can simplify testing.
The system factory methods provide clocks based on the best available system clock This may use System.currentTimeMillis(), or a higher resolution clock if one is available.

Best practice for applications is to pass a Clock into any method that requires the current instant. A dependency injection framework is one way to achieve this:

{% highlight java %}
  public class MyBean {
    private Clock clock;  // dependency inject
    ...
    public void process(LocalDate eventDate) {
      if (eventDate.isBefore(LocalDate.now(clock)) {
        ...
      }
    }
  }
 
 {% endhighlight %}
 
This approach allows an alternate clock, such as fixed or offset to be used during testing.

#### Creating a Clock
{% highlight java %}
Clock a = Clock.system(ZoneId.of("Europe/Paris"));
Clock b = Clock.systemUTC();	
Clock c = Clock.systemDefaultZone(); // causes dependency to the default time-zone into your application
Clock d = Clock.fixed(Instant.now(), ZoneId.of("Europe/Paris"));
Clock e = Clock.offset(Clock.system(ZoneId.of("Europe/Paris")), Duration.ofSeconds(2));
{% endhighlight %}
 
The first 3 methods, (a)(b) and (c), obtains a clock that returns the current instant using best available system clock.This may use System.currentTimeMillis(), or a higher resolution clock if one is available. 
 
In first method, (a), conversion from instant to date or time uses the specified time-zone.

In Second method, (b), conversion from instant to date or time uses the UTC time-zone. This is preferrable to using systemDefaultZone method.

In Third method, (c), conversion from instant to date or time uses the default time-zone. Using this method hard codes a dependency to the default time-zone into your application. It is recommended to avoid this and use a specific time-zone whenever possible. The UTC clock method (b) should be used when you need the current instant without the date or time
 
The fourth method, (d), obtains a clock that always returns the same instant. As such, it is not a clock in the conventional sense.The main use case for this is in testing, where the fixed clock ensures tests are not dependent on the current clock.
 
The fifth method, (e) obtains a clock that returns instants from the specified clock with the specified duration added.
This clock wraps another clock, returning instants that are later by the specified duration. If the duration is negative, the instants will be earlier than the current date and time. The main use case for this is to simulate running in the future or in the past.
A duration of zero would have no offsetting effect. Passing zero will return the underlying clock.

