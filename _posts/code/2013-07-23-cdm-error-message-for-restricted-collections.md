---
title: Modifying CONTENTdm error message for restricted or unpublished collections
modified: 2013-07-23
categories: code
image:
  teaser: 
---

Quick post for tonight.  Our digital services librarian asked me to change the default error message that appears when an unauthorized user tries to access one of our collections that is available only on campus.  CONTENTdm's default message looks like this:


We wanted to change the wording to be somewhat more friendly and also to provide an easy point of contact for users who have questions.

## Modifications

After a bit of grepping around on the server, I found the relevant piece of code in this file: 
    
`/usr/local/Content6/Website/public_html/ui/cdm/default/collection/default/resources/languages/cdm_language.xml`
    
and it is:
    
<pre class="prettyprint lang-html">
    &lt;tu tuid=&quot;SITE_error_KEY_error_1&quot;&gt;
      &lt;tuv xml:lang=&quot;en_US&quot;&gt;
        &lt;seg&gt;The collection &quot;~CDMERRORCOLLECTION~&quot; cannot be displayed. Log in and refresh the page to access restricted or unpublished collections.&lt;/seg&gt;
      &lt;/tuv&gt;
    &lt;/tu&gt;
</pre>


```
&lt;tu tuid=&quot;SITE_error_KEY_error_1&quot;&gt;
    &lt;tuv xml:lang=&quot;en_US&quot;&gt;
        &lt;seg&gt;The collection &quot;~CDMERRORCOLLECTION~&quot; cannot be displayed. Log in and refresh the page to access restricted or unpublished collections.&lt;/seg&gt;
    &lt;/tuv&gt;
&lt;/tu&gt;
```

However, this is not the file we want to modify because in this case, we want the change to apply only to one of our collections.  Good news is, there's a corresponding collection-level XML file for each collection.  This file can be found at:

`/usr/local/Content6/Website/public_html/ui/custom/default/collection/coll_<em>[collection alias]</em>/resources/languages/cdm_language_coll_<em>[collection alias]</em>.xml`

> Note: if you don't have FTP or shell access to your server, you can download the file by accessing CONTENTdm's Website Configuration Tool, choosing your collection, clicking Tools in the left sidebar menu, and choosing Language.  Then click the "Download the current website interface config language file" link.

The *SITE_error_KEY_error_1* section does not appear by default in the collection-level XML file, but you can simply add it, and modify the text to your specifications. If including HTML, be sure to enclose it in *<![CDATA[ ]]>* tags.
    
This is what I added to my `cdm_language_coll_clippings.xml` file (if necessary, you can translate it and add a version for other languages too):


```
<VirtualHost *:80>
    DocumentRoot "{path/to/project}"
    ServerName {project-name}
    <Directory "{path/to/project}">
    Options Indexes FollowSymLinks ExecCGI Includes
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>
```

Then add the following lines to your Windows `hosts` file (typically found in `Windows\System32\drivers\etc\`), again replacing `{project-name-n}` with the appropriate project name. Typically you'll have to run your text editor as an administrator in order to be able to save the changes. (If you're running your text editor as administrator and still are unable to save the file, you may have an antivirus program that's preventing you from making the change. You may need to momentarily disable it to save the file.)

```
# localhost entries
127.0.0.1       localhost
127.0.0.1       {project-name-1}.test
127.0.0.1       {project-name-2}.test
```

Then restart Apache.

Now you can simply access `http://{project-name-n}.test` to reach the project of the same name. You can make bookmarks or shortcuts for quick access.

*Note: there's nothing special about the .test extension—you can use .local or anything else you like, or nothing. I prefer to use .test as a simple visual indicator that I'm working on my development version of a site. In addition, .test is already reserved for "Testing & Documentation Examples" via [RFC-2606](http://tools.ietf.org/html/rfc2606), so it won't be purchased by, say, Google exclusively for its own use [like .dev was](https://iyware.com/dont-use-dev-for-development/).*

### An example

The relevant lines of my httpd.conf currently look like this (I'm set up for working on two projects):

```
NameVirtualHost *:80

<VirtualHost *:80>
    DocumentRoot "C:/xampp/htdocs"
    ServerName localhost
</VirtualHost>

<VirtualHost *:80>
    DocumentRoot "C:/Users/Zhanna/Code/streetrailway"
    ServerName streetrailway.test
    <Directory "C:/Users/Zhanna/Code/streetrailway">
    Options Indexes FollowSymLinks ExecCGI Includes
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>

<VirtualHost *:80>
    DocumentRoot "C:/xampp/htdocs/PlanetZhanna/public_html/surveymarks"
    ServerName surveymarks.test
    <Directory "C:/xampp/htdocs/PlanetZhanna/public_html/surveymarks">
    Options Indexes FollowSymLinks ExecCGI Includes
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>
```

And my hosts file looks like this:

```
# localhost entries
127.0.0.1       localhost
127.0.0.1       streetrailway.test
127.0.0.1       surveymarks.test
```
