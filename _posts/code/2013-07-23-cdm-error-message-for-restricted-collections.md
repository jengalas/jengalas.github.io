---
title: Modifying CONTENTdm error message for restricted or unpublished collections
modified: 2013-07-23
categories: code
image:
  teaser: 
---

Quick post for tonight.  Our digital services librarian asked me to change the default error message that appears when an unauthorized user tries to access one of our collections that is available only on campus.  CONTENTdm's default message looks like this:

<figure>
  <a href="/images/code/default_error.gif" title="Default error message"><img src="/images/code/default_error.gif" title="Default error message"></a>
  <figcaption>Default error message</figcaption>
</figure>

We wanted to change the wording to be somewhat more friendly and also to provide an easy point of contact for users who have questions.

## Modifications

After a bit of grepping around on the server, I found the relevant piece of code in this file: 
    
`/usr/local/Content6/Website/public_html/ui/cdm/default/collection/default/resources/languages/cdm_language.xml`
    
and it is: 

```
<tu tuid="SITE_error_KEY_error_1">
    <tuv xml:lang="en_US">
        <seg>The collection "~CDMERRORCOLLECTION~" cannot be displayed. Log in and refresh the page to access restricted or unpublished collections.</seg>
    </tuv>
</tu>
```

However, this is not the file we want to modify because in this case, we want the change to apply only to one of our collections.  Good news is, there's a corresponding collection-level XML file for each collection.  This file can be found at:

`/usr/local/Content6/Website/public_html/ui/custom/default/collection/coll_[collection alias]/resources/languages/cdm_language_coll_[collection alias].xml`

> Note: if you don't have FTP or shell access to your server, you can download the file by accessing CONTENTdm's Website Configuration Tool, choosing your collection, clicking Tools in the left sidebar menu, and choosing Language.  Then click the "Download the current website interface config language file" link.

The **SITE_error_KEY_error_1** section does not appear by default in the collection-level XML file, but you can simply add it, and modify the text to your specifications. If including HTML, be sure to enclose it in `<![CDATA[ ]]>` tags.
    
This is what I added to my `cdm_language_coll_clippings.xml` file (if necessary, you can translate it and add a version for other languages too):


```
<tu tuid="SITE_error_KEY_error_1">
    <tuv xml:lang="en_US">
        <seg>The University of Scranton Newspaper Clippings collection is available for on-campus use only.  Please report any access issues or questions to <![CDATA[<a class="errorbox-link" href="mailto:digitalcollections@scranton.edu">digitalcollections@scranton.edu</a>]]>.</seg>
    </tuv>
</tu>
```

This is the resulting custom message:

<figure>
  <a href="/images/code/custom_error1.gif" title="Custom error message"><img src="/images/code/custom_error1.gif" title="Custom error message"></a>
  <figcaption>Custom error message</figcaption>
</figure>
    
You might notice that the small red "alert" icon is gone.  That's simple to hide by including the following in your collection's CSS file:

```
span.ui-icon-alert-cdmerror {   
    display: none;
}
```

The "errorbox-link" style and custom colors are defined in that file too.

## Uploading

The filename must remain `cdm_language_coll_[collection alias].xml`.  CONTENTdm doesn't seem to recognize it otherwise, and you'll probably get an error.  At least I have.  So make a backup copy first, but be sure that you're using that name for the file you upload.  
    
Uploading can be done through CONTENTdm's Website Configuration Tool.  Log in, choose your collection, navigate to *Tools* in the left sidebar menu, and choose *Language*. Then browse to find the file locally, and upload and publish it.
    
<figure>
  <a href="/images/code/xml_upload.gif" title="Uploading process"><img src="/images/code/xml_upload.gif" title="Uploading process"></a>
  <figcaption>Uploading process</figcaption>
</figure>
    
You'll have to log out again to test it.
