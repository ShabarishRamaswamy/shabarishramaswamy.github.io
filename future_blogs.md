Next ABVS protocol Blog:
- What are Byte-Addressable Media files

History of Video Playback
- HTML5 Video.
- Flash Player.
- Microsoft Silverlight.

* Go's static server implementation of Video Steaming.
  To know about Go's Video Implementation, we need to dive into Go's Source code \[Actually more like into Go's STD-LIB source code but the former sounds cooler].
  http.FileServer -> ServeContent -> serveContent
  * Challenges and Problems.
  * When is it good?
* Bandwidth.
* Bit Rates.
* Adaptive Bit Rate protocols \[A Primer].
* HTTP Content Ranges, Static Content Server's video streaming implementation in Go.
* HLS, DASH, MSS.
* Implementation of players.
* FFMPEG.
--
Set the method attribute to POST because file content can't be put inside URL parameters. - Who says?
[https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Sending_and_retrieving_form_data]
--
Cloudflare Pingora.
Traefik.
What won and what didn't -> HTTP, Gemini, Gopher and the bunch.
Hot Reloading.
APL, BQN and other weird languages.
Different types of Databases - SQL, Document, Table, Vector, etc.
gRPC and other high performance serialization protocols.