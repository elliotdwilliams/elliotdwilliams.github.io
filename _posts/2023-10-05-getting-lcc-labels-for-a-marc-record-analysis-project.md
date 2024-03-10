---
layout: post
title: Getting LCC labels for a MARC record analysis project
date: 2023-10-05 21:55
author: admin
comments: true
categories: [cataloging, Cataloging, LCC, Library of Congress, MARC, OpenRefine]
---
<!-- wp:paragraph -->
<p>Hello! I'm back with another edition of "Elliot writes out the process for how I did something, mostly for myself, but puts on the internet in case anyone else finds it useful." This one started out as one thing (counting percentages of MARC records in a set that contain a given field) and morphed into another thing, as well (use OpenRefine to add labels to LCC class numbers).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I've been discussing with one of my colleagues ways to enrich our MARC record for musical scores to improve their searchability in the catalog.  One thing that we're interested in is including more 505 table of contents notes, particularly for collections or anthologies of printed music.  There are a lot of reasons that would be nice, but a big one is that collections are one of the main sources in our collection for works by contemporary composers, composers of color, and women composers.  </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>As we figure out if that's worthwhile and how we might approach it, I wanted to do some analysis of our notated music records to see how many have 505 notes.  I wanted to break it down by LCC classification, since we're thinking that that might be a helpful way of identifying priority areas and chunking the project into manageable pieces.  So here's what I did:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><em>Getting the data</em></p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol><!-- wp:list-item -->
<li>In Alma, did an advanced search for all physical notated music records, then exported those MARC records.<!-- wp:list -->
<ul><!-- wp:list-item -->
<li>There were a total of 12,949 records, which is a lot but manageable for what I was hoping to do.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Used MarcEdit to export the fields I care about to a tab-delimited file: 001, 245, 050, 090, 505<!-- wp:list -->
<ul><!-- wp:list-item -->
<li>Much later in the process, I realized it would have been helpful to include the 035 field at this stage.  Whoops.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><em>Processing in OpenRefine and class number cleanup</em></p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true,"start":3} -->
<ol start="3"><!-- wp:list-item -->
<li>Imported the tab-delimited data into OpenRefine</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Created a new column called "TOC?", where I entered either yes or no based on the presence of a 505 field in the record (using OR's "Facet by blank" facet)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Combined call number data from 050 and 090, and do some preliminary cleanup (mostly removing indicators/subfields and stray extra characters from the front)<!-- wp:list -->
<ul><!-- wp:list-item -->
<li>At this point, I realized it would have been better to have the call numbers from the holdings records, rather than the bib records, but that would have been more complicated to get out of Alma so I decided to go with what I had.  Good enough for this kind of exploratory analysis.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Used a regular expression to create a new column with just the first part of the call number, up to the first period that comes before a letter (hoping to get just the base classification number)<!-- wp:list -->
<ul><!-- wp:list-item -->
<li><code>value.match(/[A-Z]+*\.?\d*).*/).toString()</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":462,"width":600,"sizeSlug":"full","linkDestination":"media"} -->
<figure class="wp-block-image size-full is-resized"><a href="https://elliotdwilliams.com/wp-content/uploads/2023/10/Capture.png"><img src="https://elliotdwilliams.com/wp-content/uploads/2023/10/Capture.png" alt="Screenshot of an OpenRefine window for &quot;Add column based on column Call No normalized&quot;.  " class="wp-image-462" style="width:600px" width="600"/></a><figcaption class="wp-element-caption">Regex for getting the base class number</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p><em>Getting more info about class numbers</em></p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true,"start":7} -->
<ol start="7"><!-- wp:list-item -->
<li>Now I had the basic class number for each record. But I'm not super familiar with music class numbers, so I wanted to include the label for each number. I knew that on our old friend id.loc.gov, you can get data about each LC class in a variety of data formats, and you can construct the URL using the class number itself, e.g. <a href="https://id.loc.gov/authorities/classification/ML3551.html">https://id.loc.gov/authorities/classification/ML3551.html</a>.&nbsp; I first tried to work with the JSON versions, but I’m more comfortable with XML, so ended up using the RDF/XML format, e.g. <a href="https://id.loc.gov/authorities/classification/ML3551.rdf">https://id.loc.gov/authorities/classification/ML3551.rdf</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Before making a bunch of URL requests to the Library of Congress, I used OpenRefine’s records mode and blank down feature, to make sure that I only had to make one request for each class number.&nbsp; So only 757 URL requests, instead of 12,949.<!-- wp:list -->
<ul><!-- wp:list-item -->
<li>I think I picked up this tip from a <a href="https://programminghistorian.org/en/lessons/fetch-and-parse-data-with-openrefine#example-2-url-queries-and-parsing-json">Programming Historian tutorial</a> I was using to help figure out this process.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":465,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://elliotdwilliams.com/wp-content/uploads/2023/10/Capture4.png"><img src="https://elliotdwilliams.com/wp-content/uploads/2023/10/Capture4-1024x421.png" alt="Screenshot of OpenRefine in records mode, showing three records with multiple rows each, with the column &quot;Call No base&quot; as the first column" class="wp-image-465"/></a><figcaption class="wp-element-caption">Records mode, always a lifesaver</figcaption></figure>
<!-- /wp:image -->

<!-- wp:list {"ordered":true,"start":9} -->
<ol start="9"><!-- wp:list-item -->
<li>Okay, now the good stuff: I created a new column in OpenRefine by fetching URLs based on Call No base column, using expression <code>'https://id.loc.gov/authorities/classification/'+value+'.rdf'</code>.&nbsp; Now I have a column full of XML!</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Looking at the RDF/XML, I decided I wanted the &lt;rdfs:label&gt; field that contained the full chain of where this class number fits in the classification hierarchy.&nbsp; The problem is, though, that there are multiple &lt;rdfs:label&gt; elements in the output – so using just plain parseXml().select(’rdfs|label’) wouldn’t work.&nbsp; I want the instance of that element that is directly under the &lt;lss:ClassNumber&gt; element.&nbsp; So I looked at the <a href="https://jsoup.org/cookbook/extracting-data/selector-syntax">documentation for Jsoup</a> (I know, what an idea, right?), and figured out I could use <code>value.parseXml().select("rdf|RDF &gt; lcc|ClassNumber &gt; rdfs|label")[0].toString()</code> to get exactly the element I wanted.<!-- wp:list -->
<ul><!-- wp:list-item -->
<li>(This makes it sound a lot more straight-forward than it actually was.  Imagine lots of flailing around and frantic googling at this stage, particularly when I was trying to use JSON results before switching to XML.)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Pro-tip that I learned in this process: if your XML elements have namespaces, you have to enter them in the select expression as "rdfs|label", instead of "rdfs:label".</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":463,"sizeSlug":"full","linkDestination":"media"} -->
<figure class="wp-block-image size-full"><a href="https://elliotdwilliams.com/wp-content/uploads/2023/10/Capture3.png"><img src="https://elliotdwilliams.com/wp-content/uploads/2023/10/Capture3.png" alt="Screenshot of three columns in OpenRefine. First column is labeled &quot;Call No base&quot;, and the value is &quot;M1621&quot;. Second column is labeled &quot;RDFXML&quot; and has a lot of XML data in it. Third column is labeled &quot;rdfs Label&quot;, and the value is &quot;Music and Books on Music--Music--Vocal music--Secular vocal music--One solo voice--Accompaniment of keyboard instrument, keyboard and one other instrument, or unaccompanied--Separate works--Keyboard instrument accompaniment&quot;" class="wp-image-463"/></a><figcaption class="wp-element-caption">Class number, retrieved XML, and the full hierarchy label for that number</figcaption></figure>
<!-- /wp:image -->

<!-- wp:list {"ordered":true,"start":11} -->
<ol start="11"><!-- wp:list-item -->
<li>Ta-dah!&nbsp; Now I have a column with the full LCC description for each class number in my dataset.&nbsp; I used fill down in OpenRefine to add the class number and label to all of the rows.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>There were about 7 records where the process didn’t work because the data wasn’t structured normally or the call number was missing from the bib, so I added those manually.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>At this point, I thought I might be fancy and do something with pandas (which I'm very much a beginner with) to count the number of records without a 505 per class number.  But I decided I knew how to do what I wanted to in Excel, so I just did that instead. &#x1f937;  I used some simple deduping and COUNTIF formulas to get the number of records per class number, and the number in each class number that do not have a 505 field.  And it includes the full classification hierarchy label for each number, which is helpful.  That allowed me to highlight all of the rows with "Collection" in the classification label, which has already been helpful in pinpointing some class numbers to explore in more depth.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":460,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://elliotdwilliams.com/wp-content/uploads/2023/10/Capture2.png"><img src="https://elliotdwilliams.com/wp-content/uploads/2023/10/Capture2-1024x275.png" alt="Screenshot of an Excel spreadsheet, showing columns labeled # in class, Class Number, # missing TOC, % missing TOC, and Class Label." class="wp-image-460"/></a><figcaption class="wp-element-caption">The final product in Excel</figcaption></figure>
<!-- /wp:image -->
