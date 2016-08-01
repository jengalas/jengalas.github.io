---
title: Journal Search Pages Update
modified: 2015-10-10
categories: code
image:
  teaser: 
---

The library's Journal Search pages (and the related Citation Linker) were badly in need of an update. From their appearance, I'd estimate that they were designed in the early 2000s (and I'm being generous). Part of the reluctance to update the styles was due to the fact that, for some reason, changes to the Serials Solutions platform only go live once a day. There is no way to make a change and test it, and then iterate until you get what you want. (So in a way it prevents cowboy coding, but because there is no test server or development environment, and you're modifying an existing system with existing styles that need to be overriden, there's also no good way to make a reliable mockup.)

Let's take a trip back to, say, 2003 ...

<figure>
  <img src="/images/code/journal-title-search-old.png" title="Original Journal Search page" style="border: 1px solid #eee;">
  <figcaption>Original Journal Search page</figcaption>
</figure>

<figure>
  <img src="/images/code/citation-linker-form-old.png" title="Original Citation Linker form" style="border: 1px solid #eee;">
  <figcaption>Original Citation Linker form</figcaption>
</figure>

It got to the point, as you can imagine, where I couldn't stand to look at the pages. The pastel color scheme and obnoxious JavaScript dropdown menus were, frankly, an embarrassment. It was time to take on the challenge of a redesign.

First I polled the Reference librarians, who are some of the most frequent users of the journal search pages. I asked:

1. Do the pages need titles? What are the cases in which someone could come to this page out of context and be confused as to what it is? The tab title is currently "Full Text Electronic Journal List." In any case, this should probably be changed.
2. Do we need the A-Z links?
3. Do we need the "search titles" index? I already took the liberty of switching over to page numbers, which is at least as useful and much less cluttered.
4. Is the "Get Article Linker URL" link useful? Keep in mind that it links only to the results page and not to any particular article.
5. What about the randomly-placed "Search for full-text journals" at the bottom of the Citation Linker form and search results?

Compiled responses, among others, were:

1. Use the title "Periodicals Search, to match my.scranton (this was eventually voted down based on the language currently used in bibliographic instruction classes, but we may revisit it)
2. No; remove. They are not useful because there are hundreds of journals beginning with each letter, so it's a very inefficient way that no one would use to search. Also, the results are paginated, so there isn't even a way to bring up a complete list of titles beginning with a particular letter and use the browser search function to find the journal.
3. Page numbers are much more useful
4. Not useful; remove
5. Not useful; remove

This was a great help in determining what functionality we needed to keep, what should be emphasized for ease of use, and what is useless clutter and could be removed. The librarians also helped a lot with the choice of wording for the labels and titles to make them consistent with what they teach in information literacy courses. 

Next, I experimented with colors and styles to build a clean, minimal, easy-to-navigate site that uses colors and other branding elements consistently with our library home page, University portal, and other aspects of our public-facing web presence. Changes were made in the Serials Solutions/360 client. 

The header and footer are clean and simple:

Header

```html
<div id="wrap">
    <div id="header" style="background-image: url(http://www.scranton.edu/academics/wml/images/serials-solutions-background.png);">
        <div id="logo">
            <img src="http://www.scranton.edu/global/images/scranton-logo.jpg"> <span id="name"><a href="http://scranton.edu/library/">Weinberg Memorial Library</a></span>
        </div>
    </div>

<div id="main">
```

Footer

```html
        </div>
    </div>
</div>
<div id="footer">
    <p class="contact-info"><a href="https://www.scranton.edu/library">Weinberg Memorial Library</a> <span class="divider">|</span> <a href="https://www.scranton.edu/">The University of Scranton</a> <span class="divider">|</span> Scranton, Pennsylvania 18510-4634 <span class="divider">|</span> 570-941-7525</p>
</div>

```

The styling comes from a single css file:

```css
/***************************
****************************
  Global styles
****************************
***************************/
* {
  margin: 0;
  padding: 0;
}
html, body {
  height: 100%;
}
body {
  font-family: Arial, sans-serif;
  font-size: 16px;
}
a {
  color: #007093;
  font-weight: bold;
}
a:hover {
  color: #e9a73e;
}
#wrap {
    min-height: 100%;
}
#main {
    overflow:auto;
    padding-bottom: 140px;
    max-width: 960px;
    margin: 0 auto;
}  /* must be same height as the footer */
#header, #footer {
  background-color: #3b3365;   
  background-image: url(http://www.scranton.edu/academics/wml/images/serials-solutions-background.png);
}
#logo {
  max-width: 960px;
  margin: 0 auto; 
  padding-top: 18px; 
  padding-bottom: 18px;
}
#logo img {
  vertical-align: middle;
}
#name a {
   font-family: "Noto Sans", Arial, sans-serif;
   font-size: 38px; 
   font-weight: normal;
   color: #fff; 
   padding-left: 24px; 
   padding-top: 8px;
   text-decoration: none;
}
#name a:hover {
  color: #e9a73e;
}
#footer {
    position: relative;
    margin-top: -140px; /* negative value of footer height */
    height: 140px;
    clear:both;
    width: 100%;
} 
/*Opera Fix*/
body:before {/* thanks to Maleika (Kohoutec)*/
content:"";
height:100%;
float:left;
width:0;
margin-top:-32767px;/* thank you Erik J - negate effect of float*/
}
p.contact-info {
  max-width: 960px; 
  margin: 0 auto; 
  padding-top: 20px; 
  color: #fff;  
}
#footer {
  font-size: 14px;
}
.divider {
  color: #bbb8ce;
}
#footer a {
  color: #fff;
}
/***************************
****************************
  360 Link
****************************
***************************/
input[type="submit"], input[id="CitationClear"], table.WideNoBorder:first-of-type a.AnchorButton {
  background-color: #f2f2f2;
  color: #333;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 3px;
  cursor: pointer;
  display: inline-block;
  font-size: 14px;
  font-weight: 400;
  line-height: 1.42857;
  margin-bottom: 0;
  padding: 6px 12px;
  text-align: center;
  vertical-align: middle;
  white-space: nowrap;
  text-decoration: none;
}
table.WideNoBorder:first-of-type a.AnchorButton {
  display: inline;
}
input[type="submit"]:hover, input[id="CitationClear"]:hover, td.CustomLinkGroup a.AnchorButton:hover {
  background-color: #d1d1d1;
  border-color: #bbb;
  text-decoration: none;
}
span.SS_searchTitle, table#JournalLinkTable > caption, span.CustomLinkGroupLabel {
  font-size: 22px;
  color: #666;
  font-weight: normal;
  padding-bottom: 0.5em;
  padding-top: 1em;
}
input#SS_CFocusTag, input.RefinerInput, select {
  height: 26px;
  font-size: 16px;
  display: inline-block;
  vertical-align: middle;
}
select#select_searchTypes, select#select_subjects {
  height: 32px;
  font-size: 16px;
  vertical-align: middle;
}
span#MoreOptions {
    display: none;
}
table.SS_CitationTable {
  background-color: #efefef;
  border: 1px solid #dfdfdf !important;
  padding: 0.5em;
  line-height: 1.1em;
}
table#JournalLinkTable {
  padding: 0.5em 0;
  line-height: 1.1em; 
  font-size: 1.1em;
  border: 0 none;
}
.NoteFlag {
  padding: 5px;
  background-color: #fff;
  border-radius: 2px;
}
.SS_CitationTable {
  margin-top: 0;
  margin-bottom: 0;
  padding-bottom: 1em;
}
span.SS_ContentHeading {
  font-size: 1em;
  color: #444;
}
td.ContentHeading:nth-child(3) span.SS_ContentHeading {
  padding-left: 24px;    
}
td.CustomLinkGroupLabel {
  padding-bottom: 0.6em;
}
td.ContentHeading {
  text-align: left;    
}
div#ArticleCL {
  text-align: left;    
}
div#DatabaseCL {
  padding-left: 24px;    
}
.SS_Text {
  padding-top: 0.6em;
  font-style: italic;
}
.RefinerFooter {
  padding: 1.5em 0;
}
div#ArticleCL.cl a:link, div#BookCL.cl a:link, div#crossRefLinkWrapper.cl a:link {
  color: #E24D12 !important;      
}
table.WideNoBorder:first-of-type {
  padding-bottom: 1.5em;    
}
table.RefinerForm {
  margin: 0;
  background-color: #efefef;
  padding: 0 1.5em 2em 0;
  border: 1px solid #dfdfdf;
}
td.RefinerLabel {
  text-align: left !important;
  padding-left: 16px;
}
input.RefinerInput {
  padding-left: 2px; 
  font-family: "Noto Sans", Arial, sans-serif;
  font-size: .95em;
}
div.SSRefinerBannerSubText {
  padding: 1em 0;    
}
div.searchForm_c {
  width: 860px;
}
input#CitationSubmit {
  background-color: #d8d8d8;
}
input#CitationSubmit:hover {
  background-color: #cacaca;
}
/***************************
  Hide Format/Genre options
***************************/
.RefinerRow {  
  display: none;    
}
.RefinerRow ~ .RefinerRow { /* But display next instance of .RefinerRow class */
  display: table-row;
} 
/*********************************************
  Hide journal search form at bottom of page
*********************************************/
div.SS_NoResults, div.SS_NoResults + div.searchForm {  /* Citation linker search form  */
  display: none;    
}
td.SS_Text, div.searchForm  {  /* Citation linker results view  */
  display: none;      
}
```

<h2>2015 version of the Journal Title Search page and Citation Linker form</h2>

<figure>
  <img src="/images/code/journal-title-search-new.png" title="" style="border: 1px solid #eee;">
  <figcaption>New Journal Search page (with results)</figcaption>
</figure>

<figure>
  <img src="/images/code/citation-linker-form.png" title="" style="border: 1px solid #eee;">
  <figcaption>New Citation Linker page</figcaption>
</figure>

