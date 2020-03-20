---
author: admin
date: 2014-07-20 19:44:15+00:00
draft: false
title: 'Sebastian Q. Canard: How to make friends and influence people'
type: post
url: /sebastian-q-canard-make-friends-influence-people/
---

In my previous post I described how I created a [cigar-loving twitter bot called Sebastian ](../making-twitter-bot/)using some Python code and a Raspberry Pi. Now that Sebastian has started tweeting, it's was time for him to start making some friends.

## The fastest way to make friends is to buy them

Friends on Twitter (as in life) can be bought for the right price. In Sebastian's case, I used the online marketplace [fiverr](http://www.fiverr.com/) (where people offer all kinds of questionable services for $5) to purchase 13,000 followers (these are bots like Sebastian, though with less panache). This gave Sebastian the kind of cache normally reserved for D-list celebrities, branded bathroom products and regional radio stations and (I hoped) would make new additions more likely to follow him back.

## Follow users who like to follow back

Having bought the first 13,000 friends, I wanted Sebastian to make some real ones. There are various projects online with sophisticated network-orientated, pattern-searching algorithms for following users. Sebastian's taste's aren't that complex however - all he wants to do is meet interesting people as efficiently as possible and build his list of followers by choosing people who are likely to follow back:

* **They have followers, but not too many:** Real people don't tend to have 50,000 friends and I wanted Sebastian to make friends with real people.
* **They have similar interests to Sebastian:** The [previous post](../making-twitter-bot/) in this series describes how Sebastian 'creates' his tweets by copying them from other people. The list of people he copies from forms the pool of potential users to befriend.
* **They like to follow other users:** The objective is to build Sebastian's social network, so a good potential friend is one who likes to follow others. Analysing the friends and followers list of every candidate would quickly exhaust my [rate limit](https://dev.twitter.com/docs/rate-limiting/1.1) so instead the script looks for users with a roughly 1:1 ratio of friends vs. followers.

## Add twitter followers, one at a time

The python script uses a random number generator to decide when a friend request is made. As with the tweeting, a daily profile is used so that Sebastian is more active at some times than others. A daily limit is in place to stop Sebastian making too many friends at once.

## How many twitter followers is 'too many'?

Time will tell, but for now Sebastian is [busy tweeting away](https://twitter.com/SebQCanard) and should be for some time. I'll revisit the project in a couple of months and see how successful the projects has been. In the meantime, the code is available on [GitHub.](https://github.com/AdamDynamic) If you have any questions feel free to [say hello](https://twitter.com/AdamDynamic).
