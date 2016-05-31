---
layout: article
modified: 2016-05-26
title: Adding a Virtual Host for Local Development
categories: code
image:
  teaser: xampp-logo.png
---

I've been using XAMPP for local development on a Windows machine, but I wanted to store some projects in folders other than the default `htdocs` folder. After some research and trial and error, I came up with the following procedure.

Add the following lines to the bottom of `httpd.conf` (found in `xampp\apache\conf\`):

```
NameVirtualHost *:80

<VirtualHost *:80>
DocumentRoot "C:/xampp/htdocs"
ServerName localhost
</VirtualHost>
```

Then, for each project, add the following block, where `{path/to/project}` is the folder where your project files can be found, and `{project-name}` is how you want to refer to your project:

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
