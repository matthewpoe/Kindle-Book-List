# Kindle-Book-List

> **BLUF:**  Use the Kindle for PC app to generate the KindleSyncMetadataCache.xml. Use that file and a parser program to extract the data fields into a CSV, HTML(Table), MD(Table) or TSV file for use. What follows is my train (wreck) of research to reach this decision.  

This is a research project to collect information from Amazon related to purchased books from the Kindle Cloud Library. All the books added to Kindle account are stored on Amazon servers, not on the particular device. Access your Kindle Cloud Library by logging into your Amazon account then select Manage Your Content and Devices. 

Access the Library: https://read.amazon.com/kindle-library

Note:
- Kindle Cloud Reader offers very basic features: highlights, notes, or search within a book.  
- You can read your books in the browser, creating notes & highlights, just like using your Kindle Device.  
- You can copy your notes & highlights within the web browser for all synced books.
Data in the Kindle Cloud Library is stored in a JSON format.  


#### Equipment
* Windows 10 PC
* Kindle Fire HD10

#### References
* [Amazon Product Advertising API (AWS Account)](https://affiliate-program.amazon.com/gp/advertising/api/detail/main.html) - An AWS account is required!  
* [GoodReads API](https://www.goodreads.com/api) - A GoodReads key is required!  
* [Karen's Print Directory](http://www.karenware.com/)  
* [Kovid Goyal's calibre](https://github.com/kovidgoyal/calibre)  
* [Markdown Cheat Sheet](https://github.com/lifeparticle/Markdown-Cheatsheet)  

#### Related GISTS
- https://gist.github.com/jkubecki  
- https://gist.github.com/usayamadx  

## Acronyms
| Acronym | Description |  
| ----------- | ----------- |
| ASIN    | Amazon Store Identification Number;<br> Amazon Standard Identification Number |  
| csrs    | ??? - If you know - let me know! |  
| CSV     | Comma Separated Values (a Database) |  
| CTRL    | Control (a keyboard key!) |  
| HTML    | HyperText Markup Language |  
| JSON    | JavaScript Object Notation (a Database) |  
| K4PC    | Kindle for Personal Computers |  
| K4W     | Kindle for Windows |  
| TSV     | Tab Separated Values |  
| XHR     | XML HTTP Request |  
| XML     | eXtensible Markup Language (a Database) |   

## Build the Query
| Query Type | Options Available |  
| ----------- | ----------- |
| sortOrder | { DESCENDING &#124; ASCENDING } |  
| sortIndex | { DATE &#124; TITLE &#124; AUTHOR } |  
| startIndex | <StartIndex> ( 0, then multiples of batchSize) |  
| batchSize | <bsize> ( Max 50 ) |  
| contentType | Ebook |  
| itemStatus | { Active &#124; Expired } |  
| excludeExpiredItemsFor | { KOLL &#124; Purchase &#124; Pottermore &#124; FreeTrial &#124; DeviceRegistration &#124; ku &#124; Sample } |  
| originType | { Purchase &#124; PublicLibraryLending &#124; PersonalLending &#124; KOLL &#124; RFFLending &#124; Pottermore &#124; Rental &#124; DeviceRegistration &#124; FreeTrial &#124; ku &#124; Sample &#124; Prime } |  
| isExtendedMYK | { True &#124; False } |  
| csrfToken | encodeURIComponent(csrf) |  

> **Note**: Select one item within {} for your query. Not all elements are required in the query. I haven't tested all possible queries, but the first 5 in the list do work well.  

## Data Elements (Fields)
| Element | Description | Tested - Problems | 
| ----------- | ----------- | ----------- |  
| authors[0] | List of Authors | Yes - colons & commas |  
| title | The Title of the Book | Yes - Series & commas |  
| asin | Amazon's Unique ID Number for the Book | Yes |  
| webReaderUrl |  bookmark to the web Reader | No |  
| productUrl | a link to cover images |  Yes | 
| percentageRead | a guestimate of how much you've read of the book.  | Yes |  
| resourceType | (EBOOK or EBOOK_SAMPLE) | No |   
| originType | (PRIME and PURCHASE) | No |   
| orderDetailURL | URL To your Purchase Order for the Book | No |   
| productImage | URL to the Book cover | Yes - Undefined |   
| acquiredDate | Purchase Date | Yes - Undefined |   

## Additional Possible Fields from the product page:
| Element | Description |  
| ----------- | ----------- |  
| sales_rank Sales | ranking of the book (Date Specific) |  
| rating | Rating of the Book (Date Specific) |  
| number_of_ratings | Number of Ratings for the Book (Date Specific) |  

## Alternate Sources of Book List
* [KindleSyncMetadataCache.xml](https://github.com/MrMikey59/Kindle-Book-List/blob/main/KindleSyncMetadataCache.xml) (a Kindle for PC database normally found at **C:\Users\%USERNAME%\AppData\Local\Amazon\Kindle\Cache**)
  - Books are in Root **<add_update_list>** in **<meta_data>**...**</meta_data>** blocks.  
  - Fields include: *ASIN*, *title*, *authors*, *publishers*, *publication_date*, *textbook_type*, *cde_contenttype*, and *content_type*.
* [Kindle Cloud Reader](https://read.amazon.com/kindle-library) HTML file.
* [GoodReads](https://www.goodreads.com/) or [GoodReads Mobile](https://www.goodreads.com/toggle_mobile) Shelfari or My Books HTML file.
* You can copy/print the file names (mostly ASINs) from your Kindle while in MPT Mode attached to your computer (using Karen's Print Directory) from either of the following directories that must be copied to a local drive temporarily:
  - Internal **storage\Android\data\com.amazon.kindle\files\medium** 
  - Internal **storage\Android\data\com.amazon.kindle\files\small** 
* You could parse the JSON file metadata.calibre created by [calibre](https://github.com/kovidgoyal/calibre) for the device. You may have two of these files with an external storage installed!  

## KindleSyncMetadataCache.xml File Sample Format:
```xml
    <add_update_list>  
      <meta_data>  
        <ASIN></ASIN>  
        <title></title>  
        <authors>  
          <author></author>  
        </authors>  
        <publishers>  
          <publisher></publisher>  
        </publishers>  
        <is_multimedia_enabled></is_multimedia_enabled>  
        <cde_contenttype></cde_contenttype>  
        <content_type></content_type>  
        <publication_date></publication_date>  
      </meta_data>  
    </add_update_list>
```

> **Note**: This file has different information available as compared to the Kindle Cloud Library. a conversion from XML to CSV would be necessary.

## Kindle Cloud Reader HTML file format example.
```html
    <DIV id="titles_inner_wrapper" style="font-size: 191.25px;">  
      <DIV id="B00DJI3HWS" class="book_container">  
        <DIV class="book_cover">  
          <IMG class="book_image book_click_area" src="https://images-na.ssl-images-amazon.com/images/P/B00DJI3HWS.01._SX255_SY255_TTXW_SCLZZZZZZZ_.jpg" title="I Bastardi di Pizzofalcone (Italian Edition)">  
          <DIV class="alt_title book_click_area"></DIV>  
        </DIV>  
        <DIV class="book_details">  
          <DIV class="book_title book_click_area">I Bastardi di Pizzofalcone (Italian Edition)</DIV>  
          <DIV class="book_author book_click_area">Maurizio de Giovanni</div>  
        </DIV>  
      </DIV>  
    </DIV>    
```

> **Note:** Wait for all of your books to be loaded into the browser window, then view it's source and copy the DIV section that start with ID of titles_inner_wrapper.   
This isn't the best choice as clean up of the HTML will take so much more time, and doesn't lend itself to a simple parsing program.

# Using ExportKindleCSV.js for CSV File or ExportKindleTSV.js for TSV File
First, modify the query to support your book list requirements based on the query options available above. 

    let url = domain + 'kindle-library/search?query=&libraryType=BOOKS' + ( paginationToken ? '&paginationToken=' + paginationToken : '' ) + '&sortType=recency&querySize=50'

Second Modify these two line to reflect your data form the data elements to be collected in the CSV creation (This example has ASIN, Title, & Authors):

    let csvData = "ASIN,Title,Author(s)\n"
    csvData += item.asin + '","' + item.title + '","' + item.authors + '\n'

Make sure you put the '\n' at the end or all items will be on one row following the header created in the first line.  

Go to your [Kindle Cloud Reader](https://read.amazon.com/kindle-library) in Chrome, or FireFox. Open the Developer Console.  
* In FireFox useTools > Web Developer (Ctrl+Shift+C)  
Paste the code block in and then select RUN.  A dialog box will open when the download completes so you can save or open the new CSV file. Go check out your saved CSV file in the Downloads folder of the current user!  

> **Note**: Run this script in FireFox’s Multiline mode. 

Here's a screenshot of the console as it's running:

[![Script Running](https://github.com/MrMikey59/Kindle-Book-List/blob/main/ScriptRunning.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/ScriptRunning.png) 

Here's a screenshot of the Save File dialog box:

[![Save the File](https://github.com/MrMikey59/Kindle-Book-List/blob/main/SaveFileDialog.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/SaveFileDialog.png) 

Screenshot in Windows File Explorer showing the Saved file (Your name will vary!):

[![Saved File](https://github.com/MrMikey59/Kindle-Book-List/blob/main/FileExplorerScreenShot.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/FileExplorerScreenShot.png) 

I've included a test run file that I created for my 17000+ book Library.  Note, only the first 10000 books were gathered. This appears to be a limitation of the AMAZON data exchange, not the script.  I've included a Raw Object file that I copied from the console for those that like to look under the hood!

Screenshot of FireFox's Console where I collected the [Raw Object data](https://github.com/MrMikey59/Kindle-Book-List/blob/main/RawObject.txt)  (Pink):

[![Raw Object Data Collection](https://github.com/MrMikey59/Kindle-Book-List/blob/main/FFBrowserConsole.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/FFBrowserConsole.png) 

> **Note**: You can use multiple sort options to get a broader listing and then combine the files and remove duplicates.

### Second Running of Script?

Script cannot be run consecutively.  Refresh your browser page and then rerun. If you do run it again without the refresh, here’s the clue:

[![Script Run Error](https://github.com/MrMikey59/Kindle-Book-List/blob/main/SecondRunError.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/SecondRunError.png) 

> **Note**: You will not lose the console or the script when you refresh.

> **Note**: Using the TSV version solves the problem with title or authors using commas, but adds steps to convert it to a proper spreadsheet by importing it into Excel or Google Sheets.

### Change App Default for TSV File

Using Windows Settings, I tried to change the Default App to Excel, but it's not possible:

[![Change TSV Default App](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVFileDefaultApp.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVFileDefaultApp.png)

And the Windows Store doesn't recognize the file type!  So I went to the trouble to "Open With" then selecting more apps till I seek out the Excel.exe file.  I selected that and the change was made to my "Open With" selections.  And now I see these: 

From File Explorer Context Menu:  
[![From File Explorer Context Menu](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVOpenWith1.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVOpenWith1.png)

From Choose Another App:  
[![From Choose Another App](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVOpenWith2.png)](https://github.com/MrMikey59/Kindle-Book-List/blob/main/TSVOpenWith2.png)

Note: I recommend not making it a permanent change because you can review the file in NotePad the way it is...and import to a spreadsheet when you are ready.

# Using the Kindle Directory Small or Medium for ASIN's 

After copying the small directory to my desktop, I used [Karen' Print Directory](http://www.karenware.com/) to create a text file of the file names. I deleted most of the header created by Karen's PD but left the odd names for you to review. The file [DirPrntInfo.txt](https://github.com/MrMikey59/Kindle-Book-List/blob/main/DirPrnInfo.txt) is available for your underhood review.   
Now, with a list of ASINs, you can use some of the other GIT projects to acquire further details about the books.  With >10000 books, this may be an easier way to get the data than the previously tested idea above.

After completing the header removal, the extraneous non-ASIN file listings and the "FILE      " lead in to each ASIN, I now have the functional [ASINList.txt](https://github.com/MrMikey59/Kindle-Book-List/blob/main/ASINList.txt).


# Using the Kindle Sync Metadata Cache XML File from Kindle for PC

I copied the file into a Word temperary document. I Prettied with an edit replace to see all the lines in a more readable format using :
    Search Field: *><*  
    Replace Field: *>^p  <*  (Note I put two spaces there to add an indent.)  
This process took >45 minutes with my ~17,600 records - causing some ~270,000 replacements and that was just the first pretty step, now with ~7800 pages!

Due to size, I deleted many <meta_data>...</meta_data> entries to make the file usable for testing - also a just little time consuming! Using *[CTRL] + [SHIFT] + END* from a high point at at <meta_data> markermade selection and cursor back up the an end tag made the mass deletion a lot easier in WORD!

*NOTE*: This is a research project, so I did all of the above in WORD to see how long it would take. I know it could be programatically done much faster.  ;)  

The prettier [KindleSyncMetadataCache.xml](https://github.com/MrMikey59/Kindle-Book-List/blob/main/KindleSyncMetadataCache.xml) is now ready for further XML to CSV or TSV processing.
      
### KindleSyncMetadataCache.xml

Basic details about the [file](https://github.com/MrMikey59/Kindle-Book-List/blob/main/README.md#kindlesyncmetadatacachexml-file-sample-format) needed for parsing
| Node | Element Used|  
| ---- | ---- |  
| Root Node | <add_update_list> |  
| Book Nodes | <meta_data>…</meta_data> |  
      
Using the add_update_list section with an XML DOCID:
|Child Node Index|Node ID|Notes|
| :---: | --- | --- |  
|0|ASIN|Amazon’s Standard Identification Number (Primary Key)|  
|1|title|May contain a series, commas, and colons| 
|2|authors|May contain multiple entries (<author pronunciation="">…</author>), commas and colons| 
|3|publishers|May contain multiple entries (<publisher>…</publisher>), commas and colons| 
|4|publication_date|Left(<Node>,10) trimming required.| 
|5|purchase_date|Left(<Node>,10) trimming required.| 
|6|textbook_type|| 
|7|cde_contenttype|EBOK| 
|8|content_type|MIME: `application/x-mobipocket-ebook`| 

Use:
```vbscript
  Set xmlDocRoot = xmlDoc.childNodes(1)
' Get to the add_update_list
  Set xmlDocRecord = xmlDocRoot.childNodes(1)
' Set the Root for the Record Set
  Set xmlDocList = xmlDoc.getElementsByTagName("meta_data
```
With the full XML package as saved by Kindle for PC:

|Child Node Index|Node ID|Notes|
| :---: | --- | --- |
|0|?xml|<?xml version="1.0"?>|
|1|response||
|3|cache_metadata||
|4|add_update_list|Broken down above|

Use:
```vbscript
  Set xmlDocRoot = xmlDoc.childNodes(1)
' Get to the add_update_list
  Set xmlDocRecord = xmlDocRoot.childNodes(4) 
' Set the Root for the Record Set
  Set xmlDocList = xmlDoc.getElementsByTagName("meta_data")
```
      
## Using Excel 2017/O365

After updating the KindleSyncMetaqDataCache.xml, I used Excel 2016/O365 to import the data into the [spreadsheet My Kindle Books](https://github.com/MrMikey59/Kindle-Book-List/blob/main/My%20Kindle%20Books.xlsx) in the following process:
>1. File > Open, Choose the file location.
>2. Since no schema is provided, select to open the XML as a spreadsheet (first option)
>3. Review the file!

Problem I noticed:
- Sync Date & Time was added to every book. Deleted the column.  
- Columns headers were exactly the XML Element name. Attributes were also expanded into their own column, so I deleted them.  
- Mulitiple Authors/Publishers were treated to duplicate entries with repeated information from the book's other information.  
- Dates & Time are large details that will need trimming.  

My Conclusion - this is a viable option, Excel converts the file (17700+ Unique Books) quickly, but some serious massaging of the data is required to make this a **GOOD NORMAILIZED** database.
      
