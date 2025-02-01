---
title: 'Video Streaming in Go [Part 2] - Client Request, HTTP Ranges'
slug: video-streaming-in-go-part-2
description: 'Learn about the <Video> tag, HTTP Content Ranges and more.'
tags:
  - technical
added: 2025-02-01T08:00:00.000Z
---

This is the second part in a series of blogs on Video Streaming. Check out the first part: [Video Streaming in Go \[Part 1\]](/post/video-streaming-intro/)

# What have we learned so far?

We can "stream" video using a static server. But we are not in control of anything!

# Next Steps

1. Somehow send data ourselves and not abstract it away.
2. Account for Bandwidth and other video constraints.
3. Maybe code custom input methods like Speed-Up, Slow-Down, change quality.
   etc.

But all of these are high level objectives. We need a good base and build up to these. <br> <br>

# The HTML Video Element

The first key to controlling everything is understanding the <b><u>output field</u></b>. Almost all web based video playback uses the [HTML5 Video](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video) element.

You can technically make your own video player but it's more hassle than what it's worth. I plan to cover what used to be done pre-HTML 5 Video days in another blog.

For now, the only thing we care about is the fact that the Video Element takes in a [URL](https://webmasters.stackexchange.com/questions/19101/what-is-the-difference-between-a-uri-and-a-url) and a [MIME Type](https://developer.mozilla.org/en-US/docs/Web/Media/Formats).<br> <br>

# Simple way to use the \<video> element

[From MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video):

```html
<video controls width="250">
  <source src="/media/cc0-videos/flower.mp4" type="video/mp4" />
</video>
```

You specify the <u>source</u> and the <u>MIME-Type</u> and the browser requests the said media.<br><br>

Now, we will see a more comprehensive example:

```html
<video controls width="250">
  <source src="/media/cc0-videos/flower.webm" type="video/webm" />

  <source src="/media/cc0-videos/flower.mp4" type="video/mp4" />

  Download the
  <a href="/media/cc0-videos/flower.webm">WEBM</a>
  or
  <a href="/media/cc0-videos/flower.mp4">MP4</a>
  video.
</video>
```

Here, we list the MIME-Type in <u>descending order</u> of importance. I.e Given that a client has all the [codecs](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video_codecs) listed above, it will choose <u><b>WebM first</b></u>. <u>If</u> it does not have the WebM codec <u>then only</u> will it choose the MP4 format to stream.

# HTTP Content Ranges

* What [request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) does the video element send to the server?

The answer is, a request which has a <u>Content-Range</u>. The specifics of the request is down to the browser. Some servers cannot handle Content Ranges, so they will send the entire file in a single response. Not only is this slow, but also memory inefficient. Video files can be many GBs.

Usually, browsers will send a [GET](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET) request when requesting a piece of media.

![GET Request in Firefox Video Element](/assets/2-1-Video-Get.png)
Here, we can see that Firefox is sending a GET request for the video.

#### Keen ones might notice the HTTP 206 response code, we will put a pin on it for now.

![](/assets/2-2-Vid-Req.png)
As you can see, the client sends a request for all the bytes. If the server has logic to handle Content-Ranges, it will send back the total size(bytes) of the video. <u>It becomes the client's responsibility to chunk the video and ask for the pieces in accordance to the user's bandwidth.</u>

![](/assets/2-3-Progressive-Download.png)

Here, the browser chunks the video and downloads 6.5 MBs of data. The browser has the choice to download more or less.

# How does Go handle this request?

This will be covered in the next blog, but for now, you can take a look at the Static Server implementation inside [net->http->fs.go](https://github.com/golang/go/blob/master/src/net/http/fs.go).

# Next:

* Go's static server implementation of Video Steaming.
* How to account for Bandwidth.
* Bit Rates.
* Adaptive Bit Rate protocols \[A Primer].
* HLS, DASH, MSS.
* Implementation of players.
* FFMPEG.

# Further Reading:

* The HTML 5 Video W3C standard: [https://www.w3.org/TR/2011/WD-html5-20110113/video.html](https://www.w3.org/TR/2011/WD-html5-20110113/video.html)
* HTML Video Encoding: [https://imagekit.io/blog/html5-video-encoding/](https://imagekit.io/blog/html5-video-encoding/)

# Readings related to upcoming Blogs:

* Digital Video Encoding Primer: [https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video\_concepts](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video_concepts)
