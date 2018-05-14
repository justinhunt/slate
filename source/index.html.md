---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ

toc_footers:
  - <a href='https://poodll.com/contact'>ASK for beta access</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:

search: false
---

# Introduction

Cloud Poodll is a single javascript file that will load Poodll audio or video recorders into html divs on the page. Cloud Poodll has no dependencies. It works within or without an AMD module.

[Poodll](https://poodll.com) is a set of language learning tools for teachers and learners, that was developed for the [Moodle](https://moodle.org) platform. With Cloud Poodll educators and interested people can embed Poodll anywhere they like!

# Loading

## Basic loading

> Javascript library include:
> (between head tags)

```html
<script src="https://cdn.jsdelivr.net/gh/justinhunt/cloudpoodll@latest/amd/build/cloudpoodll.min.js"
type="text/javascript"></script>
```

> Placeholder div:
> (between body tags)

```html
<div class=”cloudpoodll”></div>
```

> Javascript initiation code:
> (before closing body tag)

```html
<script type="text/javascript">
   CloudPoodll.autoCreateRecorders();
   CloudPoodll.initEvents();
</script>
```

By default Cloud Poodll will look for elements of class "cloudpoodll" and swap them out for Poodll recorders. To load Cloud Poodll on a page, you will need to perform three steps.

* Include Cloud Poodll
* Set a placeholder
* Load the recorder

### Include Cloud Poodll

At the top of the page, in between the head tags, you will need to load the Cloud Poodll library.
The easiest way to do that is to load it from CDN:

`https://cdn.jsdelivr.net/gh/justinhunt/cloudpoodll@latest/amd/build/cloudpoodll.min.js`

But its also possible to host the file yourself. Its available at:

[https://github.com/justinhunt/cloudpoodll](https://github.com/justinhunt/cloudpoodll)

### Set a placeholder

Wherever you want the recorder to appear on the page, you set a placeholder element in html. Probably this will be a div element. But it could be something else.
Via the id or class name of the element, Cloud Poodll will detect it and insert a recorder into that element.

`<div class=”cloudpoodll”></div>`

### Load the recorder

Cloud Poodll will not load the recorder(s) until explicitly told to do so. Its pretty easy though, you can just call any of the helper functions:

* autoCreateRecorders()
* createRecorder()
* insertRecorder()

e.g `CloudPoodll.autoCreateRecorders();`

Ultimately you will want to do more than just load the recorder. You will want to do something with the recorded file and respond to recorder events. This would be the place to set all that up too. So at this point you should also call:

`CloudPoodll.initEvents();`

## Using load helpers

### Loading by class

```javascript
   CloudPoodll.autoCreateRecorders('myrecorderclass');
```

You can specify your own class in place of "cloudpoodll" in the call to autoCreateRecorders. Then it will load recorders into any element of that class. If you do not pass in a class name to the function, then it will assume it should be looking for 'cloudpoodll'

### Loading by id

```javascript
  CloudPoodll.createRecorder('myrecorderid');
```

You can pass in the DOM id of an element to the createRecorder function and it will load a recorder into that div only.

### Loading in javascript

```javascript
  var container = document.getElementById('myrecorderid');
  var attributes = {"media": "audio","width": 450,"height": 350};
  CloudPoodll.insertRecorder(container,attributes);
```

You can pass in the DOM element (container) and a map of attributes to the insertRecorder function and it will create the recorder accordingly. The attributes that you can pass in are [described later in this page](#parameters).
<aside class="notice">
Note that the name of the attribute declared in the map to the insertRecorder function, is different to the name of the attribute set on the container element. The container element takes data-[parameter name], but the map takes simply [parameter name].
</aside>

### Loading from an AMD module

```javascript
 define('mycoolmodule',['jquery','https://cdn.jsdelivr.net/gh/justinhunt/cloudpoodll@1.1.0/amd/build/cloudpoodll.min.js'], function($,CloudPoodll){
    return function(opts){
      CloudPoodll.createRecorder('myrecorderid');

      CloudPoodll.theCallback = function(message){
         alert('got message: check console');
         console.log(message);
      };

      CloudPoodll.initEvents();
    };
  });
```

Cloud Poodll can work from within an AMD module. Either from CDN directly or from your own project. It will automatically detect require.js , and if present will try to join in nicely.

# Configuration

The simplest way to configure the Cloud Poodll recorders is via data-xxxx attributes on the container element. The Cloud Poodll loaders will pick those up and pass them as parameters to Cloud Poodll to configure the recorder and its behaviour.  Defaults are in place for each of the attribute/parameters. So you should omit ones that you are not interested in.



## Parameters <a name="parameters"></a>
> If setting parameters on container element

```html
<div class="cloudpoodll" data-id="recorder1" data-parent="https://www.mycoolsite.com"
data-media="audio" data-type="bmr" data-width="450" data-height="350"
data-iframeclass="letsberesponsive" data-updatecontrol="someformfieldid" data-timelimit="5"
data-transcode="yes" data-transcribe="no" data-transcribelanguage="en"
data-expiredays="365"></div>
```

> If setting parameters as a map

```json
{"id": "recorder1", "parent": "https://www.mycoolsite.com", "media": "audio",
"type": "bmr", "width": 450, "height": 350, "iframeclass": "letsberesponsive",
"updatecontrol": "someformfieldid", "timelimit": 5, "transcode": "yes",
"transcribe": "no", "transcribelanguage": "en", "expiredays": 365,
"owner":"poodll","region":"tokyo"}
```


Parameter | Default | Description
--------- | ------- | -----------
data-id | '' | A value passed in by the integrator, that is not used by Poodll. We simply pass it back out again with events. Its role is to allow the integrator’s callback javascript to know which recorder on the page the event occurred on.
data-parent | URL of current page | The URL (as far as the domain) of the parent hosting the recorder iframe. This MUST be correct or stuff won't happen. It should start with https. Nothing will work on http sites.
data-media | 'audio' | The type of media being recorded. Either 'audio' or 'video'
data-type | 'bmr' | The skin name of the recorder. Try ‘bmr’, or ‘onetwothree’
data-width | 450 | The width in pixels of the iframe. Ignored if parameter iframeclass is set.
data-height | 350 | The height in pixels of the iframe. Ignored if parameter iframeclass is set.
data-iframeclass | '' | The class that will be applied to the iframe. You would use this to create a [responsive iframe.](https://blog.theodo.fr/2018/01/responsive-iframes-css-trick)
data-updatecontrol | '' | The DOM id of a form control on the page (probably type ‘hidden’ or ‘text’). When a recording is saved successfully, and when data-inputcontrol is set, Poodll will set the URL of the recorded file as the value on the control. NB The updatecontrol parameter will be ignored if you have registered a callback function to handle [Cloud Poodll events](#events).
data-timelimit | 0 | If set this will set the number of seconds available for recording.
data-transcode | 1 | If set to yes, Cloud Poodll will transcode audio to MP3 and video to MP4 for you. Any non 'yes' value means 'no.'
data-transcribe | 0 | If set to yes, Cloud Poodll will transcribe the audio in the file to text and return it in the Cloud Poodll [transcriptioncomplete event](#transcriptioncomplete).
data-transcribelanguage | 'en' | If Cloud Poodll is transcribing the audio in your file, we need to tell it the language. Possible values are "en" and "es" (ie English or Spanish).
data-expiredays | 365 | Sets the number of days for which Cloud Poodll will keep your file. Possible values are 1, 3, 7, 30, 90, 180, 365, 730, 9999. 9999 means Cloud Poodll will never automatically delete your file.
data-owner | 'poodll' | An identifier tag that can be used to find recordings made by a particular individual/entity. Later, delete and other operations can be made against this.
data-region | 'tokyo' | The Amazon AWS region in which recordings should be stored. Possible values are 'tokyo','useast1','dublin','sydney'
data-token | 'somedefaultforlocalhost' | An authorisation token that you receive from https://cloud.poodll.com. You need this to access the service. The default token authorises localhost domain only.

# Events <a name="events"></a>

Having audio and video recorders on your site is a lot of fun. But it usually won't make sense unless you do something with the recordings, or react in some way to a recording.
So we have events. The events that you get are:

* awaitingprocessing
* filesubmitted
* transcriptioncomplete
* error

Event | Description
--------- | ---------
awaitingprocessing | After uploading, recordings are converted(optional), and then copied to a final destination. Use this event to do somethung while the user waits. If you do not wish to wait, the final URL is known at this stage and unless an error occurs, it can be trusted..
filesubmitted | When your recording is uploaded and converted, the filesubmitted event fires. It carries with it the information that you need about filenames and URLs.
transactioncomplete | After your recording is uploaded and converted, and if you have set data-transcribe to yes, then Cloud Poodll will post your file for transcription. The result of that arrives in this event.
error | If an error occurs, Cloud Poodll will do its best to return a notification of that and a message explaining what went wrong.

## Callback

```javascript
CloudPoodll.theCallback=function(thedata){
    console.log(thedata);
};
```

CloudPoodll allows you to register a single callback function to handle recording events. When any of the [events](#events) described above fire, your callback function will be called and the event data will be passed to it.
To register your callback function simply set it as the value of the CloudPoodll.theCallback property.
e.g

`CloudPoodll.theCallback=function(thedata){
    console.log(thedata);
};`

<aside class="notice">
Note that for your events to fire, you must initiate them by calling: CloudPoodll.initEvents();
</aside>

## Event data

> A sample event handler

```javascript
CloudPoodll.theCallback=function(thedata){
    console.log(thedata);
    switch (thedata.type){
        case 'awaitingprocessing':
            alert('awaitingprocessing:' + thedata.s3root + thedata.s3filename);
            break;
        case 'filesubmitted':
            alert('filesubmitted:' + thedata.shorturl);
            break;
        case 'transcriptioncomplete':
            alert('transcriptioncomplete:' + thedata.transcription);
            break;
        case 'error':
            alert('Error: ' + thedata.message);
            break;
    }
};
```

Each of the [events](#events) described above returns a data payload to your callback function. The data contained in each event's data payload is explained in the table below.

### awaitingconversion

Name | Description
--------- | ---------
type | 'awaitingprocessing'
id | The id of the recorder that the event originated from. This is the id you set when creating the recorder.
finalurl | The url of the recorded file 
s3filename | This is the filename of the file in S3 storage. 
s3root | This is the directory in which the file is stored in S3 storage. Combine with s3filename to make the full URL.
updatecontrol | This is the DOM id of the updatecontrol element that the recorder was initialised with.

### filesubmitted

Name | Description
--------- | ---------
type | 'filesubmitted'
id | The id of the recorder that the event originated from. This is the id you set when creating the recorder.
finalurl | The url of the recorded file 
s3filename | This is the filename of the file in S3 storage. 
s3root | This is the directory in which the file is stored in S3 storage. Combine with s3filename to make the full URL.
updatecontrol | This is the DOM id of the updatecontrol element that the recorder was initialised with.

### transcriptioncomplete

Name | Description
--------- | ---------
type | 'transcriptioncomplete'
id | The id of the recorder that the event originated from. This is the id you set when creating the recorder.
finalurl | The url of the recorded file 
s3filename | This is the filename of the file in S3 storage. 
s3root | This is the directory in which the file is stored in S3 storage. Combine with s3filename to make the full URL.
updatecontrol | This is the DOM id of the updatecontrol element that the recorder was initialised with.
language | This is the language that was transcribed. Either 'en' or 'es' (English or Spanish)
transcription | This is the text that was transcribed.
confidence | This is a numerical confidence score of the transcription accuracy.

### error

Name | Description
--------- | ---------
type | 'error'
id | The id of the recorder that the event originated from. This is the id you set when creating the recorder.
code | A code indicating the error type. (currently the only code is '1')
message | A message explaining what went wrong

