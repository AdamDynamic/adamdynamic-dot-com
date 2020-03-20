---
author: admin
date: 2015-01-24 12:07:52+00:00
draft: false
title: 'War In Pieces: Leo Tolstoy''s classic, 140 characters at a time.'
type: post
url: /war-and-peace/
---

This post describes my project to recite Leo Tolstoy's classic War And Peace, 140 characters at a time using twitter. You can read along at [@_War_And_Peace](https://twitter.com/_War_And_Peace).

## War in Pieces

A while back I started thinking about a new twitter project having recently made a [twitter bot](http://adamdynamic.com/making-twitter-bot/) and some [infographics](http://adamdynamic.com/twittermetrics/) that I enjoyed doing. Specifically, I thought about what the 'opposite of Twitter' would be - an antithesis to the 140 character format that has made the Twitter so popular.

What came to mind was the modern metaphor for anything that lacks brevity: [Leo Tolstoy's 'War and Peace'](http://en.wikipedia.org/wiki/War_and_Peace). At about 580,000 words (depending on the translation) it's to the length of your manager's powerpoint on '_Inter-Departmental Performance Measurement Metrics_' what a [Double-Decker Bus](http://en.wikipedia.org/wiki/List_of_unusual_units_of_measurement#Bus) is to the height of [Nelson's Column](http://en.wikipedia.org/wiki/Nelson%27s_Column) - a catch-all unit of measurement, universally understood as being "very, very long".

So, naturally I thought it would be fun to tweet it, line by line.

{{< tweet 558946783016284160 >}}

## War and Peace e-Book

To chop it up into 140 character chunks, I first needed an electronic copy of the text.

[Project Gutenberg](https://www.gutenberg.org/) is a digital library of books in the public domain, run and maintained by volunteers. They've been producing e-versions of copyright-free works since the 1970s when they used to digitise books by typing them manually (the Declaration of Independence was their first).

[Their *.txt version of War and Peace](http://www.gutenberg.org/files/2600/2600.txt) meant that I didn't have to do the same.

## 140 Characters at a time

To make the feed as readable as possible I wanted to be careful how I broke the original text into segments, avoiding dividing paragraphs mid-word or mid-sentence. There are also special cases that I wanted to catch, such as book-ending with quotation marks segments of character's speeches (where the speech is divided across several tweets).

To divide the text I used a  hierarchy of steps. At each step, the paragraph is divided into segments and then tested to see whether it was longer than 140 characters. If it was, the next method in the list was applied to divide that segment into smaller segments (the ones that are fine are left) and so on.

The order of the hierarchy follows some grammatical rules but was mainly based on what I thought would work best based on the way the book is written.

The hierarchy went as follows:

### 1) Full-stops, question-marks, exclamation marks_ etc_

Dividing a sentence into segments (or "tokens" as they are referred to in natural language processing) is harder than it sounds - simply splitting paragraphs using full-stops doesn't work when the paragraph contains words like "St. Petersburg" (which, being about Russia,  the paragraphs in War and Peace do. A lot.)

The [Natural Language Processing Toolkit](http://www.nltk.org/) is a python library designed to do the heavy lifting in cases like this and is excellent at capturing these corner cases. Because the library is much more sophisticated than anything I would be able to design, this was used as the first method for dividing up the text.

### 2) Other Punctuation

Where the NLTK failed to break the text into a segment of less than 140 characters in length, the sentence was split by (in order) semi-colons, colons, hyphens, brackets and finally commas.

Because I wanted to make each tweet as long as possible (and because commas for example are, sometimes, quite, short), I ran a consolidation process afterwards to try and re-combine the segments into longer (sub-140 character) ones.

### 3) Doing it by hand

In about 1000 cases of the ~36,000 sentence segments that the script produced, the segment was still longer than 140 characters. These segments were usually missed because the use of punctuation wasn't completely correct (e.g. a space was missing before an opening quotation mark) or the segment didn't contain any punctuation at all.

Rather than fine-tune the script to capture more special cases I went through and divided the final segments by hand.

## The Output

The script split the e-book (including the epilogue) into a total of 36,403 tweets (a total of 559,231 words). The story is tweeted using a Raspberry Pi at a rate of one tweet an hour (I didn't want to do it too fast and spam followers' timelines - it's a really long book) and it should finish in March 2019.

You can read along by following the account on twitter at [@_War_And_Peace](https://twitter.com/_war_and_peace); if you have any thoughts on the project then I'd love to hear them - message me [@AdamDynamic](http://twitter.com/AdamDynamic).

## The Code

You can download my segmented version of Project Gutenberg's War and Peace [here](https://github.com/AdamDynamic/war-and-peace/blob/master/LeoTolstoy_WarAndPeace_Complete_TweetVersion.txt.zip).
