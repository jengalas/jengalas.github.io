---
title: Journal Search Pages Update
modified: 2015-10-10
categories: code
image:
  teaser: 
---

The library's Journal Search pages (and the related Citation Linker) were badly in need of an update. From their appearance, I'd estimate that they were designed in the early 2000s (and I'm being generous). Part of the reluctance to update the styles was due to the fact that, for some reason, changes to the Serials Solutions platform only go live once a day. There is no way to make a change and test it, and then iterate until you get what you want. (So in a way it prevents cowboy coding, but because there is no test server or development environment, and you're modifying an existing system with existing styles that need to be overriden, there's also no good way to make a reliable mockup.)

<figure>
  <img src="/images/code/journal-title-search-old.png" title="" style="border: 1px solid #eee;">
  <figcaption>Original Journal Search page</figcaption>
</figure>

<figure>
  <img src="/images/code/citation-linker-form-old.png" title="" style="border: 1px solid #eee;">
  <figcaption>Priginal Citation Linker page</figcaption>
</figure>

It got to the point, though, where I couldn't stand to look at the pages. The pastel color scheme and obnoxious JavaScript dropdown menus were, frankly, an embarrassment. It was time to take on the challenge of a redesign.

First I polled the Reference librarians, who are some of the most frequent users of the journal search pages. I asked:

1. Do the pages need titles? What are the cases in which someone could come to this page out of context and be confused as to what it is? The tab title is currently "Full Text Electronic Journal List." In any case, this should probably be changed.
2. Do we need the A-Z links?
3. Do we need the "search titles" index? I already took the liberty of switching over to page numbers, which is at least as useful and much less cluttered.
4. Is the "Get Article Linker URL" link useful? Keep in mind that it links only to the results page and not to any particular article.
5. What about the randomly-placed "Search for full-text journals" at the bottom of the Citation Linker form and search results?

Compiled responses were:

1. Use the title "Periodicals Search, to match my.scranton (this was eventually voted down based on the language currently used in bibliographic instruction classes, but we may revisit it)
2. No; remove. They are not useful because there are hundreds of journals beginning with each letter, so it's a very inefficient way that no one would use to search. Also, the results are paginated, so there isn't even a way to bring up a complete list of titles beginning with a particular letter and use the browser search function to find the journal.
3. Page numbers are much more useful
4. Not useful; remove
5. Not useful; remove

Changes were made in the Serials Solutions/360 client. 


<figure>
  <img src="/images/code/citation-linker-form.png" title="" style="border: 1px solid #eee;">
  <figcaption></figcaption>
</figure>



<figure>
  <img src="/images/code/journal-title-search-new.png" title="" style="border: 1px solid #eee;">
  <figcaption></figcaption>
</figure>

