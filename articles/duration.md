---
layout: article
title: Duration
subtitle: User guide for the Duration class
categories:
- articles
tags: Duration time-line time instant 
---

The `Duration` class represents a duration between two instants on the time-line.
 
A physical duration could be of infinite length.
The duration is measured in "seconds" with nanosecond precision, but these are not necessarily identical to the scientific "SI second" definition based on atomic clocks. This difference only impacts durations measured near a leap-second and should not affect most applications.

#### Creating a Duration

##### As difference between two instants
Common usage of creating a duration is to give two instants and find the duration between them. 

{% highlight java %}
Instant start = Instant.now();
Instant end = Instant.now().plusSeconds(5).plusNanos(2);
Duration a = Duration.between(start, end);                // PT5.000000002S
{% endhighlight %}

When end-Instant is earlier than that of start-Instant, the duration will be negative 

{% highlight java %}
Instant start = Instant.now();                                                  
Instant end = Instant.now().minusSeconds(5).minusNanos(2);                      
Duration b = Duration.between(start, end);                // PT-5.000000002S
{% endhighlight %}

##### Using factory methods

Duration object can also be obtained using the following factory methods. 

{% highlight java %}
Duration a = Duration.of(1, ChronoUnit.MINUTES); // PT60S           
                                                                    
Duration b = Duration.ofNanos(1);                // PT0.000000001S  
Duration c = Duration.ofMillis(1);               // PT0.001S        
Duration d = Duration.ofSeconds(1);              // PT1S            
Duration e = Duration.ofSeconds(3, 1);           // PT3.000000001S  
Duration f = Duration.ofMinutes(1);              // PT60S           
Duration g = Duration.ofHours(1);                // PT3600S         
Duration h = Duration.ofDays(1);                 // PT86400S        
                                                                    
Duration i = Duration.parse("PT1S");             // PT1S            
{% endhighlight %}

The first method, (a), obtains a duration from the value of `ChronoUnit` and the type passed. In this example we are passing one minute which is equivalent to 60 seconds. 
 
The second method, (b),  obtains a duration from a number of nanoseconds. Nanoseconds is one billionth of a second and hence 1 second = 1000,000,000 nanos. 

The third method, (c), obtains a duration from a number of milliseconds. 1 second = 1000 milliseconds.

The fourth method, (d), obtains a duration from a number of seconds. Here, the nanoseconds is set to zero. 

The fifth method, (e), obtains a duration from a number of seconds and an adjustment in nanoseconds. 

The sixth method, (f), obtains a duration from a number of minutes. Each minute is 60 seconds. Here, the nanoseconds is set to zero.  

The seventh method, (g), obtains a duration from a number of hours. Each hour is 3600 seconds. Here, the nanoseconds is set to zero.  

The eighth method, (h), obtains a duration from a number of days. Each day is 86400 seconds which implies a 24 hour day. Here, the nanoseconds is set to zero.  

The ninth method, (i), obtains a duration by parsing the text passed. This will parse the string produced by toString() which is the ISO-8601 format PTnS where n is the number of seconds with optional decimal part. The number must consist of ASCII numerals. There must only be a negative sign at the start of the number and it can only be present if the value is less than zero. There must be at least one digit before any decimal point. There must be between 1 and 9 inclusive digits after any decimal point. The letters (P, T and S) will be accepted in upper or lower case. The decimal point may be either a dot or a comma.
