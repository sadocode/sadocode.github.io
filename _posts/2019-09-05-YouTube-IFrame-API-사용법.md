---
layout:	post
title:  “YouTube IFrame API 사용법”
date: 2019-09-05
excerpt:	“How-to-use-YouTube-IFrame-API”
tag:
- YouTube
- API
- JavaScript
comments:	true
---

## 1. 내용

* YouTube Iframe API를 이용하면, 원하는 유튜브 영상을 웹페이지에 띄울 수 있다.
* 따로 키 인증을 받거나 OAuth 2.0을 쓰지 않고, javascript코드로 원하는 영상을 띄울 수 있어서 편리하다.

<br>

## 2. 사용 조건

 * 유저는 HTML5를 지원하는 브라우저를 이용해야한다. -> [지원 불가능한 브라우저 항목](https://stackoverflow.com/questions/1834077/which-browsers-support-script-async-async/1834129#1834129)
 * Internet Explorer 7은 해당 기능을 지원하지 않는다.
 * 동영상 플레이어의 크기는 200 X 200 px 이상이어야 한다.
 * 16:9 플레이어의 크기는 480 X 270 px 이상으로 사용하는 것이 좋다.

<br>

## 3. 코드

~~~ html
<!DOCTYPE html>
<html>
  <body>
    <!— 1. iframe 비디오 플레이어는 “player” <div> 태그에 위치될 것이다. —>
    <div id=“player”></div>

    // 2. 이하 코드는 IFrame Player API 코드가 비동기적으로 로드되게 한다
    <script src=“https://www.youtube.com/iframe_api”></script>

    <script>
      // 3. 이하 함수는 API 코드가 다운로드 된 후에, <iframe>을 생성한다.
      // 높이 360px, 너비 640px로 iframe을 생성하고,
      // videoId M7lc1UVf-VE인 영상이 나타나게 한다.
      var player;
      function onYouTubeIframeAPIReady() {
        player = new YT.Player(‘player’, {
          height: ‘360’,
          width: ‘640’,
          videoId: ‘M7lc1UVf-VE’,
          events: {
            ‘onReady’: onPlayerReady,
            ‘onStateChange’: onPlayerStateChange
          }
        });
      }

      // 4. IFrame API는 비디오 플레이어가 준비되었을 때, 이하 함수를 호출한다.
      function onPlayerReady(event) {
        event.target.playVideo();
      }

      // 5. IFrame API는 비디오 플레이어의 상태가 변경되었을 때, 이하 함수를 호출한다.
      // 이하 함수는 영상이 재생 중인지 여부를 가리킨다.
      // 이하 함수의 값 6000은 이 플레이어가 6초 재생 후 자동으로 멈춰야 함을 의미한다.
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
  </body>
</html>
~~~

<br>

## 4. 코드 설명

<br>

## 5. 결과

<iframe width="640" height="360" src="//www.youtube.com/embed/M7lc1UVf-VE" frameborder="0"> </iframe>

