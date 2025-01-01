---
title: 'Video Streaming in Go [Part 1]'
slug: video-streaming-intro
description: This series goes through how a generic video streaming website functions.
tags:
  - technical
added: 2025-01-01T18:30:00.000Z
---

I have been messing around with video streaming tech over the past few months for [GoSeek](https://github.com/ShabarishRamaswamy/GoSeek), this series of blogs are an account of my personal journey into discovering how video streaming works!

Before diving deep into the topic, we should answer some of these basic questions:

* How does the web handle data?
* What even is streaming?
* What is Video?
* How can I stream video? (Lmao recursion)

<br>
Let us start with the most basic question first:

# How does web handle data?

There are a number of ways data is shared on the web, most popular of which is [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview). But HTTP works on <u>text</u>. And <u>video data is clearly not text</u>!<br>

The answer to this lies in the spec itself. Although the request and response formats are text based, the payload data shared during the request or the response can be <u>anything</u>, refer [HTTP 1.0+ Content-Type](https://www.w3.org/Protocols/HTTP/1.0/spec.html#Content-Type). <br><br>

# Data can be anything, so what?

Our primary intuition can be to transfer video as <u>binary</u> data which can be decoded and displayed. This is easier said than done. This was the way in which videos were served pre [HTML5 Video](https://en.wikipedia.org/wiki/HTML_video) era. Back then, you had to use Flash Player or roll your own video decoder on the client side to even display videos, <u>Crazy Times!</u>.<br><br>

# What even is Video?

<u>It's just a bunch of images with an overlayed audio</u>. In reality, videos are much more complicated than that, having complex compression mechanisms, encoding and decoding steps, etc. We might or might not visit these steps in detail in this series. <br> <br>

# HTML 5 Video

The HTML spec now natively supports [video](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video). In the frontend you can specify a source [URI](https://datatracker.ietf.org/doc/html/rfc3986#section-1.1) and a [codec/type](https://en.wikipedia.org/wiki/Video_codec). The browser fetches this video and starts playing. <br><br>

## This is where things get interesting.

You can now run a static content webserver eg. [Go's FileServer](https://pkg.go.dev/net/http#FileServer) and the video should work as expected.

# Working Example

#### Go FileServer

This hosts the static files.

```go
http.Handle("/Static-Content/", http.StripPrefix("/Static-Content/", http.FileServer(http.Dir("./Static-Content"))))
```

All this says is, whenever you get a request for /Static-Content/\* just map it to the contents in the Static-Content folder.

E.g. GET /Static-Content/index.html serves the index.html

```
Static-Content
  index.html
  video.mp4
main.go
go.mod
```

Full Working Example:

```go
package main

import (
	"log"
	"net/http"
	"path/filepath"
	"text/template"
)

type VideoPath struct {
	VideoPath string
}

func main() {
	http.HandleFunc("/play-video", defaultImplementation)
	http.Handle("/Static-Content/", http.StripPrefix("/Static-Content/", http.FileServer(http.Dir("./Static-Content"))))
	log.Fatal(http.ListenAndServe(":5005", nil))
}

func defaultImplementation(w http.ResponseWriter, r *http.Request) {
	vp := VideoPath{VideoPath: "/Static-Content/video.mp4"}
	defaultImplementationPath := filepath.Join("./", "Static-Content", "default.html")

	template.Must(template.ParseFiles(defaultImplementationPath)).Execute(w, vp)
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML 5 Video</title>
</head>

<body>
    <div>
        <p>Static Video</p>
        
        <video controls id="video">
            <source src="{{.VideoPath}}" type="video/mp4">
            Your browser does not support the video tag.
        </video>
        
    </div>
</body>
</html>
```

Output:

![](</assets/Screenshot 2024-12-29 at 2.33.35 AM.png>)

# Is this it?

Nope!
This is just the beginning.

For the curious, you might be thinking, "But we didn't touch the video at all". And that's true. In this implementation we are not in control! The Static server does the work for us, but for a video streaming website, <u>WE NEED TO BE IN CONTROL!</u> <br><br>

In the following blogs we will cover:

1. How Static Sites actually send the video.
2. HTTP Content Ranges.
3. Video Encoder/Decoders (Codecs).
4. Adaptive Bitrates.
5. Streaming Standards like Apple HLS, Microsoft Smooth, MPEG-DASH. <br>

# and much more.

So stay tuned for the next week's blog!
