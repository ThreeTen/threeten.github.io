---
layout: article
title: LocalDateTime
subtitle: User guide for the LocalDateTime class
categories:
- articles
tags: local date time hour minute second nano schedule
---

The `LocalDateTime` class represents the date-time,often viewed as year-month-day-hour-minute-second and has got no representation of time-zone or offset from UTC/Greewich. 

The "local" part of the name refers to the local time-line. `LocalDateTime` class supports nanosecond precision. For example, the value "2nd October 2007 at 13:45.30.123456789" can be stored in a LocalDateTime.

Remember that `LocalDateTime` class does not store or represent a time-zone. Instead, it is a description of the date, as used for birthdays, combined with the local time as seen on a wall clock. It cannot represent an instant on the time-line without additional information such as an offset or time-zone.

#### Creating a LocalDateTime

There are a number of ways to create a `LocalDateTime`.

##### Current Date Time

Current date-time can be created using the following `now` methods:

{% highlight java %}
LocalDateTime a = LocalDateTime.now();
LocalDateTime b = LocalDateTime.now(ZoneId.of("Europe/Paris"));
	
LocalDateTime c = LocalDateTime.now(aClock );
{% endhighlight %}

The key point is that although LocalDateTime has got no representation of time zone, determining current date time requires knowledge of the time-zone.

The first method (a) uses the Java default time-zone, as per `TimeZone.getDefault()`.
The second method (b) allows the time-zone to be explicitly controlled.
The third method (c) uses a [`Clock`](clock.html) object, which provides further control over the current instant and time-zone. This will come handy in testing scenarios as we can provide an alternate clock. 

##### Using fields

If you want to hard code the creation of date-time, or have fields available, these factory
methods can be used:

{% highlight java %}

LocalDateTime a = LocalDateTime.of(2013, 12, 18, 14, 30);                            // 2013-12-18T14:30                 
LocalDateTime b = LocalDateTime.of(2012, Month.DECEMBER, 18, 14, 30);                // 2012-12-18T14:30                 
                                                                                                                         
LocalDateTime c = LocalDateTime.of(2013, 12, 18, 14, 30, 40);                        // 2013-12-18T14:30:40              
LocalDateTime d = LocalDateTime.of(2013, Month.DECEMBER, 18, 14, 30, 40);            // 2013-12-18T14:30:40              
                                                                                                                         
LocalDateTime e = LocalDateTime.of(2013, 12, 18, 14, 30, 40, 999999999);             // 2013-12-18T14:30:40.999999999    
LocalDateTime f = LocalDateTime.of(2013, Month.DECEMBER, 18, 14, 30, 40, 999999999); // 2013-12-18T14:30:40.999999999    
                                                                                                                         
LocalDateTime g = LocalDateTime.of(aLocalDate, aLocalTime);                                                              
LocalDateTime h = LocalDateTime.ofInstant(anInstant);
                                     
{% endhighlight %}

The first two methods, (a) and (b), creates the `LocalDateTime` from a combination of year, month, day, hour and minutes. In this case, seconds and nanoseconds are set to zero. 

The methods, (c) and d), creates the `LocalDateTime` from a combination of year, month, day, hour, minutes and seconds. In this case, nanoseconds are set to zero.

The methods, (e) and f), creates the `LocalDateTime` from a combination of year, month, day, hour, minutes, seconds and nanoseconds.

Note that you can use the type safe enum Month.December instead of numeric value 12 for month field as shown in  (b)(d) and (f).
Valid numeric value for month is 1 - 12 and for hour is 0 - 23 and for minutes/seconds is 0 - 59. If an invalid value is passed in, say 25 as value of hour, 61 as value of minute/second, 13 as value of month etc, then an exception is thrown. 

The method (g) creates the `LocalDateTime` from a combination of [LocalDate](local-date.html) and [LocalTime](local-time.html).

The method (h) creates the `LocalDateTime` from an [Instant](instant.html)

In addition to these methods there is another factory method called LocalDateTime.ofEpochSecond which allows the epoch-second field to be converted to a local date-time. This is primarily intended for low-level conversions rather than general application usage. Eager users can look into [Javadoc](http://threeten.github.com/threetenbp/apidocs) for more details. 

##### By Parsing Text

{% highlight java %}

LocalDateTime a = LocalDateTime.parse("2013-12-18T14:30");                        // 2013-12-18 14:30              
LocalDateTime b = LocalDateTime.parse("2013-12-18T14:30:40");                     // 2013-12-18 14:30:40           
LocalDateTime c = LocalDateTime.parse("2013-12-18T14:30:40.999999999");           // 2013-12-18 14:30:40.999999999 
LocalDateTime d = LocalDateTime.parse("2013-12-18T14:30:40.100");                 // 2013-12-18 14:30:40.100000000 
LocalDateTime e = LocalDateTime.parse("2013-12-18T14:30:40.010");                 // 2013-12-18 14:30:40.010000000 
LocalDateTime f = LocalDateTime.parse("2013-12-18T14:30:40.001");                 // 2013-12-18 14:30:40.001000000 
LocalDateTime g = LocalDateTime.parse("2013-12-18T14:30:40.000100");              // 2013-12-18 14:30:40.000100000 
LocalDateTime h = LocalDateTime.parse("2013-12-18T14:30:40.000010");              // 2013-12-18 14:30:40.000010000 
LocalDateTime i = LocalDateTime.parse("2013-12-18T14:30:40.000001");              // 2013-12-18 14:30:40.000001000 
LocalDateTime j = LocalDateTime.parse("2013-12-18T14:30:40.000000100");           // 2013-12-18 14:30:40.000000100 
LocalDateTime k = LocalDateTime.parse("2013-12-18T14:30:40.000000010");           // 2013-12-18 14:30:40.000000010 
LocalDateTime l = LocalDateTime.parse("2013-12-18T14:30:40.000000001");           // 2013-12-18 14:30:40.000000001 
 
                                        
LocalDateTime m = LocalDateTime.parse("2010 12 18 14 30 40", aDateTimeFormatter); // 2010-12-18 14:30:40    

// where DateTimeFormatter aDateTimeFormatter = DateTimeFormatters.pattern("y M d H m s");

{% endhighlight %}

The first twelve methods, (a) to (l), creates a `LocalDateTime` from text of the format yyyy-mm-ddThh:mm:ss.SSSSSSSSS. Second and nano-of-second parts of text are optional and you can safely omit the trailing zeros of nano-of-second.
The final method (m) uses an additional `DateTimeFormatter` which specifies the format of text used.

### Note
When using toString() method of `LocalDateTime`, remember that it prints either 0, 3, 6 or 9 digits for nano-of-second field, depending on the value. So for example 2013-12-18T14:30:40.100000000 will be printed as 2013-12-18T14:30:40.100 .