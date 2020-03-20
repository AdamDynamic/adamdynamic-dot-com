---
author: admin
date: 2014-06-14 10:08:17+00:00
draft: false
title: 'Sebastian Q. Canard: The cigar-loving twitter bot'
type: post
url: /making-twitter-bot/
---

This post describes how I attempted to build a social twitter bot. It runs on a [Raspberry Pi](http://www.raspberrypi.org/) and tweets automatically about his decadent life of cigars and moustaches. His name is Sebastian Q. Canard, he's a myth and you can follow him on Twitter [here](https://twitter.com/SebQCanard).

## Creating a Twitter bot

A couple of months back I found [a story from the Wired 2013 conference](http://www.wired.co.uk/news/archive/2013-10/17/kevin-ashton-quartz) about a guy called [Kevin Ashton](http://kevinjashton.com/). Kevin built a reasonably quick-and-simple twitter bot that had managed to quickly generate a [Kred](http://kred.com/) score of 754 (which is quite high, apparently). I've been playing around with the Twitter api for a while so I thought I'd see if I could beat Kevin's high score.

## Introducing Sebastian Q. Canard

{{< tweet 474665101367463937 >}}

<sub>@SebQCanard first tweet.</sub>

I chose the name 'Sebastian Q. Canard' pretty much at random with the only criteria being I wanted to him to have a Victorian moustache.

The basic strategy of creating (or rather, "creating") Sebastian's tweets is to copy other people's tweets and pass them off as if Sebastian had created them. The script scans twitter periodically for a list of keywords that represent Sebastian's 'interests'.

What these interests are I tweaked a couple of times but broadly speaking they are cigars, fine scotch and other things that interest the gentleman-rogue-about-town that I wanted Sebastian to be.

## Filtering the searched tweets

In order to make the Sebastian Q. Canard a welcome addition to people's Twitter feed, I wanted to make sure that the tweets selected were of a high calibre. I also wanted to make tweets reasonably consistent in tone.

To achieve this, all tweets returned by the search go through a pretty strict filtering process. It means that 99% of tweets get removed, but with [500 million tweets produced each day](https://blog.twitter.com/2013/new-tweets-per-second-record-and-how), there should still be enough to go around:

* **Banned Words:** Certain words are banned and any tweet containing them are automatically excluded. These include most swear words but also terms like 'lol' and 'OMG' which seemed out of character for someone named 'Sebastian' ("_OMG! This cigar is da bomb #Cohiba #LeatherBoundBooks")_
* **Gender-specific terms:** Sebastian's 'character' is male, so it might look odd if he tweeted about being pregnant
* **Links or hastags:** There's no easy way to control the nature of the links that were retweeted so rather than worry about filtering them for anything unsuitable I just banned them altogether
* **Direct tweets:** So as to not spam users by repeating other people's tweets back at them (and avoiding detection by having a user get an identical tweet from two different users)

Once the tweets have passed the filter, I clean up the tweets by capitalising the first letter, removing multiple spaces etc to generally tidy them up (Sebastian Q. Canard is someone who cares about correct punctuation).


## Selecting the tweets to re-tweet

The tweets that survive the filter are ranked according to criteria designed to determine the 'quality' of the tweet:

* **Retweeted or favourited tweets**: A good sign that a tweet is a good one is if other users have already promoted it. The number of times an eligible tweet has been retweeted is capped to prevent Sebastian accidentally retweeting a tweet that had already gone viral
* **The popularity of the user**: Popular users are assumed to be popular for a reason so the algorithm favours users who have lots (but not 'too many') followers
* **The length and content of the tweet:** Longer tweets are favoured over shorter ones (someone called 'Sebastian' should be able to fill 140 characters) and the percentage of words that match [a list of 1,000 common words](http://www.infochimps.com/datasets/word-list-1000-most-frequently-used-english-words-by-frequency-w--2) is included as a factor.

## Deciding when to tweet

Like most people, I assume that Sebastian has a job (maybe as a Haberdasher) so it's unlikely that he's going to be tweeting around the clock. Equally, it's going to be obvious that he's a bot if he only tweets 'on the hour' or at the same time every day.

To handle this, the script runs every 19 minutes and uses a random number generator to decide whether to tweet or not. The script includes a profile that makes it more likely that Sebastian will tweet at some times (in the evenings) than others (first thing in the morning).

## The next step

Sebastian is [up and running](https://twitter.com/SebQCanard) and producing tweets, the next step is to have him go out into the world and try to meet people. More on that in the near future...

## The Twitter bot code

The twitter bot is created using python and the [python-twitter](https://github.com/bear/python-twitter) library and runs automatically on a [Raspberry Pi](http://www.raspberrypi.org/) configured as a web server. All the code for the Sebastian Q. Canard project can be found on [GitHub](https://github.com/AdamDynamic).

## Other cool Twitter bot projects

The Sebastian Q. Canard project was inspired by a few of the many projects that have done this before. Below are some links to the best ones.

* [Horse_eBooks](https://twitter.com/Horse_ebooks) became so big it even got it's own [wikipedia page](http://en.wikipedia.org/wiki/Horse_ebooks)
* [TofuBot](http://www.geekosystem.com/tofu-product-creator-explains-bot/) takes tweets from a user's timeline and uses them to reply to direct messages
* [RealBoy](http://ca.olin.edu/2008/realboy/index.html) did some cool things with social graphs that I hope to use when Sebastian Q. Canard searches for followers
* [TrackGirl](http://www.wired.com/2012/06/twitter_arm/) was a social Twitter bot that people started to actually care about
