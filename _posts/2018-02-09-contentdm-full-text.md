---
layout: post
title: Export CONTENTdm Full Text field to .txt files
date: 2018-02-09 23:01
author: admin
comments: true
categories: [command line, contentdm, excel, metadata, Metadata]
---
This week, we had a case at my library where we wanted to extract the full-text field for a few of our digital collections and save it as a separate text file for each image.  Normally, we create text files for images by running OCR on them outside of CONTENTdm.  But for a few collections, transcriptions were manually created and entered into the Full Text field directly in CONTENTdm.  We wanted to transfer those transcriptions from the CONTENTdm metadata to plain text files that we can preserve separately, both as a backup and in preparation for a future migration.

I thought that other people might have a similar use case for this workflow, and wanted to make sure and save it for myself, so I decided to write up some quick documentation here.  I’m fairly confident that there are other, possibly much simpler, ways to do this, but this was a method that I could do using tools that I already (mostly) was familiar with.

First, export the metadata for the collection from CONTENTdm, and open it in Excel. Then copy two columns into a new spreadsheet:
<ul>
 	<li>Column A should contain containing the identifiers, which will become the filenames for the text files (for us, these are our “Digital IDs”, which match the filenames of the corresponding TIFFs)</li>
 	<li>Column B should be the full text transcription (for us, this is simply called “Full Text”</li>
</ul>
Delete any rows without text in the Full Text column, using “Go To Special”

<a href="https://elliotdwilliams.com/wp-content/uploads/2018/02/ExcelGoToSpecial.png"><img class="alignnone wp-image-372" src="https://elliotdwilliams.com/wp-content/uploads/2018/02/ExcelGoToSpecial.png" alt="" width="383" height="359" /></a>

![Screenshot of Excel 'Go To Special' dialog box](/images/2018/ExcelGoToSpecial.png)

To save each row as its own text file, I used a quick macro that I adapted from responses to a <a href="https://stackoverflow.com/questions/13077740/create-text-files-from-every-row-in-an-excel-spreadsheet">Stack Overflow question</a>. The macro saves each cell in Column B as a txt file, with the value of Column A as the filename:
<blockquote><span style="color: #333333;"><strong>Sub savemyrowsastext()</strong></span>

<span style="color: #333333;"><strong>Dim saveText As String</strong></span>
<span style="color: #333333;"><strong>  </strong></span>
<span style="color: #333333;"><strong> For Each cell In Sheet1.Range("A1:A" &amp; Sheet1.UsedRange.Rows.Count)</strong></span>
<span style="color: #333333;"><strong>     saveText = cell.Text</strong></span>
<span style="color: #333333;"><strong>     Open "C:\Users\ewilliams\Desktop\" &amp; saveText &amp; ".txt" For Output As #1</strong></span>
<span style="color: #333333;"><strong>     Print #1, cell.Offset(0, 1).Text</strong></span>
<span style="color: #333333;"><strong>     Close #1</strong></span>
<span style="color: #333333;"><strong> Next cell</strong></span>

<span style="color: #333333;"><strong>MsgBox ("Done")</strong></span>

<span style="color: #333333;"><strong>End Sub</strong></span></blockquote>
Ta-dah!  Now you have text files for each page/image in CONTENTdm!

In looking through the text files, though, I realized that the line breaks in the transcriptions got lost in the process.  (I suspect CONTENTdm doesn’t output the line breaks in the metadata at all, which makes sense.)  I wanted those line breaks, though, since they were present in the transcriptions in CONTENTdm, and help make the transcriptions more user-friendly.

Looking at the files, I discovered that in the exported metadata, what showed up in the CONTENTdm front-end as two line breaks was turned into four blank spaces.  I can’t guarantee that is how CONTENTdm will always export that, but it was in my case.

Well, that should be easy enough to fix en masse using the command line.  After googling around and testing various ways of manipulating text files on the command line, I settled on using the <a href="https://www.computerhope.com/unix/used.htm">sed command</a>. (Various attempts to use the tr command or perl didn’t work, for reasons I wasn’t willing to put the time into figuring out.) I did a little more research/refreshing on how to encode <a href="https://en.wikipedia.org/wiki/Newline">newline characters</a>, and ended up with this:
<blockquote><strong>sed –i ‘s/    /\r\n\r\n/g’ *.txt</strong></blockquote>
It's worth noting that sed only works on a bash shell (I use Git Bash).  It’s also a good idea to make backups of the text files before running this, just in case something goes wrong (although you could always just re-export them from the Excel file).

<a href="https://elliotdwilliams.com/wp-content/uploads/2018/02/finishedProduct.png"><img class="alignnone wp-image-373" src="https://elliotdwilliams.com/wp-content/uploads/2018/02/finishedProduct.png" alt="" width="391" height="172" /></a>

So that’s it.  Relatively straight-forward, although as I mentioned, I’m sure there are quicker/cleaner ways to do it.  I’d love to hear any feedback or suggestions for how to improve this process!
