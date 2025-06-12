---
title: 'What''s up with Adaptive Bitrate Streaming? [Video Streaming in Go Part 3]'
slug: video-streaming-in-go-part-3
description: >-
  Learn about Adaptive Bitrate Video streaming protocols like MPEG-DASH & Apple
  HLS.
tags:
  - technical
added: 2025-06-12T07:00:00.000Z
---

# About Adaptive Bitrate Streaming.

We already have HTTP Content Range based video streaming, \[also known as Progressive Download], why do we even need Adaptive Bitrate Streaming protocols to do, basically, the same thing? Is this over-engineering?

Not so fast! Adaptive Bitrate Protocols were made to solve a different problem. They are not made for your average desktop PCs connected with Gigabit Ethernet. They were mainly designed to reduce the load and provide smooth\[Not Microsoft type :P] streaming for low powered and low/unstable internet connected devices.

# HLS - HTTP Live Streaming

Apple's HLS is based on a very simple idea. Rather than dividing the videos on byte-addressable spaces, what if we divide it based on time duration, say 10 seconds.

## It's a damn simple idea.

HLS builds on [MPEG-2 Transport Streams\[hence .ts files\]](https://www.itu.int/rec/T-REC-H.222.0-201808-S/en)(See ISO\_13818) or [MPEG-4 P14](https://www.loc.gov/preservation/digital/formats/fdd/fdd000155.shtml). The videos are made using H.264 and collated into an index file -> .m3u8;

Since the index\[or playlist] files are just addressing timestamps, the client can request for a higher or lower quality stream pretty easily. This is called [adaptive bitrate switching]()\[Aha!]

And because the switching is not based on a fixed metric, these protocols are called Adaptive Bitrate video streaming protocols.

[Big Idea: In HLS, the client is in control. It initiates the conversation and demands the content. Server only provides.]()

![](/assets/HLS-Architecture-Oversimplified.png)
(Image Above): HLS client server architecture oversimplified, adapted from [\[5\]](https://www.rfc-editor.org/rfc/pdfrfc/rfc8216.txt.pdf).

Explanation: <br/>
M3U8</u> = Manifest containing metadata about the content.<br/> <u>Segments</u> = Actual binary contents of the file.<br/> <u>Heuristics</u> = The Client-Side logic which pulls in media based on its metrics \[say bandwidth].<br/> <u>Media Player</u> = The final application that displays the media.<br/>

# MPEG-DASH

This was made by the Open Source community as an alternative to Apple controlled Open Source HLS protocol.

Compared to HLS, MPEG-DASH is much more flexible. This flexibility also leads to the specification being more complex.

MPEG-DASH follows a client-server model while defining its <u>data structures</u>. It is expected that client will be separate from the server and that, their communication will primarily happen using the [HTTP protocol](https://developer.mozilla.org/en-US/docs/Web/HTTP).

While DASH does not require re-encoding of the video, it does require the generation of some specification related data structures that the encoding layer should generate. These data structures will later be used by the <u>client to query data from the server.</u> <br/><br/>
[Big Idea: In MPEG-DASH, the client is in control. It initiates the conversation and demands the content. Server only provides.]()

![](/assets/2-DASH-Client-Server-Simplified.png)
(Image Above): MPEG-DASH client server architecture oversimplified, adapted from [\[1\]](https://www.mpeg.org/standards/MPEG-DASH/).

Explanation: <br/> <u>MPD</u> = Manifest containing metadata about the content.<br/> <u>Segments</u> = Actual binary contents of the file.<br/> <u>Heuristics</u> = The Client-Side logic which pulls in media based on its metrics \[say bandwidth].<br/> <u>Media Player</u> = The final application that displays the media.<br/>

# Biggest Differences

At this point, you might be wondering both of these protocols sound pretty similar, what's the actual difference between them, and how does one make a choice.

| Core Distinctions | HLS                                               | DASH                                         |
| ----------------- | ------------------------------------------------- | -------------------------------------------- |
| Codecs            | H.264, HEVC.                                      | H.264, HEVC, VP9, AV1, etc.                  |
| DRM               | Apple Airplay.                                    | Any DRM standard.                            |
| Adaptive Logic    | In control of the HLS client provider \[Browser]. | In control of the client itself \[Frontend]. |
|                   |                                                   |                                              |
| Live Streaming    | Updating the index file.                          | Explicit mechanisms for Live Streaming.      |
|                   |                                                   |                                              |
| Ideology          | Apple Controlled.                                 | FOSS.                                        |

# Other notable protocols which will be elaborated in a further blog:

* Microsoft Smooth Streaming.
* Real-time Transport Protocol (RTP).
* Adobe HTTP Dynamic Streaming.

# Sources

1. [MPEG White Paper](https://www.mpeg.org/standards/MPEG-DASH/)
2. [MPEG-2 Transport Streams\[hence .ts files\]](https://www.itu.int/rec/T-REC-H.222.0-201808-S/en)
3. [MPEG-4 P14](https://www.loc.gov/preservation/digital/formats/fdd/fdd000155.shtml)
4. [https://developer.mozilla.org/en-US/docs/Web/HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)
5. [HLS RFC](https://www.rfc-editor.org/rfc/pdfrfc/rfc8216.txt.pdf)
