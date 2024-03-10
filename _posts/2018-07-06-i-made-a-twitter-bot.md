---
layout: post
title: I made a twitter bot!
date: 2018-07-06 02:25
author: admin
comments: true
categories: [metadata, social media, technology, Uncategorized]
---
<blockquote class="twitter-tweet" data-lang="en">
<p dir="ltr" lang="en">'People taking a gondola ride along the Coral Gables Waterway. Coral Gables, Florida'
"Recognizing the importance of having a luxury resort hotel in the city, George Merrick turned to John McEntee Bowman, President o...'<a href="https://t.co/27MqoSFJtU">https://t.co/27MqoSFJtU</a></p>
— SSDNbot&#x2600;&#x1f334;&#x1f40a; (@SSDNbot) <a href="https://twitter.com/SSDNbot/status/1007986372983042049?ref_src=twsrc%5Etfw">June 16, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I love twitter bots. There are some really cool ones doing interesting things with cultural heritage materials:
<ul>
 	<li><a href="http://www.twitter.com/NYPLEmoji">@NYPLEmoji</a>, which responds to emojis with matching items from NYPL’s collections</li>
 	<li><a href="http://www.twitter.com/pokemon_nypl" target="_blank" rel="noopener">@pokemon_nypl</a>, which tweets collections images with Pokemon added to them</li>
 	<li><a href="http://www.twitter.com/PDcutup" target="_blank" rel="noopener">@PDcutup</a>, which creates collages out of two public domain images from two different institutions based on similarity in their titles</li>
 	<li><a href="http://www.twitter.com/leia_quotes" target="_blank" rel="noopener">@leia_quotes</a>, which tweets all of Princess Leia’s lines of dialogue (not cultural heritage related, but I still love it)</li>
</ul>
I’ve been slowly working my way through the Codecademy Python course, and I thought that a twitter bot would be a good chance to practice my Python coding skills. At work, I’ve been working a lot with a bunch of other amazing people on getting the <a href="https://sunshinestatedigitalnetwork.wordpress.com/" target="_blank" rel="noopener">Sunshine State Digital Network</a> (SSDN) established as the DPLA hub for the state of Florida, and as part of that, I’ve had the chance to play around with the <a href="https://pro.dp.la/developers/api-basics" target="_blank" rel="noopener">DPLA API</a>. So, combining all of those things, I decided to make a twitter bot that randomly tweets out items that SSDN has contributed to DPLA. It seemed like a good project for a variety of reasons: it was doable, there are lots of tutorials on creating a twitter bot using Python, and it combined several things I’ve been interested in lately.  And so, <a href="http://www.twitter.com/ssdnbot" target="_blank" rel="noopener">@SSDNbot</a> was born!

This was my first real project in Python - I’ve tinkered around with other people’s scripts, and done some tutorials, but this was my first start-to-finish project. That made it a little intimidating, but like I said, there are plenty of Python twitter bot tutorials out there. Some of the ones that I found most helpful are:
<ul>
 	<li><a href="http://briancaffey.github.io/2016/04/05/twitter-bot-tutorial.html" target="_blank" rel="noopener">http://briancaffey.github.io/2016/04/05/twitter-bot-tutorial.html</a></li>
 	<li><a href="https://jitp.commons.gc.cuny.edu/make-a-twitter-bot-in-python-iterative-code-examples/" target="_blank" rel="noopener">https://jitp.commons.gc.cuny.edu/make-a-twitter-bot-in-python-iterative-code-examples/</a></li>
 	<li><a href="https://github.com/tommeagher/heroku_ebooks" target="_blank" rel="noopener">https://github.com/tommeagher/heroku_ebooks</a></li>
 	<li><a href="https://github.com/llamafarmer/bitcoin_tweeter" target="_blank" rel="noopener">https://github.com/llamafarmer/bitcoin_tweeter</a> (just ignore the fact that it's about bitcoin…)</li>
</ul>
I could maybe have followed some of these tutorials without really knowing what I was doing, but having been working through the Python course on Codecademy really helped. I also used two other DPLA-related bots as models, which were super helpful (<a href="https://github.com/samplereality/DPLAbot" target="_blank" rel="noopener">@DPLAbot</a> and <a href="https://github.com/ruebot/dplafy" target="_blank" rel="noopener">@dplafy</a> [links go to their GitHub repositories]).

Reading tutorials like those above also introduced me to <a href="http://www.heroku.com" target="_blank" rel="noopener">Heroku</a>, which I wasn’t at all familiar with. Basically, Heroku provides isolated Unix machines that you can use to run scripts (I’m sure it does much more, but that is how I’m using it). And the best part is that there is a free version! So I launched my script on Heroku, and was able to use the Heroku scheduler to run it every 12 hours. Which is great, but I could not for the life of me figure out how to write the scheduling into the script itself (which was extraordinarily frustrating).

The <a href="https://github.com/elliotdwilliams/SSDNbot" target="_blank" rel="noopener">Python script</a> harvests 500 items at a time from DPLA (because that is DPLA’s API limit), limited to items contributed by SSDN, then randomly picks one of those items to tweet. It composes a tweet that includes the item’s title, the item’s description field (if it has one), and a link to the item in DPLA. Initially, I was just using the title, but so many items have titles that aren’t really very descriptive, so I decided to include the description. Figuring out how to write the logic to only include the description field if it exists was tricky, but was a good thing to learn how to do. I also ended up having to rewrite the script in Python 3, instead of Python 2.7, because of the different ways Unicode is handled in the two versions. Character encoding problems are just inescapable, apparently.

One of the things that this project reinforced for me is the number of things you need to know to get a technical project like this going. (I actually wrote about a similar thing, way back in the <a href="https://elliotdwilliams.com/open-source-software-expertise-required/" target="_blank" rel="noopener">first entry on this blog</a>, back when I was just a wee iSchool student.) I had most of the Python knowledge that I needed (or could pick it up along the way), but there’s always way more to it than that. I had to learn the slightly different way you use pip on Python 2.7 vs. Python 3. I had to learn about and understand how to deploy something on Heroku. I had to get a better understanding of how to actually use GitHub. There are always more pieces to master than I expect, and I often don’t know I need to know them until I’m confronted with them. I don’t know that there is an easy solution to this, but I think it’s important to remember, especially when thinking about how to create usable documentation and tutorials.
<blockquote class="twitter-tweet" data-lang="en">
<p dir="ltr" lang="en">'Jones, Randolph'
'A record describing Jones, Randolph, a colored voter from Leon County, who registered to vote in 1867.'<a href="https://t.co/btiuXMEN6h">https://t.co/btiuXMEN6h</a></p>
— SSDNbot&#x2600;&#x1f334;&#x1f40a; (@SSDNbot) <a href="https://twitter.com/SSDNbot/status/1012697426740891648?ref_src=twsrc%5Etfw">June 29, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I’ve really enjoyed watching my little bot tweet away. Seeing individual items has helped me notice things about our collections that I wasn’t aware of, and putting them in my twitter feed has helped me contextualize them in new and interesting ways. I also think there is real value for metadata librarians to build things like this. For example, I’ve become more aware of how important distinctive titles are after seeing how unhelpful some titles were on their own.

So what’s next for SSDNbot? A few weeks after the bot started tweeting, I noticed that some of the links to DPLA weren’t working - they would sometimes (but not always) lead to a 404 page. I’m still not sure what was going on there, so I’m keeping my eye on that. There are a few improvements I’d like to make to the bot, as well. It would be really cool if it could include the actual image from the item, instead of just the link. That is complicated by two factors: the images aren’t stored by DPLA, so it would need to pull them from the system of origin; and since the script runs on Heroku, it doesn’t really have a place to store the image files before tweeting them. Possibly not insurmountable obstacles, but will take some thinking. I’d also really like to find a way to pull from all 148,000 items contributed by SSDN, instead of just the first 500. I’m not sure what the way to do that without clobbering the DPLA API might be, though. I’ve also thought about doing some sillier things with it - replacing text in the metadata with emojis, for example. Who knows; the twitter sky is the limit!
