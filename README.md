# VG Player Public API

This repository contains VG Player public API documentation.

# Sample Page

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>VG Player</title>
    <link rel="stylesheet" href="vgplayer.min.css"/>
</head>
<body>
<div id="playerContainer" style="width: 80%;"></div>
<script src="vgplayer.js"></script>
<script>
    var player;

    window.onload = function() {
      var playerContainer = document.getElementById("playerContainer");
      var options = {hotkeys: true};
      var player = new VG.Player(playerContainer, options);
      VG.VGThemeInit.init();
      
      player.load("http://server/to/video.mp4", function() {
          console.log("player Loaded");
      });
    };
</script>
</body>
</html>

```
