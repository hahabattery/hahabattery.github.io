---
layout: page
title: Redis
description: I will summarize the data types that I dealt with while using Redis.
permalink: /datastore/redis/datatypes
author: wonchang
category: datastore
date: 2024-02-05 10:43:22 +0900
---



# Sorted Set

### score
According to the [Redis documentation](https://redis.io/commands/zadd/), a sorted set score is:

the string representation of a double precision floating point number.

[A double-precision floating point number](https://en.wikipedia.org/wiki/Double-precision_floating-point_format)

allows the representation of numbers between 10−308 and 10308, with full 15–17 decimal digits precision.

### tiebreaker
Basically, Redis Rank (ZSet) sorts based on the score, and if there are tiebreakers, it sorts again based on the value. Redis does not provide support for other tiebreaker criteria, so it needs to be handled at the server application level.




