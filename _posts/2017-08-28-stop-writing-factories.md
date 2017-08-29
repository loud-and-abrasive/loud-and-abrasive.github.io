---
layout: post
title:  "Stop Writing Factories"
date:   2017-08-28 10:13:00 -0600
tags: development patterns ioc factories
---

## (When you have an IoC container)

Coding patterns are nice. They provide a common language to convey ideas, foster familiarity between codebases, and _can_ keep code clean, ideally.

But they are also often introduced too early, implemented incorrectly, or more generally bent and abused. For this reason, it's as important to think about when not to use them as it is when to take advantage. One of the best times to not implement a pattern is when tools, libraries, or language features provide the pattern to you.

The factory pattern is one of the more commonly unnecessarily re-implemented of them. Let's look at why most of the time we don't need to reach for it directly. 

<!--more-->

[Factories](https://en.wikipedia.org/wiki/Factory_method_pattern) are most useful when objects require a consistent, maybe complex, but definitely controlled, construction method. When using them, you may see `FishFactory.Wrasse()`, instead of the more conventional `new Fish(wrasseOptions)` because `wrasseOptions` is long, mostly constant, or intended to be tightly scoped. There are plenty of valid use cases for a fully implemented factory, and you may find that writing a custom Factory class is necessary for your object construction. However, there are also cases where factory classes have been rendered irrelevant. The culprit I see most often is for database connections or sessions:

```c#
public class DatabaseConnectionFactory
{
    public Database GetDatabase()
    {
        var connectionString = Configuration.ConnectionStrings["myConnectionString"].ConnectionString;
        
        return new Database(new SqlConnection(connectionString), GetOptions());
    }

    private DbOptions GetOptions()
    {
        //yadda yadda yadda
    }
}
```

Fill in the above with the what-have-you for any application. A class that creates some Context, Connection, Session, or class wrapper around a data connection based on a constant configuration. 

More [than]() [one]() of my mentors often say that many of our tools and patterns are 
