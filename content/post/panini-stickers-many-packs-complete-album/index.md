---
author: admin
date: 2014-06-03 07:55:04+00:00
draft: false
title: Panini Stickers - How many packs to complete the album?
type: post
url: /panini-stickers-many-packs-complete-album/
---

This post uses a Monte Carlo simulation written in Python to estimate how many packs of stickers it takes to complete the Panini sticker album for the 2014 Fifa World Cup.

## How many packs of stickers do you need to buy to complete a Panini sticker album?

Got, got, need, got got.

To a generation of once-pre-teen football fans, this was the sound of a new football season. With the 2014 FIFA World Cup comes the [Panini 2014 World Cup Sticker Album](http://2014fifaworldcupbrazil.paninigroup.com/home?utm_source=panini_uk&utm_medium=banner&utm_campaign=fwc_collection) and with it a new wave of[ nostaligic football fans reliving their sticker-swapping youth](http://www.bbc.co.uk/news/magazine-27051215).

This time around there seems to be a fair bit of interest in the question that every parent (subconsciously) asks when shelling out pocket money: '[how many stickers do I need to buy to complete a Panini sticker album?](http://www.economist.com/blogs/economist-explains/2014/05/economist-explains-13)' (and more importantly, '[how much will it cost?](http://www.theguardian.com/football/blog/2014/apr/20/panini-world-cup-stick-album-collector-highs-lows-woes)')

An article in the [Independent](http://www.independent.co.uk/sport/football/international/world-cup-2014-taking-the-fun-out-of-panini-football-stickers-or-putting-the-fun-into-algebra-9452657.html) that discusses a [blog post](http://legavrik.blogspot.co.uk/2014/05/world-cup-stickers.html) caught my attention. In it, some [probability theory](http://en.wikipedia.org/wiki/Probability_theory) is used to determine the expected number of sticker's you'd have to buy in order to collect them all. The number of packs (and from it, the total cost) is determined by dividing the total number of stickers needed by the number of stickers you get in each pack.

The problem with this analysis and [others like it](http://siwhitehouse.co.uk/blog/2010/04/25/panini-football-stickers-and-the-coupon-collector-problem/) is that they assume that the stickers are collected one by one and that the probability of you needing a random sticker is [independent](http://en.wikipedia.org/wiki/Independence_(probability_theory)) of whether you needed the previous random sticker. The stickers in each pack are guaranteed to be unique from each other however, even if they're not unique from the stickers you've already got*. The statistical dependence that this feature of the sticker packs introduces means that some more esoteric probability theory is required.

It's been a while since I've spent any time with probability theory (esoteric or otherwise) so I thought a reasonably-quick-and-dirty [Monte Carlo simulation](http://en.wikipedia.org/wiki/Monte_Carlo_method) with a Python script would suggest a solution that accounts for this 'sticker pack independence', without having to fully understand the maths behind it.

\* <sup>Assume you have say, 100 stickers in your album and you open a new pack of stickers. The probability that you already have the first sticker in your new pack is 100/639 (or 15.6%) as there are 100 stickers you own, and 639 stickers in total (assuming the distribution is completely random), _Given you have the first sticker_, the probability that you also have the second is now _99/638_ (or 15.5%). This is because you know that this sticker is different from the first one you tested (the stickers in each pack are unique from each other, and you know you already have it in the album) so you can remove that sticker from both sides of the fraction. The difference doesn't seem big (only 0.1% in this example), but it's large enough to create an error in your calculations if you assume it isn't there.</sub>

## The Assumptions

The Monte Carlo simulation makes the following assumptions;

* There are 639 stickers in the album
* You get 6 (unique) stickers free when you buy the album
* Each purchased pack contains 5 stickers
* The stickers in each pack are assumed to be unique from each other
* The distribution of stickers is assumed to be even, with no teams or players more popular than any other*

As Panini allow you to buy the last 50 stickers (and it's assumed that this is cheaper than trying to find the last 50 by buying packs) the objective is to find the number of packs needed to collect the first 639 - 50 = **589** stickers. The first 6 of these are free with the album so we're interested in how long it takes to collect the next 583.

\* <sub>Anyone who collected stickers as a kid and found the same faces peering out of every new pack would dispute this (for me: Dion Dublin, Man Utd, '92-'93 Merlin Premier League sticker album. Every. Single. Time.); [these researchers](http://www.unige.ch/math/folks/velenik/Vulg/Paninimania.pdf) seem to suggest that the assumption is accurate however.</sub>

## The Monte Carlo Simulation

The simulation works as follows:

1. A new album is created pre-populated with 6 stickers (in the script, stickers in the album are represented by the numbers 0 to 638)
2. A random 'pack' of 5 stickers is generated (e.g. [606, 124, 185, 499, 318])
3. The script checks the new pack, ignoring any numbers already in the album ("got") and adding to the album any that are missing ("need")
4. The process of generating packs and adding missing numbers is repeated until the number of unique stickers in the album is greater than 610
5. Once this is done, the total number of packs opened is recorded and the process starts again

I repeated the process 250,000 times.

## The Results

The mean number of packs needed (based on a simple weighted average) to complete the first 589 stickers is **292 packs** (well, 291.7898 to be more precise, but we assume that you can't buy 0.7898 of a pack).

##  The Cost

In order to calculate the cost of buying the packs we consider the following:

* The album itself costs **£2.99**
* The album comes with **6 free stickers** and **3 free packs** of stickers
* Each additional **pack of 5** stickers costs **£0.50**, or you can buy **multi-packs of 30 stickers (6 packs)** for **£2.75**
* Stickers direct from Panini cost **£0.25** each plus postage & handling of **£1**
* Assume that collectors will buy the packs in the most efficient way*

Based on this, the cheapest way to buy the stickers is:

* 1 x album (includes 6 free stickers) = £2.99
* 3 free packs (included with the album)
* 48 multi-packs x £2.75 = £132.00
* 1 single pack x £0.50 = £0.50
* 50 stickers from Panini x £0.25 = £12.50
* Postage & handling for the 50 stickers = £1
* **Total: £148.99**

\* <sub>There's some data-snooping here as it assumes that the collector will know in advance that they will collect the 589th sticker in the next 5 packs, otherwise it's possible that they would buy the multi-pack instead. This type of 'probabilistic decision making with incomplete information' is beyond the scope of this analysis but might be fun for a future project.</sub>

## Injury Time

To wrap up, it's important to note that buying 292 packs doesn't 'guarantee' that you'll complete the album; 292 is merely the number where completing the album becomes 'more likely than not'. In fact, as each pack is random no number of packs guarantees success, so it's possible you'd have gaps in your album and find nothing but swaps in each new pack.

Speaking of swaps, the simulation ignores the ability to swap stickers with others (got, got, need, got) which I would expect to significantly reduce the number of packs needed - though by how much I expect would depend on the number of friends you had and whether they were willing to swap [Lionel Messi](http://en.wikipedia.org/wiki/Lionel_Messi) for your [Sokratis Papastathopoulous](http://en.wikipedia.org/wiki/Sokratis_Papastathopoulos) duplicate on a one-for-one basis (from experience, not always guaranteed).

The Python code I used is below, any comments in the meantime feel free to say hello on [Twitter](https://twitter.com/adamdynamic).

##  The Code

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    '''
    #######################################################################################################

    AUTHOR:             Adam Dynamic
    CONTACT:            helloadamdynamic@gmail.com / www.adamdynamic.com
    LAST REVISION:      29 May 2014

    PROJECT:            Stickers
    DESCRIPTION:        Monte Carlo Simlation to determine how many packs of stickers it takes to complete the album

    #######################################################################################################
    '''

    import random

    ### Constants used in the Monte Carlo Simulation ###
    NUMBER_OF_STICKERS_TOTAL = 660
    NUMBER_OF_STICKERS_PURCHASED = 50
    NUMBER_OF_STICKERS_FREE = 6

    NUMBER_OF_STICKERS_IN_PACK = 5

    COST_PER_PACK = 0.5

    ITERATIONS_TOTAL = 250000


    class album(object):

        ''' Album of stickers, to which stickers from packets are added '''

        def __init__(self):
            ''' creates the album with stickers '''

            # Assume without loss of generality that the free stickers in each album are identical in every album
            self.stickers = [0,1,2,3,4,5]
            self.total = 6

        def add_stickers(self, sticker_pack):
            ''' appends stickers to the album '''

            for sticker_value in sticker_pack:

                # Only add the sticker if not already in the collection
                if sticker_value not in self.stickers:

                    self.stickers.append(sticker_value)

                    self.total = self.total + 1


    class packet(object):

        ''' Packets of stickers, each sticker in each pack is unique '''

        def __init__(self):
            ''' creates a random pack of 5 stickers '''

            complete_stickers = [i for i in range(0,NUMBER_OF_STICKERS_TOTAL)]

            random.shuffle(complete_stickers)

            self.stickers = []

            # Generate a random selection of unique stickers
            while len(self.stickers) < NUMBER_OF_STICKERS_IN_PACK:

                self.stickers.append(complete_stickers.pop())



    ### Monte Carlo Simulation ###

    number_of_albums = 0
    output_dict = {}
    stickers_remaining_dict = {}

    while number_of_albums < ITERATIONS_TOTAL:

        #print number_of_albums

        # Reset the number of packs
        number_of_packs = 0

        sticker_album = album()

        # Assume that the last stickers are purchased and remove the ones that come with the album for free
        # Continue until the album is full
        while sticker_album.total < (NUMBER_OF_STICKERS_TOTAL - NUMBER_OF_STICKERS_PURCHASED):

            # Generate packet of random, unique stickers
            packet_of_stickers = packet()

            # Add them to the album
            sticker_album.add_stickers(packet_of_stickers.stickers)

            # Increment the number of packets 'opened'
            number_of_packs = number_of_packs + 1

        # Increment the histogram dictionary
        if number_of_packs in output_dict.keys():
            output_dict[number_of_packs] = output_dict[number_of_packs] + 1
        else:
             output_dict[number_of_packs] = 1

        # Determine how many stickers are remaining that require purchasing individually
        stickers_remaining = NUMBER_OF_STICKERS_TOTAL - sticker_album.total

        if stickers_remaining in stickers_remaining_dict.keys():
            stickers_remaining_dict[stickers_remaining] = stickers_remaining_dict[stickers_remaining] + 1
        else:
            stickers_remaining_dict[stickers_remaining] = 1

        # Increment the number of albums created
        number_of_albums = number_of_albums + 1
        print number_of_albums


    ### Output to external file ###

    # Write output_dict in pseudo-josn format
    with open('Stickers_Frequency_Output.txt','w') as sticker_output:
        for p in output_dict.keys(): # python will convert \n to os.linesep
            sticker_output.write('{"TimeSlot":"%s", "Frequency":"%s"},\n' % (p,output_dict[p]))

    sticker_output.close()

    # Write stickers_remaining_dict in pseudo-josn format
    with open('Stickers_Remaining_Output.txt','w') as sticker_frequency:
        for p in stickers_remaining_dict.keys(): # python will convert \n to os.linesep
            sticker_frequency.write('{"TimeSlot":"%s", "Frequency":"%s"},\n' % (p,stickers_remaining_dict[p]))

    sticker_frequency.close()
