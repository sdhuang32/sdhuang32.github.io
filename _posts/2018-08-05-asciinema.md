---
title:  "asciinema: Amazing Terminal Session Recorder"
classes: wide
tags: 
  - Scripting
  - Tools
date: 2018-08-05
---

In this article I'm going to share my experience using [asciinema](https://asciinema.org), a very elegant and useful tool for you to share all your works in a Linux/Unix shell.

The asciinema project consists of some pieces:
- [asciinema](https://github.com/asciinema/asciinema): command-line tool for producing the recording artifact, which is named "asciicast"
- [asciinema-player](https://github.com/asciinema/asciinema-player): javascript web player for the asciicast
- [asciinema-server](https://github.com/asciinema/asciinema-server): web app for hosting asciicasts
- [asciicast2gif](https://github.com/asciinema/asciicast2gif): tool for transforming from asciicast to a gif file

Benefits using asciinema includes:
- It helps us memorize all the inputs and outputs shown on the screen, including the timing and color information in an extremely simple way. You can just use it for your working diary or learning history.
- Instead of manually keying in every single character you see in a youtube video, you can just highlight any command on the asciinema player and copy and paste to reproduce a series of works very easily.
- You can embed the asciinema player at many places such as your blog, Google Site, or any pages that support `<script>` tag.

Next, I'll briefly show what it can do for us. The first part is a self-describing example that shows the most useful combination `asciinema rec` + `asciinema play` + `asciinema upload`:
<script id="asciicast-YF4J9BXk1Ym8l5r5oz8ZQcsYg" src="https://asciinema.org/a/YF4J9BXk1Ym8l5r5oz8ZQcsYg.js" async data-size="big"></script>

The latter part is a terminal-to-terminal streaming example which uses the raw recording mode available since v2.0.0:
<script id="asciicast-ZkdPJOQBJzlRGf5R6wEHiJT9S" src="https://asciinema.org/a/ZkdPJOQBJzlRGf5R6wEHiJT9S.js" async></script>
In this example I use the localhost as both the recording host and viewing host, but of course you can replace the viewing host with your friend's host IP or your customer's host IP to do such a live streaming!
