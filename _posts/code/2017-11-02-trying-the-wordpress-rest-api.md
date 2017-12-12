---
title: Trying the WordPress REST API
modified: 2017-11-02
categories: code
image:
  teaser: 
---

Over the past few weeks, Rich and I have been adding to our WordPress photo gallery from this year's trip to Maine. After most of the photos were added, Rich asked if I could create a Google Doc to share with him so he could help to create and edit captions for the images. The document would contain each image's filename and its caption, if one existed, one per line. 

There was no way I wanted to do this manually for 255 images. I thought this would be a good opportunity to learn a little about the [WordPress REST API](https://developer.wordpress.org/rest-api/), which I've been hearing about lately but really know nothing about. 

It turns out that it's pretty simple to do what we wanted! 

First, I checked the response from this URL:

`https://[mysite.com]/wp-json/wp/v2/media?parent=1187`

That gives us a nice JSON-formatted list of the media whose parent ID is 1187. That's the ID of the page that contains our photo gallery.

The first thing I noticed was that there are only 10 media items in the JSON response, while there are 255 images attached to the page. Ten must be a default limit for the API response. We'll have to look into increasing that limit. Also, just judging by the image titles, I can see that they're not being returned in the proper order. That makes sense because the files weren't always uploaded or attached in the order in which they should appear in the gallery.

My plan was:
1. Figure out how to retrieve available data for all of the images attached to this particular page
2. Put them in the order that the images were taken
3. Output the filenames and any captions that currently exist. 

I don't need to use this in my theme, so just running it on localhost using XAMPP, and then copying and pasting the output into a Googe Doc, would be fine.

It's pretty easy to increase the number of items returned—but the API has a limit of 100 per page. That means that we'll need to make three separate requests to obtain all 255 images. Here's an example of what we'll need to do, of course changing the `page=1` to `page=2` and `page=3` as appropriate:

`http://[mysite.com]/wp-json/wp/v2/media?parent=1187&per_page=100&page=1`

That will work to obtain all 255 images. The next problem was how to order them. Looking at the [API Handbook](https://developer.wordpress.org/rest-api/) I could see that while `media` offers an `orderby` argument, the options don't include anything that would output the media items in the order in which they exist in the page's media gallery. We'll have to order them ourselves somehow. Let's look into the JSON response and see if there's anything we can use.

I see a key buried in there called `created_timestamp`. This should be perfect! We can throw all of our images into a big array and then order it by the time the images were created. No need to worry about pulling the order from the WordPress gallery, and no conflicts due to the four different cameras we were using to take the images (assuming that the time was set correctly set in all of the cameras, and it was).

The output should be pretty simple once we've obtained and sorted the information. We'll need the filename (which is stored in `slug`) and the caption, if there is one. In my case, I want to use `alt_text`. This is based on the fact that when I add media metadata in WordPress, I add it to both the title and alt text fields (your case may be different). I don't want to use the title field because WordPress automatically populates this with the media slug. I would prefer for images that don't yet have captions for the caption to appear blank, so I'm using `alt_text` instead.

So that's what I did here:

```php
<?php

  $json = file_get_contents('http://[mysite.com]/wp-json/wp/v2/media?parent=1187&orderby=id&order=asc&per_page=100&page=1');
  $response1 = json_decode($json);

  $json2 = file_get_contents('http://[mysite.com]/wp-json/wp/v2/media?parent=1187&orderby=id&order=asc&per_page=100&page=2');
  $response2 = json_decode($json2);

  $json3 = file_get_contents('http://[mysite.com]/wp-json/wp/v2/media?parent=1187&orderby=id&order=asc&per_page=100&page=3');
  $response3 = json_decode($json3);

  $full_response = array_merge($response1, $response2, $response3);

  foreach ($full_response as $key => $value) {
    $sorting_array[$key] = ['slug' => $full_response[$key] -> slug,
                    'alt_text' => $full_response[$key] -> alt_text,
                    'created' => $full_response[$key] -> media_details -> image_meta -> created_timestamp];
  }

function order_by_timestamp($a, $b) {
    return ($a['created'] <= $b['created']) ? -1 : 1;
  }

usort($sorting_array, "order_by_timestamp");

foreach ($sorting_array as $k => $v) {
    $sorted_array = substr($sorting_array[$k]['slug'], 0, -3) . "\t" . $sorting_array[$k]['alt_text'] . "\n";
    echo $sorted_array;
  }

?>
```

The first part obtains and decodes the JSON response into arrays. Then the arrays are merged into one large array. Then we make a separate array that contains only the keys and values we need (in this case, the slug (filename), alt_text (caption), and the timestamp for the image creation). Then we sort that array on the timestamp field, and print the filenames and corresponding captions to the screen.

How can we improve on this? Well, of course I don't like the hard-coding at the beginning. I would much prefer to use the API find out how many attachments exist, and then use this number to figure out how many times we need to make the call. So far I don't see a built-in way to do that.



