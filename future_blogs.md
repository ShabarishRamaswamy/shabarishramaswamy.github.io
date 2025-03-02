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
Cloudflare Pingora.
Traefik.
What won and what didn't -> HTTP, Gemini, Gopher and the bunch.