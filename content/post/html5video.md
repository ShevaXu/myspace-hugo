+++
date = "2014-12-23T22:19:47+08:00"
draft = false
title = "Embracing videos with Html5"
subtitle = "subtitle"
categories = ["Technology", "Development"]
tags = ["html5", "video", "javascript"]
+++

HTML5 comes with the *video* element. Here's its usage:

```html
<video width="640" height="360" controls>
  	<source src="movie.mp4" type="video/mp4">
  	<source src="movie.ogg" type="video/ogg">
  	<source src="movie.webm" type="video/webm">
	Your browser does not support the video tag.
</video>
```

<!--more-->

And that's what it looks like:

{{% html5video src="https://www.paypalobjects.com/webstatic/mktg/videos/PayPal_AustinSMB_baseline" %}}

The multiple sources/formats (the only 3 supported) are for different [browsers](http://www.w3schools.com/html/html5_video.asp). If indeed different movies are provided, the *video* element plays the first recognized one.

### What's missing?

You probably find it ugly and too simple for a modern video player. It should at least shows something, say a preview picture, before being played, and it would be better if it can play subtitle as well. All those require the player to be more controllable, and that's where *JavaScript* comes in ... 

## Video.js

When you Google *html5 videos*, **Video.js** is the top answer you get. After seeing its beautiful [website](http://www.videojs.com/) and easy-to-use features, you would not doubt such ranking. It even ships with an online [designer](http://designer.videojs.com/) to help you stylish your player. And it's [open sourced](https://github.com/videojs/video.js).

Embeding this player just needs a couple lines of html, and I won't show it here since you spot it right after you go to its website. What I show here is the **shortcode** I write to embed it in *Hugo* rendered Markdown:

```html
<!-- Should find a way to put it in the head when needed -->
<link href="http://vjs.zencdn.net/4.10/video-js.css" rel="stylesheet">
<script src="http://vjs.zencdn.net/4.10/video.js"></script>
<!-- The video -->
<video class="video-js vjs-default-skin" controls preload="auto"
width="640" height="360" {{ with .Get "poster" }} poster="{{ . }}" {{ end }} data-setup='{}'>
	{{ with .Get "src" }}
	<source src="{{ . }}.mp4" type="video/mp4" />
	<source src="{{ . }}.webm" type="video/webm" />
	{{ end}}
	{{ with .Get "vtt" }}
	<track kind="captions" label="English" src="{{ . }}" srclang="en" default />
	{{ end }}
<!-- <track kind="captions" src="http://example.com/path/to/captions.vtt" srclang="en" label="English" default> -->
<p class="vjs-no-js">
	To view this video please enable JavaScript, and consider upgrading to a web browser
	that <a href="http://videojs.com/html5-video-support/" target="_blank">supports HTML5 video</a>
</p>
</video>
```

And the code to call it is even simpler:
```
{{ % videojs src="https://www.paypalobjects.com/webstatic/mktg/videos/PayPal_AustinSMB_baseline" poster="http://paypal.github.io/accessible-html5-video-player/media/poster_PayPal_Austin2.jpg" vtt="/media/captions_PayPal_Austin_en.vtt" % }}
```
*NOTE* that there should be no space between `{{` and `%`, I separate them above to prevent *Hugo* manipulate it instead of showing it as codes. That's what the *shortcodes* [docs](http://gohugo.io/extras/shortcodes/) do and it fools me for a while.

And the player now has a poster and shows subtitle:

{{% videojs src="https://www.paypalobjects.com/webstatic/mktg/videos/PayPal_AustinSMB_baseline" poster="http://paypal.github.io/accessible-html5-video-player/media/poster_PayPal_Austin2.jpg" vtt="/media/captions_PayPal_Austin_en.vtt" %}}

### px-video.js

Another player I found was the *Accessible HTML5 Video Player*, aka px-video.js. You can see its demo [here](http://paypal.github.io/accessible-html5-video-player/). Actually, the movie/subtitle in this article are from this demo. It's also [Github](https://github.com/paypal/accessible-html5-video-player) open sourced. 

It provides similar functionalities but different style:

{{% pxvideojs src="https://www.paypalobjects.com/webstatic/mktg/videos/PayPal_AustinSMB_baseline" poster="http://paypal.github.io/accessible-html5-video-player/media/poster_PayPal_Austin2.jpg" vtt="/media/captions_PayPal_Austin_en.vtt" %}}

The shortcodes and calling are similar too.

To compare, *px-video.js* requires additional initialization script and did not comes with beautiful website and designing tools, which gives *video.js* a head lead for now.

### TODO

The subtitle ([.vtt](http://dev.w3.org/html5/webvtt/) format) now suffers the [*same origin policy*](http://en.wikipedia.org/wiki/Same-origin_policy) problem, so I have to save it locally before going deep into [CORS](http://enable-cors.org) support.

In the shortcodes, the height and width can also be specified. And I would definitely try to access and play with the images from the video content. 