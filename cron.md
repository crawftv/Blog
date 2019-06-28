---
description: Scheduling everyday tasks
---

# Cron

Cron is a Linux tool for scheduling scripts for running at certain times, any minute, hour, day, and more.

Start with the command `crontab -e` in your user.

## Honestly I couldn't make this work on my digital ocean server for my purposes. 

I am including it so if you haven't heard of it, this might give you a better idea of what cron is. \(It's a common term thrown around by people who know what it is, but doesn't actually come up a lot.\)  
Also, Understanding cron time is useful as it shows up in some scheduling libraries.

## Cron Time

Cron time is 'minute hour day-of-month month day-of-week.   
Asterisks \* indicate every such unit. So, `0 12   * * *` is everyday at 12 o'clock.



