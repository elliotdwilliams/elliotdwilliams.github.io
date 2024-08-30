---
layout: post
title: Searching for unique subject headings in DPLA
date: 2023-08-18 22:08
author: admin
comments: true
categories: [DPLA, metadata, Metadata, python, subject headings]
excerpt_separator: <!--more-->
---
<!-- wp:paragraph -->
<p>I’ve been thinking about subject headings a lot lately (as one does), and it inspired me to return to some work that I had started doing while I was at TDL, thinking about subject heading uniqueness in <a href="https://dp.la" target="_blank" rel="noreferrer noopener">DPLA</a>.</p>
<!-- /wp:paragraph -->
<!--more-->

<!-- wp:paragraph -->
<p>One of the things that I’ve noticed in my own metadata work, and that was confirmed for me when I was looking at other people’s metadata as part of my TDL job, is that subject headings for digital collections are often very specific and detailed. On top of that, there aren’t good rules for applying subject headings to digital collections items (in the way that there are for MARC cataloging), and the people who create digital collections metadata often don’t have a lot of expertise in LCSH structure and application rules (no shade, it’s a very niche and abstruse area to have expertise in). All of that combined means that I have a strong suspicion that a lot of subject terms used in a given institution’s digital collections are unique to that institution.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>That’s all well and good within that institution’s repository, but when you start sharing metadata with an aggregator like DPLA, I think that starts to become more of a problem. If we assume that one of the purposes of applying subject headings is to link similar items together, then having subject headings that are unique to an institution don’t serve that purpose, or the purpose of aggregation, very well.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>So I wanted to find a way to easily identify what subject headings for a given institution are unique in DPLA. I was inspired a lot by the <a href="https://doi.org/10.1108/EL-11-2020-0317" target="_blank" rel="noreferrer noopener">research that Mark Phillips and Hannah Tarver have published around metadata record graphs</a>, also looking at subject headings in DPLA. Their research is a lot more detailed and uses network analysis to explore how subject headings link records together within a large corpus. My goal was more modest: as a metadata creator, I want to see what subject headings in my institution are unique, to potentially inform metadata creation practice at my library.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>As I do in most situations, I turned to my faithful friends: python and the DPLA API. (Can we talk for a minute about the DPLA API, and how simple and easy to use and well-documented it is? It’s really great.) I ran through a couple of iterations of the script before I settled on a method that I like. Here’s the script I came up with: <a href="https://github.com/elliotdwilliams/dpla-subject-search/blob/main/dpla-uniq-subjects.py" target="_blank" rel="noreferrer noopener">https://github.com/elliotdwilliams/dpla-subject-search/blob/main/dpla-uniq-subjects.py</a> (In that Github repository, there are a few other variations that do parts of that work, if you’re looking for something other than strictly unique subject terms.)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Basically, the script takes a given contributing institution in DPLA, searches for that institution, and grabs a list of the most common subject terms in that institution’s contributed records. (You can get up to 2,000 subject terms, because that is the limit that the API will return as a facet.) Then, it searches for each of those subject terms in DPLA, and checks to see if it is used by only one institution. If it is, that subject term is written to an output file. It also generates a percentage for how many of the subject terms it searched are unique.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I ran it today for <a href="https://dp.la/search?provider=%22UTSA+Libraries+Special+Collections%22&amp;page=1" target="_blank" rel="noreferrer noopener">UTSA Libraries</a>, and of our top 1000 subject headings in DPLA, 323 are unique to us. Which, honestly, I feel like a 32% uniqueness percentage is pretty good! Looking at the list, a lot of them are unique because they are about San Antonio, which makes sense (either local places like “West Side (San Antonio, Tex.)” or pre-coordinated strings like “Lesbians--Texas--San Antonio”). There are also quite a few that show up as unique because of some idiosyncratic formatting choices that have been made in the past. There are some headings, though, which I wouldn’t have expected to be unique in DPLA, like “Immigrants, English” - offhand, that looks to me like a correctly formatted LCSH term, but I’m definitely going to double-check. I also spotted a couple of typos, which now that I know about them I can fix them!</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I wouldn’t call it a groundbreaking metadata analysis tool by any means, but I’m glad to have this script available for exploring subject terms in DPLA.</p>
<!-- /wp:paragraph -->
