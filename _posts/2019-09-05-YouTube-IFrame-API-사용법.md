---
layout: post
title:  "YouTube IFrame API 사용법"
date:   2019-09-05
excerpt: "How to use Youtube IFrame API."
tag:
- YouTube
- API
- JavaScript
comments: true
---

    <div id=”player”></div>
    <script src="https://www.youtube.com/iframe_api">

      var player;
      function onYouTubeIframeAPIReady() {
        player = new YT.Player(‘player’, {
          height: ‘360’,
          width: ‘640’,
          videoId: ‘M71c1UVf-VE’,
          events: {
            ‘onReady’: onPlayerReady,
            ‘onStateChange’: onPlayerStateChange
          }
        });
      }

      function onPlayerReady(event) {
        event.target.playVideo();
      }

      var done = false;
      function onPlayerStateChange(event) {
        if (event.data == YT.PlayerState.PLAYING && !done) {
          setTimeout(stopVideo, 6000);
          done = true;
        }
      }
      function stopVideo() {
        player.stopVideo();
      }
    </script>
