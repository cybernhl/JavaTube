# JavaTube
JavaTube is a YouTube video download utility that is based on python's pytube library

## Features
* Support for downloading the full playlist
* Support for progressive and adaptive streams
* Interaction with channels
* onProgress callback register
* Keyword search support
* Ability to get video details (Title, Description, Length, Thumbnail Url, Views, Author and Keywords)
* Subtitle generator for .srt format

## Using JavaTube

To download videos from YouTube you need to import the YouTube class and pass a url argument like this to get access to the streams

```java
public static void main(String[] args) throws Exception {
    Youtube yt = new Youtube("https://www.youtube.com/watch?v=2lAe1cqCOXo");
    yt.streams().getHighestResolution().download("./");
}
```

### Download using filters 

You must pass a HashMap String with the filter you want to use and its respective value

```java
public static void main(String[] args) throws Exception {
    Youtube yt = new Youtube("https://www.youtube.com/watch?v=2lAe1cqCOXo");

    HashMap<String, String> filters = new HashMap<>();
    filters.put("progressive", "true");
    filters.put("subType", "mp4");

    yt.streams().filter(filters).getFirst().download("./");
    
}
```

### Download with callback function

If no parameter is passed, a download percentage string will be printed to the terminal
```java
public static void progress(Long value){
    System.out.println(value);
}

public static void main(String[] args) throws Exception {
    Youtube yt = new Youtube("https://www.youtube.com/watch?v=2lAe1cqCOXo");
    yt.streams().getHighestResolution().download("./", Download::progress);
}
```

### Downloading a playlist

The `getVideos()` method will return an ArrayList with the links extracted from the playlist url (YouTube mix not supported)

```java
 public static void main(String[] args) throws Exception {
    for(String pl : new Playlist("https://www.youtube.com/playlist?list=PLS1QulWo1RIbfTjQvTdj8Y6yyq4R7g-Al").getVideos()){
        new Youtube(pl).streams().getHighestResolution().download("./");
    }
}
```

### Using the search feature

The `results()` method will return an ArrayList with Youtube objects that can be inspected and downloaded.

```java
public static void main(String[] args) throws Exception {
    for(Youtube yt : new Search("YouTube Rewind").results()){
        System.out.println(yt.getTitle());
    }
}
```

### Interacting with channels

The `getVideos()` method returns an ArrayList containing the channel's videos.

```java
public static void main(String[] args) throws Exception {
    for(String c : new Channel("https://www.youtube.com/channel/UCmRtPmgnQ04CMUpSUqPfhxQ").getVideos()){
        System.out.println(new Youtube(c).getTitle());
    }
}
```

### Using the subtitles feature

See available languages.

```java
public static void main(String[] args) throws Exception {
    for(Captions caption: new Youtube("https://www.youtube.com/watch?v=2lAe1cqCOXo&t=1s").getCaptionTracks()){
        System.out.println(caption.code);
        }
    }
}
```

Write to console in .srt format.

```java
public static void main(String[] args) throws Exception {
        System.out.println(new Youtube("https://www.youtube.com/watch?v=2lAe1cqCOXo&t=1s").getCaptions().getByCode("en").xmlCaptionToSrt());
    }
}
```

Download it in .srt format (if the .srt format is not informed, the xml will be downloaded).

```java
public static void main(String[] args) throws Exception {
        new Youtube("https://www.youtube.com/watch?v=2lAe1cqCOXo&t=1s").getCaptions().getByCode("en").download("caption.srt", "./")
    }
}
```

## Download Methods

To download you can use the methods:

* `getHighestResolution()`
* `getLowestResolution() `
* `getOnlyAudio() `

## Filters Parameters:
* `"res"` The video resolution (e.g.: "360p", "720p")
            

* `"fps"` The frames per second (e.g.: "24fps", "60fps")


* `"mineType"` Two-part identifier for file formats and format contents composed of a “type”, a “subtype” (e.g.: "video/mp4", "audio/mp4")


* `"type"` Type part of the mimeType (e.g.: audio, video)


* `"subType"` Sub-type part of the mimeType (e.g.: mp4, webm)


* `"abr"` Average bitrate  (e.g.: "128kbps", "192kbps")


* `"videoCodec"` Video compression format


* `"audioCodec"` Audio compression format


* `"onlyAudio"` Excludes streams with video tracks (e.g.: "true" or "false")


* `"onlyVideo"` Excludes streams with audio tracks (e.g.: "true" or "false")


* `"progressive"` Excludes adaptive streams (one file contains both audio and video tracks) (e.g.: "true" or "false")


* `"adaptive"` Excludes progressive streams (audio and video are on separate tracks) (e.g.: "true" or "false")



