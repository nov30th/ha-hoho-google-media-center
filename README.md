# ha-hoho-google-media-centre
Homeassistant Google Home/Nest/TV music centre

## Summary
上周买了两个Google Nest Mini 与 Chromecast，在此之前，从来没发现Google设备如此方便，可以在多个音箱上同时播放音乐，能让家里无死角充满温馨的音乐。于是在我自己的HA系统里寻找是否有现成简单的组件来进行随机播放音乐，以及音乐列表功能。
结果发现多是YouTube与Spotify在线播放，在国内这非常不方便，VPN基本都有总流量，用本地音乐更合适，于是 [HA国外讨论区](https://community.home-assistant.io/t/m3u-playlists-in-media-browser/243231/31)找了和提问都没有发现现成的，那么，随手做一个吧。

Homemade music centre via Chromecast with node-red in Homeassistant.

**Simply And Easy.**

## Features

- Shuffling music / Stop playing to Google Home / Nest by toggle the music_time button.
- Shuffling music / Stop playing by telling Google Home / Nest to turn on/off "Music Time" (google_assistant linked with HA required).
- Stop the music playing (e.g "Hey Google, Stop") to media_player will stop shuffling music automatically.
- Pause the music playing (e.g. "Hey Google, pause music") to media_player will shuffling the next song automatically.
- Play the next song by simply calling service media_player.media_stop or media_player.media_pause
- Music can be played with different playlists.
- Favorite songs can be added or delete by press the Favorite button.
- Added favourite button that can make it likes a rich functional media player.
- Supports ID3 of mp3 file to media player displaying.
- Media player has a background if the media file contains an image.

## Before Setup

1. You need to have node-red installed in your HA.
2. Set your media folder as following
```
homeassistant:
  media_dirs:
    media: /config/media
```

Your local folder should looks like this:
```
| config/
| -- media/
| -- | mp3/
| -- | -- | a.mp3
| -- | -- | d.ogg
| -- | -- | sub-folder/
| -- | -- | -- |oh-yes.mp3
| -- | -- | -- |oh-no.mp3
| -- | favorite_mp3/
| -- | -- | b.mp3
| -- | -- | c.mp3
```
> I mounted the folder (/config/media) [rw] from my SMB server, you could simply create this folder if disk space is not a problem.

## Setup

1. Create the below input entity in your HA -> Configuration -> Helpers.

    Name|Entity ID|Type|Options
    ---|---|---|---
    Favorite Song|input_boolean.favorite_song|Toggle
    Music Playlist|input_select.music_playlist|Dropdown|Mp3;My Favorite(2 items)
    Music Time|input_boolean.music_time|Toggle

2. Install node-red palettes

    Name|Version
    ---|---
    node-red-contrib-media-tags|0.0.6
    node-red-contrib-fs-ops|1.6.0


3. Parse the node-red flow to your HA node-red.
[Node-Red Code](node-red.txt)

4. Add buttons to your Lovelace page.

    ![lovelace](images/buttons.png)

    > Next Song Button:
    ```
    type: 'custom:button-card'
    tap_action:
    action: call-service
    service: media_player.media_stop
    service_data:
        entity_id: media_player.home_group
    size: 50px
    icon: 'hass:skip-forward'
    name: Next Song
    ```

    > Volume Control
    ```
    type: entities
    entities:
    - type: 'custom:slider-entity-row'
        entity: media_player.home_group
    ```

## Configuration

1. Edit the "PLAYLIST mapping" in node-red.

    ![Edit](images/playlist_mapping.png)

    Var|Meaning
    ---|---
    playlist_mapping|Mapping the input_select.music_playlist items to your local folder
    msg.album_cover_temp_file|Music album cover image tempuary file
    msg.album_cover_temp_url|The URL of album_cover_temp_file, DOMAIN could be ip address.
    file_exts_filter|file types

## Enjoy the powerful node-red and this content.


![Mine](images/screen.png)


## Give a star to this repo if you like it.