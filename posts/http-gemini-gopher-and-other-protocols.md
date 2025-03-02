---
title: 'A tale of Winners, Losers and Bystanders - HTTP, Gemini, Gopher and others.'
slug: http-gemini-gopher-and-other-protocols
description: How HTTP won the war and other protocols got buried!
tags:
  - technical
added: 2025-03-01T08:00:00.000Z
---

Who doesn't like HTTP. It is probably one of the best designed protocols. It is surprisingly simple for an industry standard!

At this point of time, almost 100% of internet traffic uses HTTP \[At least for webpages]. But this was not always the case. In early days of the internet, before the standardization of HTTP, there were many other protocols. We will take a look at some of them!

We will start with the smaller ones.

#### Note: Examples in this document may or may not be correct, it is based on my very limited understanding of these obscure protocols.

# WAIS & Z39.50

WAIS was created by Thinking Machines Inc. as a universal database server and terminal interface, it used the ANSI/NISO Z39.50 protocol. The Z39.50 was one of the first standardized federated databases protocols.

It is a <b>client-server application layer protocol</b> mainly used in the databases domain. It came with a natural language query system known as the <b>Contextual Query Language</b> by virtue of which it allowed all of the WAIS Interfaces to talk to any of the WAIS Databases.

Initially Z39.50 was not compatible with TCP/IP as it was made for the OSI protocol. After a few iterations, they made it compatible with TCP/IP. Later, it was obsoleted by one of its replacements, which used HTTP to maintain backward compatibility with the Query Syntax.

> Z39.50 thus offers an excellent case study of the problems involved in managing the evolution of a standard over time. It may well offer useful lessons for the future of other standards such as HTTP and HTML, which seem to be facing some of the same issues. \
> \~ [D-Lib Magazine, April 1997](https://dlib.org/dlib/april97/04lynch.html).

Request/Respone

```
Request:
connect z3950.loc.gov:7090/voyager
search "library mashups"
show 0 1
quit

Response:
z3950.loc.gov:7090/voyager: 2 hits
0 database=VOYAGER syntax=USmarc schema=unknown
...
```

# Usenet/NNTP

The Network News Transfer Protocol \[NNTP] was a pre-web protocol for transferring usenet articles from one network to another. It used Unix-To-Unix Copy \[UUCP] before TCP/IP. With the rise of the web, it was re-codified to use SMTP like syntax over TCP/IP.

NNTP enables both server-server and client-server communication for exchanging and serving newsgroup articles. NNTP used IANA authorized TCP-NoTLS:119 and TCP-TLS:563.

Request/Response

```
Init Connection
...
Client: LIST
Server: 215 list of newsgroups follows
Server: net.server 00543 00501 y
Server: net.server2 10125 10011 y
Server: .
Client: GROUP net.server2
Server: 211 104 10011 10125 net.server2 group selected
...
```

# Gopher

The Gopher protocol was created to distribute text! \[Mostly-text based documents to be more precise]. It was the HTTP before the birth of HTTP. This protocol predates the web by mere months. This was perhaps the most dominant protocol for internet communication before HTTP became the standard in mid-to-late 90s.

It was created by the University of Minnesota to act as a campus wide public bulletin board. With the rise of TCP/IP and the web in general, Gopher grew exponentially. It was mainly created to distribute textual documents in a menu-driven way.

Failure to innovate under broader scope of the web \[serving images, videos and interactivity] paired with their decision to charge royalty for their implementation of a Gopher server led to the ultimate and quick demise of the Gopher protocol.

Request/Respone

```
/Reference
1CIA World Factbook     /Archives/mirrors/textfiles.com/politics/CIA    gopher.quux.org 70
0Jargon 4.2.0   /Reference/Jargon 4.2.0 gopher.quux.org 70      +
1Online Libraries       /Reference/Online Libraries     gopher.quux.org 70     +
1RFCs: Internet Standards       /Computers/Standards and Specs/RFC      gopher.quux.org 70
1U.S. Gazetteer /Reference/U.S. Gazetteer       gopher.quux.org 70      +
iThis file contains information on United States        fake    (NULL)  0
icities, counties, and geographical areas.  It has      fake    (NULL)  0
ilatitude/longitude, population, land and water area,   fake    (NULL)  0
iand ZIP codes. fake    (NULL)  0
i       fake    (NULL)  0
iTo search for a city, enter the city's name.  To search        fake    (NULL) 0
ifor a county, use the name plus County -- for instance,        fake    (NULL) 0
iDallas County. fake    (NULL)  0
```

Seems complex, but is really easy-ish when you read the documentation.

# Gemini

Gemini is a new attempt at creating a privacy-first bloat-free web. Started in 2019, it attempts to be a text-based alternative to the traditional web. It is a spiritual successor of the Gopher protocol.

What differentiates the modern web and Gemini is the fact that Gemini has no client-side scripts. It can serve images and videos, but those will be downloaded to your machine and not played using the Gemini protocol \[I.e not inside of a standard Gemini browser].

Documents are written in plain-text or a special markdown/HTML like syntax called GemText markup. The protocol mandates the use of TLS and thus is secure by default.

Request/Response

```
Client: [opens connection]
Client: "gemini://example.net/search" CRLF
Server: "10 Please input a search term" CRLF
Server: [closes connection]
Client: [prompts user, gets input]
Client: [opens connection]
Client: "gemini://example.net/search?gemini%20search%20engines" CRLF
Server: "20 " mimetype CRLF content...
Server: [closes connection]
```

# Sources

Z39.50

* [https://dlib.org/dlib/april97/04lynch.html](https://dlib.org/dlib/april97/04lynch.html).
* [https://en.wikipedia.org/wiki/Z39.50](https://en.wikipedia.org/wiki/Z39.50)
* [https://www.youtube.com/watch?v=KvmjB8gg7KY](https://www.youtube.com/watch?v=KvmjB8gg7KY)
* [https://groups.niso.org/higherlogic/ws/public/download/14978/z39-50-2003\_s2014.pdf](https://groups.niso.org/higherlogic/ws/public/download/14978/z39-50-2003_s2014.pdf)
* [https://www.datamercs.net/posts/2020-08-15-z3950-for-dummies/](https://www.datamercs.net/posts/2020-08-15-z3950-for-dummies/)

Further Reading

* [https://www.rfc-editor.org/rfc/rfc1729.txt](https://www.rfc-editor.org/rfc/rfc1729.txt)

NNTP

* [https://datatracker.ietf.org/doc/html/rfc977](https://datatracker.ietf.org/doc/html/rfc977)
* Updates on 977: [https://datatracker.ietf.org/doc/html/rfc3977](https://datatracker.ietf.org/doc/html/rfc3977)
* [https://en.wikipedia.org/wiki/Network\_News\_Transfer\_Protocol](https://en.wikipedia.org/wiki/Network_News_Transfer_Protocol)
* [https://techterms.com/definition/nntp](https://techterms.com/definition/nntp)

Gopher

* [https://en.wikipedia.org/wiki/Gopher\_(protocol)](https://en.wikipedia.org/wiki/Gopher_\(protocol\))
* [https://www.youtube.com/watch?v=QGCSYyH2r6k](https://www.youtube.com/watch?v=QGCSYyH2r6k)

Gemini

* [https://geminiprotocol.net/](https://geminiprotocol.net/)
* [https://en.wikipedia.org/wiki/Gemini\_(protocol)](https://en.wikipedia.org/wiki/Gemini_\(protocol\))
* [https://www.youtube.com/watch?v=DoEI6VzybDk](https://www.youtube.com/watch?v=DoEI6VzybDk)
* [https://www.youtube.com/watch?v=K-en4nEV5Xc](https://www.youtube.com/watch?v=K-en4nEV5Xc)
* [https://geminiprotocol.net/docs/protocol-specification.gmi](https://geminiprotocol.net/docs/protocol-specification.gmi)
