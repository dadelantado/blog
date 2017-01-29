---
layout: post
title: "HOW TO: Pull a YouTube playlist with Javascript"
Category: Articles
tags: [howto,javascript,youtube,api,playlist]
authors: Daniel
---
The following code will help you to pull a YouTube playlist with Javascript and will update the playlist automatically every time the page loads up

#Javascript code

This is the full JS code I will use on this example, the first part is just to make the playlist to switch the videos on with the links on the playlist:

    function switchVideo(videoSrc) {
      videoSrc = 'https://' + videoSrc;
      document.getElementById('FeaturedVideoID').src = videoSrc;
    }

Here is the full JS file:

    function switchVideo(videoSrc) {
      videoSrc = 'https://' + videoSrc;
      document.getElementById('FeaturedVideoID').src = videoSrc;
    }
    function onGoogleLoad() {
      gapi.client.setApiKey('YOUR_APIKEY');
      gapi.client.load('youtube', 'v3', function() {

          var request = gapi.client.youtube.playlistItems.list({
              part: 'snippet',
              playlistId: 'PLAYLIST_ID_TO_PULL',
              maxResults: 50
              });

              request.execute(function(response) {

                var container = $(".responsive-video-list");
                if(!container[0]) {
                  container = $("<div class='responsive-video-list' />")
                  $(".bodytext .description").append(container);
                }

                container.append("<div class=\"featured-video\"><iframe width=\"100%\" height=\"100%\" src=\"https://www.youtube.com/embed/"+ response.items[0].snippet.resourceId.videoId+"?autoplay=0&amp;rel=0&amp;showinfo=0&amp;modestbranding=1&amp;autohide=1\" frameborder=\"0\" allowfullscreen id=\"FeaturedVideoID\"></iframe></div>")


                var containerlist = $("<ul />");
                container.append(containerlist);

                for (var i = 0; i < response.items.length; i++) {
                  console.log(response.items[i].snippet.title + " published at " + response.items[i].snippet.publishedAt)

                  containerlist.append("<li><a onclick=\"switchVideo('www.youtube.com/embed/"+ response.items[i].snippet.resourceId.videoId+"?autoplay=1&amp;rel=0&amp;showinfo=0&amp;modestbranding=1&amp;autohide=1');\" href=\"javascript:void(0);\"> <img src=\"https://img.youtube.com/vi/"+ response.items[i].snippet.resourceId.videoId+"/0.jpg\">"+response.items[i].snippet.title+"</a></li> ");
                }
         });
     });
    }

#HTML code

The Javascript will look for a <em>div</em> with a class <em>"responsive-video-list"</em> inside a <em>div</em> with a class <em>description</em> that is also inside a <em>div</em> with a class <em>bodytext</em>. If it didn't find the <em>"responsive-video-list"</em> it will create it.

The HTML should look like this:

    <html>
    <head></head>
    <body>
      <div class="bodytext">
        <div class="description">
          <div class="responsive-video-list">
          </div>
        </div>
      </div>
    </body>
    </html>

or at least something like this:

    <html>
    <head></head>
    <body>
      <div class="bodytext">
        <div class="description">

        </div>
      </div>
    </body>
    </html>

For this example we will use the following HTML:

    <html>
    <head>
    <script src="js/script.js"></script>
    <script src="https://code.jquery.com/jquery-2.1.1.min.js"></script>
    <link rel="stylesheet" type="text/css" media="all" href="css/global.css" /></head>
    <script src="https://apis.google.com/js/client.js?onload=onGoogleLoad"></script>
    <div class="bodytext">
      <div class="description">
        <div class="responsive-video-list">
        </div>
      </div>
    </div>
    </body>
    </html>

#CSS code

The following code will make the playlist look great and also responsive:

    .video-wrapper{position:relative;width:100%;height:0;padding-top:56.25%}
    .video-wrapper iframe{position:absolute;width:100%;height:100%;top:0;left:0}
    .responsive-video-list{width:100%;margin:0 auto 1.6em} .responsive-video-list ul{padding:0;margin:0;list-style-type:none}
    .responsive-video-list ul img{display:none;width:60px;height:auto;float:left;margin-right:8px;margin-top:-11px}
    .responsive-video-list ul li{padding:0;margin:0 0 4px;overflow:hidden}
    .responsive-video-list ul a{display:inline-block;background-color:#ebebeb;width:100%;padding:11px;position:relative}
    .responsive-video-list ul a:hover{text-decoration:none !important;background-color:#e0e0e0}
    @media screen and (min-width: 46.875em){
      .responsive-video-list ul img{display:block}
      .responsive-video-list ul a{padding:11px 11px 0 0}
     }
