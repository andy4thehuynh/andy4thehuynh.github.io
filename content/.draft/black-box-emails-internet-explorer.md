+++
draft = true
author = "Andy Huynh"
date = "2016-08-27T17:26:11-07:00"
title = "Black box mailer"
+++

If anyone encounters a black screen when sending mail via Rails, I got a sanity check for you. Go in to your mailer css and look at any colors you have.

Make sure you have six characters for each color:

```
background-color: #fff;       // is wrong.
background-color: #ffffff;    // is correct.
```

This was the color of the table. HTML tables read background colors and spits it as a `bgcolor` property **on the table**, not a style. The black box occurs mostly in Internet Explorer. We found this solution when we ripped the hashtag from bgcolor like so:

```
bgcolor: "#fff"               // black box in IE only.
bgcolor: "fff"                // black box in all browsers.
```
