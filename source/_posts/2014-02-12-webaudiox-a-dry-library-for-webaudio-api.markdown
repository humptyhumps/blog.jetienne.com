---
layout: post
title: "WebAudiox.js - a DRY library for Web Audio API"
date: 2013-04-23 07:43
comments: true
published: false
categories: [webaudiox]
---

[webaudiox.js](https://github.com/jeromeetienne/webaudiox)
is a bunch of helpers for 
[WebAudio API](https://dvcs.w3.org/hg/audio/raw-file/tip/webaudio/specification.html).
I wrote them a while back. 
Now i would like to make a series of posts about it. This post is the first.
I plan to publish one post per week explaining WebAudiox the marvels you can do with WebAudio API. 
I hope we will enjoy ourself in the process :)

<iframe width="420" height="315" src="http://www.youtube.com/embed/3cSAu2mZHqU" frameborder="0" allowfullscreen></iframe>

<!-- more -->

## What is a DRY library ?

[webaudiox.js](https://github.com/jeromeetienne/webaudiox)
is a DRY library, as in **Dont Repeat Yourself** library.
It isn't a library per se.
I took the tasks i kept repeating when i did Web Audio API and put each of them in its own little helper file.
WebAudiox has more than 10 of them. 
Each of them got a single pupose and try to do it well.
Most of them are very small, 50 lines on average, so you can easily look at the code. Additionaly, they have no dependancies so you don't have to learn everything to understand a single feature you need.

You can use any of them independantly.
This makes it very light to include these in your own code.
There is a 
[webaudiox build](https://github.com/jeromeetienne/webaudiox/build/)
which bundle them though.
This is provided for convenience.
It is just the concatenation of all the helpers.


## Let's get Started

We will start by a short intro of the library, 
followed by 2 basic helpers, 
[webaudiox.shim.js](https://github.com/jeromeetienne/webaudiox/blob/master/lib/webaudiox.shim.js) to handle vendor prefix for you
, and
[webaudiox.loadbuffer.js](https://github.com/jeromeetienne/webaudiox/blob/master/lib/webaudiox.loadbuffer.js) to load and decode sounds.

Here is a simple example which  load a sound and play it.
It initializes the AudioContext, downloads a sound with ```WebAudiox.loadBuffer()``` and play it with ```.start(0)```

```javascript
// create WebAudio API context
var context	= new AudioContext()

// load a sound and play it immediatly
WebAudiox.loadBuffer(context, 'sound.wav', function(buffer){
		// init AudioBufferSourceNode
		var source	= context.createBufferSource();
		source.buffer	= buffer
		source.connect(context.destination)
		
		// start the sound now
		source.start(0);
});
```

## How To Install It ?

You can download the build with a usual ```<script>```. 
The easiest is to get 
[webaudiox.js](https://raw.github.com/jeromeetienne/webaudiox/master/build/webaudiox.js)
in ```/build``` directory.

```html
 <script src='webaudiox.js'></script>
```

[bower](http://bower.io/) is supported if you want. Just use

```bash
bower install webaudiox
```


# webaudiox.shim.js

This helper does a [shim](http://en.wikipedia.org/wiki/Shim_\(computing\)) which handle 
the vendor prefix, so you don't have to. Typically it contains code like 

```javascript
window.AudioContext	= window.AudioContext || window.webkitAudioContext;
```

### Show Don't Tell

* [webaudiox.shim.js](https://github.com/jeromeetienne/webaudiox/blob/master/lib/webaudiox.shim.js)
the source itself.
* [examples/jsfx.html](http://jeromeetienne.github.io/webaudiox/examples/jsfx.html)
\[[view source](https://github.com/jeromeetienne/webaudiox/blob/master/examples/jsfx.html)\] :
It shows a basic usage of this helper.

# webaudiox.loadbuffer.js

This helper loads sound. 
It is a function which load the sound from an ```url``` and decode it.

## Show Don't Tell

* [webaudiox.loadbuffer.js](https://github.com/jeromeetienne/webaudiox/blob/master/lib/webaudiox.loadbuffer.js)
the source itself.
* [examples/lineout.html](http://jeromeetienne.github.io/webaudiox/examples/lineout.html)
\[[view source](https://github.com/jeromeetienne/webaudiox/blob/master/examples/lineout.html)\] :
It shows a basic usage of this helper.
**TODO this link is broken**

## Usage

```javascript
WebAudiox.loadBuffer(context, url, function(buffer){
	// notified when the url has been downloaded and the sound decoded.
}, function(){
	// notified if an error occurs
});
```

## Scheduling Download

In real-life cases, like game, you want to be sure all your sounds
are ready to play before the user start playing.
So here is way to schedule your sound downloads simply.
There is global onLoad callback ```WebAudiox.loadBuffer.onLoad```
This function is notified everytime .loadBuffer() load something.
you can overload it to fit your need.
here for an 
[usage example](https://github.com/jeromeetienne/webaudiox/blob/master/lib/soundsbank.html).

```javascript
// context is the webaudio API context
// url is where to download the sound
// buffer is the just loaded buffer
WebAudiox.loadBuffer.onLoad = function(context, url, buffer){
	// put your own stuff here	
	// ... 
}
```

Additionally there is ```WebAudiox.loadBuffer.inProgressCount```.
it is counter of all the .loadBuffer in progress. 
it useful to know is all your sounds as been loaded.

# Conclusion

So, now we know how to easily load sounds for web audio api.
We can even schedule the download. We got rid of the interoperativity issues with the shim. Rather cool for a first post.
Next post will be about
[webaudiox.jsfx.js](https://github.com/jeromeetienne/webaudiox/blob/master/lib/webaudiox.jsfx.js),
an helper to use [jsfx.js](www.egonelbre.com/js/jsfx/) with Web Audio API.
[jsfx.js](www.egonelbre.com/js/jsfx/)
is a very fun library which generate retro games sounds.
The type of sounds heard in 80's aracade :)

That's all folk, have fun!
