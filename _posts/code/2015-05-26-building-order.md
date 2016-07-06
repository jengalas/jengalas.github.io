---
title: Building Order&#58; Using Free Tools to Build a Simple App and Streamline Workflows
modified: 2015-05-26
categories: code
image:
  teaser: xampp-logo.png
---

This wasn’t a project that was assigned, or one that grew out of any particularly pressing need. Instead, the idea was formed, and the application was built, over time by experimentation and the serendipitous coalescing of several different needs, wants and observations.

Since 2001, the website of the Weinberg Memorial Library of the University of Scranton has contained a section of pages about the buildings on our campus. The pages were originally built by students in a first year Research and Writing course—a great idea!

However, when we were preparing for the Library’s 20th anniversary celebration in 2012, we discovered the web page about the Library building indicated that the donors’ names were “not known.” Yikes! The donors’ names are included in the full name of the library, so the claim that we didn’t know the donors’ names looked ridiculous, not to mention insulting to the people who had given us millions of dollars to build the library. We had no idea how long the information had been missing from the page—probably years.

I began to look around at the other campus buildings pages and found woefully out of date (and sometimes completely inaccurate) information. I discovered that a few people had submitted updates to our Special Collections department, and a very well-intentioned staff member was keeping a record of this updated information. However, it was being kept on hand-annotated printouts of the original web pages that rarely saw the light of day and never made their way to the Systems department. Two staff members in Library Systems are the only people in the library who can make changes to the web pages, so no updates were ever made and the pages languished in basically the same state in which they originated, back in 2001.

In short, there was no efficient workflow for updating these pages, so they sat until updating them became “an emergency.”

I duly made the update and added the donors’ names—crisis averted, for now. But how could we address the other problems with the pages? And how could we turn them into something to be proud of? Our digital collections now contain many articles and photos about these buildings. Wouldn’t it be great to come up with a way to integrate those articles and photos as supplementary information, to be able to add information quickly and easily as we discover it, to make an interactive map of our campus, past and present—and to allow anyone to help out?

Right around the same time, my colleague Kristen described a pilot program that she had in mind for a huge collection scrapbooks containing newspaper clippings about the University’s history. These scrapbooks are a goldmine of University history information, but the 97 large-format scrapbooks contain over 16,000 separate articles.

The project involved so many steps and so much content that Kristen decided to go outside the Digital Services and Systems departments and encourage all library departments to help out. This was an inspiration for me in two ways: not only did I like the vision of a collaborative project among many library staff members, but on a more practical note it occurred to me that the people scanning and describing these newspaper articles from the scrapbooks would be coming across all sorts of informational tidbits about our campus buildings.

Wouldn’t it be great if we could capture that information at the moment they discover it—and even better, if they could update the web pages themselves, without needing any special training or access permissions?

I came across a JavaScript library that makes it very easy to use Google Spreadsheets as a lightweight CMS, and I ended up piecing together a small app that allows anyone working on the project to update the spreadsheet as soon as they discover new information, and to publish their change immediately (or nearly so) on the live website. Since we began using the app, we’ve had hundreds of updates to the pages, including live links to photos and articles in our digital collections. I was also easily able to extend the app by mapping the buildings’ locations.

There are so many great tools out there for free, and I’m grateful to their authors for giving their time and talents to projects that make life easier for the rest of us!

## Building Up: Creating the App

This post outlines the creation of the campus buildings app using Google Spreadsheets, Tabletop.js, and Handlebars.js. I’ll try to explain everything clearly enough so that you can modify the process for your own projects.

> This project was built for CONTENTdm, but if you’re running it on a regular webserver you’ll just need to add the Tabletop and Handlebars scripts to your HTML as you normally would, and jQuery as well if you want to use it (it is not required by Tabletop, although it is required to run some of the code I have below. Just an FYI if you’re mostly copying and pasting.).

### Where we're headed

This example shows how we can take data from a Google Spreadsheet that looks like this:

spreadsheet.png

And *dynamically generate* a page of this form for each building on our campus:

wml-page-2015.png

### Getting started

There are two basic components to this app:

* The Google Spreadsheet, where you’ll store and update your data
* The HTML file that generates and formats your output (in this case, a page for each building)

#### The raw materials

##### Spreadsheet

Here is an excerpt from our Google Spreadsheet. You can see how the columns correspond to the various data points that we want to appear on our building page. This is the only file that your staff and students will have to update in order to publish their information to the web!

spreadsheet.png

1. Go to Google Drive and create a new spreadsheet. In order to set up your columns, start by analyzing the data you think you’re going to want to collect. You can always add, remove or modify columns later, but it helps to have a basic idea when you start.
> In our case, this step was pretty easy, because we had several earlier versions of campus buildings pages to guide us in the types of information we wanted to include.
2. Then you’ll want to add some data—sample data is fine if you don’t have any real data to work with yet.
3. Make sure you publish your spreadsheet to the web by going to *File > Publish to the Web* while you’re in your spreadsheet. This will ensure that your script (that you’re going to write next!) will be able to access your spreadsheet.
> This is also where you will find your published spreadsheet’s URL; make note of this for later.

##### Script libraries

Now that your spreadsheet is set up, you’ll need to download two script libraries:

* [Tabletop.js](https://github.com/jsoma/tabletop)
* [Handlebars.js](http://handlebarsjs.com/)








