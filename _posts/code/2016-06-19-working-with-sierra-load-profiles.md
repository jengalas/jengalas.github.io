---
title: Working With Sierra Load Profiles
modified: 2016-06-19
categories: code
image:
  teaser: 
---

I work with an ILS, Sierra, that brings new and changed records into the database via "load profiles." In the two years since I took the load profile training session, I've created a handful of these profiles for various projects in the library, such as loading ebook and serials records.

Just for kicks, here's part of a load profile:

<figure style="display: inline-block;">
  <img src="/images/code/load-profile-sersol.png" title="Load profile excerpt">
  <figcaption>Load profile excerpt</figcaption>
</figure>

It looks like the mainframe stuff my dad worked on in the 70s and 80s. So it's very cool in a way, but also limited, and a huge shift in mindset from how we typically work today.

## Procedure Outline & Notes

* Working on test, find a load profile that is similar to the desired specs for the new table.
* Copy it to a new prefix that is indicative of the type of load it's for (this is just a label and means nothing special to the system, so make it as descriptive as possible).
* Make the necessary changes; typically they will be adding notes to a 500 line and adding a local holdings code to 049. Don't forget to make fixed field changes in the record template:
    * Create a new record template, if necessary, in the Sierra client and then tell the load table to use it in the line that contains `"dflt"@dflt="OCLCME"`. In this example, `OCLCME` would be the name of the record template. Replace it with the name of the template you're using. If you copied from a record that brings in the material type from the leader, you'll need to comment out this line: `L|| |6|1|b| |30|n|N|0|mat type`
* In some cases, we want to pull the bcode3 from a translation table (for example, in cases where we will be receiving MARC delete files and will need to overlay existing records with different codes). In this case, leave BCODE3 empty in the record template and use a translation table like `ssb3` (called from the line `L|| |5|1|b| |31|n|N|0|%map=("m2bmap.ssb3")`):

```
@stop_on_map=true
d|d
n|b
c|b
.*|b
```

* Often, the 856 field needs several changes, which are done through another translation table called from a line like: `856||+|0|0|b|y|0|y|N|0|%map=("m2bmap.ezpavon")`. Most of these changes are compatible with each other. For example, you can add subfields 3, u, and z easily using a map like this (`ezpavon`): 

```
@delimiter=!
@stop_on_map=false
@case=true
(.*)$0|z[^|]*(.*)$1!\0\1
(.*)$0!\0|3Academic Video Online|zClick here to connect to this video.
(.*)$0|u(http://)$1(.*)$2!\0|uhttp://rose.scranton.edu/login?url=\1\2
```

* Certain operations are not compatible. For example, a requirement for one table was to *Delete all 856 fields that do not have "SpringerLink" in &#124;3*. This must be done via a translation table for 856, but there were also requirements to add the proxy prefix to &#124;u and add text to &#124;z, and it turns out that these operations are mutually exclusive (one requires `@stop_on_map` to be true and one requires it to be false).  So we have to choose one or the other—and use Global Update to complete whichever process we decided not to do via the load table.  In this situation the best I can do is to create two `.m2btab` files, one that will use the &#124;3 map and one that will make the proxy and link changes, so that it's easy to choose whichever one you want to use for a particular load.
* Once the table is complete, it must be added to `m.marcload.local` so it will show up in Sierra's Data Exchange menu. Copy an existing line (Ctrl-D, Ctrl-U, Ctrl-U), change the initial to an unused letter, and substitute the `.m2btab` extension where required, following the example of the existing line.
* Restart the Sierra client, if it's currently running, so that `m.marcload.local` will be reloaded and the new profile will appear. In order to test the load profile, go to Data Exchange in the Function dropdown menu. Select **Load Records via Local Profiles**, then **Get PC** to upload the test marc file. Upload it with the `.lfts` suffix, prep, and then load a sample of the resulting `.lmarc` file (10-15 records is fine). 
* Spot check a few of the loaded titles by creating a list of the records created today. You can also use the public catalog search on the [test server](http://squillace.scranton.edu/), and check the MARC record display. Go through the specs one by one to make sure everything matches up. Then when everything looks correct, contact the cataloging librarian (or whoever requested the load profile) to do a final check.
* There is no good way to transfer the load profile from the test server to the production server. Basically you have to recreate it following the same process. 








