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

# <a id="Player"></a>`Player` class #

## `Player Constructor` ##
- [`constructor`](#Player_constructor)

## `Player Instance Methods` ##

### Initialization Methods

- [`load`](#Player_load)
- [`loadUrl`](#Player_loadUrl)
- [`loadAudioProxy`](#Player_loadAudioProxy)
- [`loadAudioTrack`](#Player_loadAudioTrack)
- [`close`](#Player_close)
- [`addAudioTrack`](#Player_addAudioTrack)
- [`addAudioTrackWithId`](#Player_addAudioTrackWithId)
- [`audioAddHandler`](#Player_audioAddHandler)
- [`setAudioTrackUrl`](#Player_setAudioTrackUrl)
- [`getAudioTrack`](#Player_getAudioTrack)
- [`addCaptions`](#Player_addCaptions)
- [`addCaptionsHandler`](#Player_addCaptionsHandler)
- [`captionsError`](#Player_captionsError)
- [`disableHotKeys`](#Player_disableHotKeys)
- [`enableHotKeys`](#Player_enableHotKeys)
- [`setStartTapeTimecode`](#Player_setStartTapeTimecode)

### Playback Control Methods

- [`play`](#Player_play)
- [`playAtRate`](#Player_playAtRate)
- [`playFaster`](#Player_faster)
- [`playFasterBackwards`](#Player_fasterBackwards)
- [`pause`](#Player_pause)
- [`togglePlay`](#Player_togglePlay)
- [`setVolume`](#Player_setVolume)
- [`getVolume`](#Player_getVolume)
- [`seek`](#Player_seek)
- [`seekToFrame`](#Player_seekToFrame)
- [`seekToSec`](#Player_seekToSec)
- [`nextFrame`](#Player_nextFrame)
- [`previousFrame`](#Player_previousFrame)
- [`nextSec`](#Player_nextSec)
- [`previousSec`](#Player_previousSec)
- [`getCurrentAudioTrack`](#Player_getCurrentAudioTrack)
- [`setCurrentAudioTrack`](#Player_setCurrentAudioTrack)
- [`setRange`](#Player_setRange)
- [`cancelRange`](#Player_cancelRange)
- [`hasRange`](#Player_hasRange)
- [`setLoop`](#Player_setLoop)
- [`enterFullscreen`](#Player_enterFullscreen)
- [`exitFullscreen`](#Player_exitFullscreen)
- [`muteAudioTrack`](#Player_muteAudioTrack)
- [`setVideoQuality`](#Player_setVideoQuality)

### Player Status Methods

- [`isPlaying`](#Player_isPlaying)
- [`getCurrentTime`](#Player_getCurrentTime)
- [`getCurrentTimeValue`](#Player_getCurrentTimeValue)
- [`getCurrentTapeTimecode`](#Player_getCurrentTapeTimecode)
- [`getCurrentStandardTimecode`](#Player_getCurrentStandardTimecode)
- [`getCurrentFrame`](#Player_getCurrentFrame)
- [`getTimeSample`](#Player_getTimeSample)
- [`getDurationSec`](#Player_getDurationSec)
- [`getSeekableDurationSec`](#Player_getSeekableDurationSec)
- [`getTimeline`](#Player_getTimeline)
- [`isAudioMutable`](#Player_isAudioMutable)


## _Player Constructor_ ##
### <a id="Player_constructor"></a>`Player()`
Constructs new Player object.


#### Arguments
1. `container` *(HTML Element)*: The container element where the player is to be created.
2. `options` *(Map)*: options map.

Options:
- hotkeys: true/false - enable standard Player hotkeys.
- serverUrl: URL - back-end URL

#### Example

The follow example shows the basic usage of a Player.

```js
    var container = document.getElementById("playerContainer");
    var options = {hotkeys: true,
                   serverUrl: "http://some.server.com:4242/"}
    var player = new Player(container, options);
```

* * *

## _Player Instance Methods_ ##

### <a id="Player_load"></a>`Player.prototype.load(proxyIdOrURL, onDone)`
Load proxy or file or stream into the player.
#### Arguments
1. `proxyIdOrURL` *(String)*: ID of the proxy or URL of the file/stream to open.
2. `onDone` *(Function)*: callback function to execute when the proxy is loaded, takes one argument `error`.

#### Example

```js
player.load("PROXY_ID", function(err) {
    if (!err) {
        console.log("everything loaded just fine");
    } else {
        console.error(err);
    }
});
```

---

### <a id="Player_loadUrl"></a>`Player.prototype.loadUrl(url, onDone)`
Load file or stream into the player.
#### Arguments
1. `url` *(String)*: url of the file/stream to open.
2. `onDone` *(Function)*: callback function to execute when the video is loaded, takes one argument `error`.

#### Example

```js
player.loadUrl("http://some.host.com/path/something.mpd", function(err) {
    if (!err) {
        console.log("everything loaded just fine");
    } else {
        console.error(err);
    }
});
```

---

### <a id="Player_loadAudioProxy"></a>`Player.prototype.loadAudioProxy(serverUrl, proxyId, [fileName])`
Load audio proxy to the current video proxy. Use in `player.load` callback.
#### Arguments
1. `serverUrl` *(String)*: the url of audioproxy server
2. `proxyId` *(String)*: ID of the audioproxy
3. `fileName` *(String)*: optional display name to show

#### Example
```js
player.load("PROXY_ID", function (err) {
    if (!err) {
        player.loadAudioProxy("SERVER_URL", "AUDIOPROXY_ID", 'FILENAME');
    }
});
```

---

### <a id="Player_loadAudioTrack"></a>`Player.prototype.loadAudioTrack(url, [fileName])`
Add audio file as a new audio track. Use in `player.load` callback.
#### Arguments
1. `url` *(String)*: the url of audio file
2. `fileName` *(String)*: optional display name to show

#### Example
```js
player.load("PROXY_ID", function (err) {
    if (!err) {
        player.loadAudioTrack("http://some.host.com/sometrack.m4a", 'FILENAME');
    }
});
```

---

### <a id="Player_close"></a>`Player.prototype.close()`
Close the video and clear player view.

---

### <a id="Player_addAudioTrack"></a>`Player.prototype.addAudioTrack(displayName, channelNames)`
Add audio track stub. Real audio stream can be associated with this stub later using [`setAudioTrackUrl`](#Player_setAudioTrackUrl).
Random id will be generated for this track.
Use in `player.load` callback.
#### Arguments
1. `displayName` *(String)*: display name of the track
2. `channelNames` *(Array)*: display names of audio channels in this track
String displayName, Array<String> channelNames
#### Returns
*(String)*: track id
#### Example
```js
player.load("PROXY_ID", function (err) {
    if (!err) {
        player.loadAudioTrack("FILENAME", ["Left", "Right"]);
    }
});
```

---

### <a id="Player_addAudioTrackWithId"></a>`Player.prototype.addAudioTrackWithId(id, displayName, channelNames)`
Add audio track stub with id=`id`. Real audio stream can be associated with this stub later using [`setAudioTrackUrl`](#Player_setAudioTrackUrl).
Use in `player.load` callback.
#### Arguments
1. `id` *(String)*: id of the track
2. `displayName` *(String)*: display name of the track
3. `channelNames` *(Array)*: display names of audio channels in this track
String displayName, Array<String> channelNames
#### Returns
*(String)*: track id
#### Example
```js
player.load("PROXY_ID", function (err) {
    if (!err) {
        player.loadAudioTrackWithId("0128749164813674", "FILENAME", ["Left", "Right"]);
    }
});
```

---

### <a id="Player_audioAddHandler"></a>`Player.prototype.audioAddHandler(callback)`
Show "add audio track" button in the player UI. `callback` function is called on click.
Use in `player.load` callback with `player.loadAudioProxy` or `player.loadAudioTrack` to add new audio track.
#### Arguments
1. `callback` *(Function)*: event handler

#### Example
```js
player.load("PROXY_ID", function (err) {
    if (!err) {
        player.audioAddHandler(function (e) {
            /*any additional code*/
            player.loadAudioProxy("SERVER_URL", "AUDIOPROXY_ID", "FILENAME");
        });
    }
});
```

---

### <a id="Player_setAudioTrackUrl"></a>`Player.prototype.setAudioTrackUrl(trackId, url)`
Set audio stream url in audio track stub. Used with audio track stubs previously created with `player.addAudioTrack`.
#### Arguments
1. `trackId` *(String)*: track id
2. `url` *(String)*: audio stream url

#### Example
```js
player.load("PROXY_ID", function (err) {
    if (!err) {
        /* create stub on UI indicating that track is being loaded */
        var trackId = player.addAudioTrack("mytrack", ["Left", "Right"]);
        /* wait for adapter to transcode the audio and give us trackUrl */
        player.setAudioTrackUrl(trackId, trackUrl)
    }
});
```

---

### <a id="Player_getAudioTrack"></a>`Player.prototype.getAudioTrack(trackId)`
Get audio track.
#### Arguments
1. `trackId` *(String)*: track id
#### Returns
*(PlayerAudioTrack)*: audio track. See [`PlayerAudioTrack`](#PlayerAudioTrack)
#### Example
```js
var track = player.getAudioTrack("my_track_id_123");
```

---

### <a id="Player_addCaptions"></a>`Player.prototype.addCaptions(captions)`
Add captions/subtitles track.
#### Arguments
1. `captions` *(Subtitles)*: parsed captions
#### Example
```js
player.addCaptions(mySubtitles);
```

---

### <a id="Player_addCaptionsHandler"></a>`Player.prototype.addCaptionsHandler(callback)`
Show "add captions track" button in the player UI. `callback` function is called on click.
Use in `player.load` callback with `player.addCaptions`.
#### Arguments
1. `callback` *(Function)*: event handler

#### Example
```js
player.load("PROXY_ID", function (err) {
    if (!err) {
        player.addCaptionsHandler(function (e) {
            /*load and parse some captions here*/
            player.addCaptions(myParsedCaptions);
        });
    }
});
```

---

### <a id="Player_captionsError"></a>`Player.prototype.captionsError(message)`
Show error message. Use it to tell the user that captions could not be loaded.
#### Arguments
1. `message` *(String)*: error message
#### Example
```js
player.captionsError("Failed to load captions from http://host.com/path/caps01.xml");
```

---

### <a id="Player_disableHotKeys"></a>`Player.prototype.disableHotKeys()`
Disable hot keys handling.

---

### <a id="Player_enableHotKeys"></a>`Player.prototype.enableHotKeys()`
Enable hot keys handling.

---

### <a id="Player_setStartTapeTimecode"></a>`Player.prototype.setStartTapeTimecode(timecode)`
Set start tape timecode. New timecode overrides start timecode obtained from video stream.
Use in `player.load` callback.
#### Arguments
1. `timecode` *(String)*: start tape timecode

#### Example
```js
player.load("PROXY_ID", function (err) {
    if (!err) {
        player.setStartTapeTimecode("01:00:00:00");
    }
});
```

---

### <a id="Player_play"></a>`Player.prototype.play()`
Start playback at 1x speed.

---

### <a id="Player_playAtRate"></a>`Player.prototype.playAtRate(rate)`
Fast forward/rewind.
#### Arguments
1. `rate` *(Integer)*: playback rate, possible values: -8, -4, -2, -1, 1, 2, 3, 4, 8.
#### Example
```js
player.playAtRate(2); //play forward at 2x speed
player.playAtRate(-2); //play backwards at 2x speed

```

---

### <a id="Player_playFaster"></a>`Player.prototype.playFaster()`
Double the playback rate. If current rate is 0 or negative then play forward at 1x speed.
#### Example

```js
player.play(); // play at 1x
player.playFaster(); //play forward at 2x speed
player.playFaster(); //play forward at 4x speed

```

---

### <a id="Player_playFasterBackwards"></a>`Player.prototype.playFasterBackwards()`
Double the backwards playback rate. If current rate is 0 or positive then play backwards at 1x speed.
#### Example

```js
player.playAtRate(-1); // play backwards at 1x
player.playFasterBackwards(); //play backwards at 2x speed
player.playFasterBackwards(); //play backwards at 4x speed

```

---

### <a id="Player_pause"></a>`Player.prototype.pause()`
Pause the player.

---

### <a id="Player_togglePlay"></a>`Player.prototype.togglePlay()`
Either start or pause playback depending on the current play state.

---

### <a id="Player_setVolume"></a>`Player.prototype.setVolume(val)`
Set the volume of the player.
#### Arguments
1. `val` *(Number)*: Number in range from 0 to 1 - new player volume.

---

### <a id="Player_getVolume"></a>`Player.prototype.getVolume()`
Get current player volume.
#### Returns
*(Number)*: Number in range from 0 to 1 - current volume.

---

### <a id="Player_seek"></a>`Player.prototype.seek(time)`
Jump to a specific frame or time.
Time can be tape or standard timecode or frame number.
#### Arguments
1. `time` *(tape / standard timecode / frame number)*:

#### Example

```js
player.seek("00:59:42;12"); //tape timecode
player.seek("00:59:42.321"); //standard timecode
player.seek(123456); //frame number
```

---

### <a id="Player_seekToFrame"></a>`Player.prototype.seekToFrame(frameNumber)`
Jump to a specific frame.
#### Arguments
1. `frameNumber` *(Integer)*: frame number

#### Example

```js
player.seek(123456);
```

---

### <a id="Player_seekToSec"></a>`Player.prototype.seekToSec(time)`
Jump to a specific time.
#### Arguments
1. `time` *(Number)*: time in seconds

#### Example

```js
player.seek(300.043);
```

---

### <a id="Player_nextFrame"></a>`Player.prototype.nextFrame(numberOfFrames)`
Pause the player and jump forward by `numberOfFrames` frames.
#### Arguments
1. `numberOfFrames` *(Integer)*: number of frames

#### Example

```js
player.nextFrame(10);
```

---

### <a id="Player_previousFrame"></a>`Player.prototype.previousFrame(numberOfFrames)`
Pause the player and jump backwards by `numberOfFrames` frames.
#### Arguments
1. `numberOfFrames` *(Integer)*: number of frames

#### Example

```js
player.previousFrame(10);
```

---

### <a id="Player_nextSec"></a>`Player.prototype.nextSec(seconds)`
Pause the player and jump forward by `seconds` seconds.
#### Arguments
1. `seconds` *(Number)*: time step in seconds

#### Example

```js
player.nextSec(12.45);
```

---

### <a id="Player_previousSec"></a>`Player.prototype.previousSec(seconds)`
Pause the player and jump backwards by `seconds` seconds.
#### Arguments
1. `seconds` *(Number)*: time step in seconds

#### Example

```js
player.previousSec(4.5);
```

---

### <a id="Player_getCurrentAudioTrack"></a>`Player.prototype.getCurrentAudioTrack()`
Get current audio track.
#### Returns
*(PlayerAudioTrack)*: current audio track. See [`PlayerAudioTrack`](#PlayerAudioTrack)
#### Example
```js
var track = player.getCurrentAudioTrack();
```

---

### <a id="Player_setCurrentAudioTrack"></a>`Player.prototype.setCurrentAudioTrack(trackId)`
Set current audio track.
#### Arguments
1. `trackId` *(String)*: track id
2. `url` *(String)*: audio stream url

#### Example
```js
player.setCurrentAudioTrack("eng_5.1");
```

---

### <a id="Player_setRange"></a>`Player.prototype.setRange(fromTime, toTime, loop)`
Set playback range and optionally enter loop mode. Times can be in any format: tape timecode, standard timecode, frame number.

#### Arguments
1. `fromTime` *(tape timecode / standard timecode / frame number)*: start of the range
2. `toTime` *(tape timecode / standard timecode / frame number)*: end of the range
3. `loop` *(Boolean)*: optional, defaults to false. If set to true switches to loop mode

#### Example

```js
player.setRange("01:00:00:00", "01:13:37:23", true);
player.setRange(10000, 10256, true);
```

---

### <a id="Player_cancelRange"></a>`Player.prototype.cancelRange()`
Cancel range. Resets range start/end to the beginning and end of entire video respectively.

---

### <a id="Player_hasRange"></a>`Player.prototype.hasRange()`
Tell if player is in range mode.
#### Returns
*(Boolean)* : `true` - player is in range mode, `false` player plays the whole stream;

---

### <a id="Player_setLoop"></a>`Player.prototype.setLoop(loop)`
Switch between loop/continuous playback modes. In loop mode playback loops within the range previously set by setRange() method.

#### Arguments
1. `loop` *(Boolean)*: true for loop, false for continuos playback

---

### <a id="Player_enterFullscreen"></a>`Player.prototype.enterFullscreen()`
Enter fullscreen mode.

---

### <a id="Player_exitFullscreen"></a>`Player.prototype.exitFullscreen()`
Exit fullscreen mode.

---

### <a id="Player_muteAudioTrack"></a>`Player.prototype.muteAudioTrack(trackId)`
Mute all channels in the audio track.
#### Arguments
1. `trackId` *(String)*: track id

---

### <a id="Player_setVideoQuality"></a>`Player.prototype.setVideoQuality(quality)`
Switch between hi and low resolution video streams. Use constants `Player.FULL_RES` and `Player.LOW_RES`.
#### Arguments
1. `quality` *(String)*: quality type
#### Example
```js
player.setVideoQuality(Player.LOW_RES);
```

---

### <a id="Player_isPlaying"></a>`Player.prototype.isPlaying()`
Is player playing or paused (true/false).

#### Returns
*(Boolean)* : `true` - for playing, `false` otherwise;

---

### <a id="Player_getCurrentTime"></a>`Player.prototype.getCurrentTime()`
Get current player position as time in seconds.

#### Returns
*(Number)*: time in seconds

#### Example

```js
player.getCurrentTime()
// => 230.0234
```

---

### <a id="Player_getCurrentTimeValue"></a>`Player.prototype.getCurrentTimeValue()`
Get current player position as time value in video stream's time units.

#### Returns
*(Integer)*: time value

#### Example

```js
player.getCurrentTimeValue()
// => 42042
```

---

### <a id="Player_getCurrentTapeTimecode"></a>`Player.prototype.getCurrentTapeTimecode()`
Get current player position as tape timecode.

#### Returns
*(String)*: string representation of the current position tape timecode.

#### Example

```js
player.getCurrentTapeTimecode();
// => "00:00:26:06"
```

---

### <a id="Player_getCurrentStandardTimecode"></a>`Player.prototype.getCurrentStandardTimecode()`
Get current player position as standard timecode.

#### Returns
*(String)*: string representation of the current position standard timecode.

#### Example

```js

player.getCurrentStandardTimecode();
// => "00:00:26.280"

```

---

### <a id="Player_getCurrentFrame"></a>`Player.prototype.getCurrentFrame()`
Get current player position as zero based frame number.

#### Returns
*(Integer)*: current frame number

#### Example

```js
player.getCurrentFrame()
// => 630
```

---


### <a id="Player_getTimeSample"></a>`Player.prototype.getTimeSample()`
Get current player position as `TimeSample` object containing time value, seconds and frame number.

#### Returns
*(TimeSample)*: TimeSample instance representing current position

#### Example

```js
player.getTimeSample()
// => {tv: 7168, frame: 14, sec: 0.56442}
```

---

### <a id="Player_getDurationSec"></a>`Player.prototype.getDurationSec()`
Get timeline duration in seconds. It is equal to video stream duration.

#### Returns
*(Number)*: duration in seconds

#### Example

```js
player.getDurationSec()
// => 230.0234
```

---

### <a id="Player_getSeekableDurationSec"></a>`Player.prototype.getSeekableDurationSec()`
Get timeline seekable duration in seconds.
It is equal to the lesser of video stream duration and current audio track duration.
Player can seek and play media from the beginning of the stream up to this duration.

#### Returns
*(Number)*: seekable duration in seconds

#### Example

```js
player.getDurationSec()
// => 230.0234
player.getSeekableDurationSec()
// => 30
```

---

### <a id="Player_getTimeline"></a>`Player.prototype.getTimeline()`
Get [`Timeline`](#Timeline) instance.

#### Returns
*(Timeline)*: `Timeline` instance

---

### <a id="Player_isAudioMutable"></a>`Player.prototype.isAudioMutable()`
Returns true if individual audio channels can be muted.

#### Returns
*(Boolean)* : `true` - individual audio channels can be muted, `false` otherwise;

* * *

# <a id="PlayerAudioTrack"></a>`PlayerAudioTrack` class #

## `PlayerAudioTrack Instance Methods` ##

- [`id`](#PlayerAudioTrack_id)
- [`getChannels`](#PlayerAudioTrack_getChannels)
- [`isMuted`](#PlayerAudioTrack_isMuted)

## _PlayerAudioTrack Instance Methods_ ##

### <a id="PlayerAudioTrack_id"></a>`PlayerAudioTrack.prototype.id()`
Get track id.

#### Returns
*(String)*: track id

---

### <a id="PlayerAudioTrack_getChannels"></a>`PlayerAudioTrack.prototype.getChannels()`
Get audio track channels.

#### Returns
*(Array)*: array of channel info objects

#### Example

```js
player.getCurrentAudioTrack().getChannels();
// => [{label: "FrontLeft", muted: false}, {label: "FrontRight", muted: false}]
```

---

### <a id="PlayerAudioTrack_isMuted"></a>`PlayerAudioTrack.prototype.isMuted()`
Returns true if every channel in the track is muted.

#### Returns
*(Boolean)*: `true` - every channel is muted, `false` - otherwise

* * *

# <a id="Timeline"></a>`Timeline` class #

## `Timeline Instance Methods` ##

- [`getFrameByTimecode`](Timeline_getFrameByTimecode)

## _Timeline Instance Methods_ ##

* * *

# <a id="TapeTimecode"></a>`TapeTimecode` class #

## `TapeTimecode Class Methods` ##

- [`isTimecode`](#TapeTimecode_isTimecode)
- [`parseTimecode`](#TapeTimecode_parseTimecode)

## `TapeTimecode Instance Methods` ##

- [`getTimecodeAtFrame`](#TapeTimecode_getTimecodeAtFrame)
- [`getFrameByTimecode`](#TapeTimecode_getFrameByTimecode)
- [`getTapeFps`](#TapeTimecode_getTapeFps)
- [`getTimecodedDuration`](#TapeTimecode_getTimecodedDuration)
- [`isDropFrame`](#TapeTimecode_isDropFrame)

## _TapeTimecode Class Methods_ ##

### <a id="TapeTimecode_isTimecode"></a>`TapeTimecode.isTimecode(str)`
Check whether `str` is a valid tape timecode string representation.
#### Returns
*(Boolean)*: `true` - `str` is valid, `false` - otherwise
#### Example
```js
TapeTimecode.isValid("01:00:00:12");
// => true
TapeTimecode.isValid("00:00:53;13");
// => true
TapeTimecode.isValid("01:00:00:1234");
// => false
```

---

### <a id="TapeTimecode_parseTimecode"></a>`TapeTimecode.parseTimecode(tc, timecodeRate, dropFrame)`
Parse tape timecode from string `tc`.

#### Arguments
1. `tc` *(String)*: string to parse
2. `timecodeRate` *(Integer)*: frames per second
3. `dropFrame` *(Boolean)*: is timecode drop frame or non-drop frame

#### Returns
*(Integer)*: frame number corresponding to the parsed timecode

#### Example
```js
TapeTimecode.parseTimecode("00:00:01:12", 24, false);
// => 36
```

## _TapeTimecode Instance Methods_ ##

### <a id="TapeTimecode_getTimecodeAtFrame"></a>`TapeTimecode.prototype.getTimecodeAtFrame(frame)`
Get tape timecode at frame `frame`.

#### Arguments
1. `frame` *(Integer)*: frame number

#### Returns
*(String)*: tape timecode

#### Example
```js
ttc.getTimecodeAtFrame(36);
// => "01:00:01:12"
```

---

### <a id="TapeTimecode_getFrameByTimecode"></a>`TapeTimecode.prototype.getFrameByTimecode(tc)`
Get frame number by tape timecode.

#### Arguments
1. `tc` *(String)*: tape timecode

#### Returns
*(Integer)*: frame number

#### Example
```js
ttc.getFrameByTimecode("01:00:01:12");
// => 36
```

---

### <a id="TapeTimecode_getTapeFps"></a>`TapeTimecode.prototype.getTapeFps()`
Get the tape frame rate of this tape timecode instance.

#### Returns
*(Integer)*: tape frame rate

---

### <a id="TapeTimecode_getTimecodedDuration"></a>`TapeTimecode.prototype.getTimecodedDuration()`
Get timeline duration as tape timecode.

#### Returns
*(String)*: tape timecode

#### Example
```js
ttc.getTimecodedDuration();
// => "00:33:28:12"
```

---

### <a id="TapeTimecode_isDropFrame"></a>`TapeTimecode.prototype.isDropFrame()`
Return true if this tape timecode instance is drop frame.

#### Returns
*(Boolean)*: `true` for drop frame, `false` for non-drop frame

---

