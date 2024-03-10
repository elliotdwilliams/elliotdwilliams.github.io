---
layout: post
title: Batch download LCSH files
date: 2021-09-17 21:53
author: admin
comments: true
categories: [cataloging, Cataloging, command line, Library of Congress, technology]
---
<!-- wp:paragraph -->
<p>It's been a minute since I wrote anything on this here blog, but I was working through a process today that I wanted to document and thought that other folks might be interested in.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This week, a group of coworkers and I were discussing whether there is an LCSH for "Cuban diaspora" (there is not), and whether there should be (we think that might be useful, and might eventually want to work on a proposal).  There are a handful of other LCSH terms for "[blank] diaspora", and I wanted to be able to view them all at once, rather than paging through them individually in ClassWeb.  That way I could compare the See From and See Also From references, the public notes, and the works cataloged, to get a sense of what the format and structure of this group of subject headings is.  So basically, what I wanted to do was to download a batch of LCSH records in a format that I could open in OpenRefine to examine.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>LCSH records are available in a variety of formats from <a rel="noreferrer noopener" href="https://id.loc.gov/authorities/subjects.html" target="_blank">id.loc.gov</a>, so I know the records are there on the web.  The records are available in MARCXML, JSON, RDF, and a number of other flavors.  I just needed a way to identify the ones I wanted and download them in batch.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Here's what I ended up doing.  As always, I'm sure there are other, probably faster or simpler ways to do this, but this process worked for what I wanted and used tools that I already knew how to use:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol><li>Search LCSH for "diaspora" in <a rel="noreferrer noopener" href="https://id.loc.gov/authorities/subjects.html" target="_blank">id.loc.gov</a></li><li>Literally copy and paste from the search results into a spreadsheet.  There were 55 total, so it was only 3 pages of results that I needed to copy.</li></ol>
<!-- /wp:list -->

<!-- wp:image {"id":436,"width":473,"height":242,"sizeSlug":"full","linkDestination":"media"} -->
<figure class="wp-block-image size-full is-resized"><a href="https://elliotdwilliams.com/wp-content/uploads/2021/09/image.png"><img src="https://elliotdwilliams.com/wp-content/uploads/2021/09/image.png" alt="Screenshot of a list of search results of LCSH terms, with the information highlighted to be copied and pasted" class="wp-image-436" width="473" height="242"/></a><figcaption>LSCH search results, ready to be copied</figcaption></figure>
<!-- /wp:image -->

<!-- wp:list {"ordered":true,"start":3} -->
<ol start="3"><li>In the spreadsheet, remove the subjects I'm not interested in, isolate the identifiers (e.g. sh2006004206), and save the list of identifiers as a text file.  Now I have a list of all of the identifiers for the relevant subject headings.</li><li>Now the part that took a bit more figuring out: Use <a href="https://curl.se/docs/manpage.html" data-type="URL" data-id="https://curl.se/docs/manpage.html">curl</a> to download the files in bulk.</li></ol>
<!-- /wp:list -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p><code>in $(cat Desktop/DiasporaSH_ids.txt); do curl -o “Desktop/diaspora/$ID.xml” “https://id.loc.gov/authorities.subjects/$ID.marcxml.xml”; done</code> </p></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p>Basically, what that does is read the list of identifiers, copies them into a variable ($ID), then runs a curl command using that variable to build the URL and saves the output, in this case as an XML file.  I use GitBash as my shell for running commands like this.  As usual, it took me some time to figure out how to get the "for" loop to work (it always takes me a while to remember how to structure it properly).  And because I use a windows computer, I also had to remove the carriage return characters at the end of the DiasporaSH_ids.txt file, so I’d stop getting an “Illegal characters found in URL” message.  I used <a rel="noreferrer noopener" href="https://linux.die.net/man/1/dos2unix" data-type="URL" data-id="https://linux.die.net/man/1/dos2unix" target="_blank">dos2unix</a> to do that.</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true,"start":5} -->
<ol start="5"><li>Now I have a folder full of MARCXML files!</li><li>Repeat the curl command for RDF and JSON files, or other formats as needed.</li></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For what I wanted to do today, the MARCXML turned out to be the most useful.  I went through a whole process using MarcEdit to convert them into .MRC files and then export the records to tab-delimited text, which I then loaded into OpenRefine.  I'm sure another tool could have done that more easily (probably pymarc?), but I knew I could get it done with MarcEdit, and just went with that.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>And voila, I had what I wanted!</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":439,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://elliotdwilliams.com/wp-content/uploads/2021/09/image-1.png"><img src="https://elliotdwilliams.com/wp-content/uploads/2021/09/image-1-1024x571.png" alt="Screenshot of OpenRefine interface, showing a facet on the left and exported MARC record data in the main view." class="wp-image-439"/></a></figure>
<!-- /wp:image -->
