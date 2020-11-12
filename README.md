Coming soon ...

# mod_voicegain
Speech-to-Text interface module for FreeSWITCH

mod_voicegain is a module that allows FreeSWITCH applications to use Voicegain ASR for either large vocabulary transcription or grammar based recognition.

NOTE: This module is currently in alpha release - please contact us for availability.

# How it works
mod_voicegain taps into the FreeSWITCH inbound audio stream and sends the audio data to Voicegain ASR in the Cloud. Voicegain ASR processes the audio according to the invocation parameters specified in the data argument. It then communicates the result of transcription or recognition.

## How to invoke it
mod_voicegain installs on FreeSWITCH as an app and can be invoked as a such, e.g.
```
<action application="vg_asr_start" data=""/>
```
or from LUA script:
```
session:execute("vg_asr_start", "");
```
Recognition or transcription will stop automtically when specified timeout (noinput/incomplete/complete) values expire. Alternatively, it is possible to call a second command provided by the module - "vg_asr_stop" - to stop the transcription or recognition at any moment.

If the data argument is "" (empty) then the default data specified in mod_voicegain.props configuration will be used. To do transcription or recognition with custom configuration, the fields that override the defaults need to be passed in data as a JSON formatted string e.g.
```
<action application="vg_asr_start" 
        data="{'mode':'transcribe','hints':['yes','no'],'callback':{'uri':'http://my.host.com/my-callback'}}"/>
```

## Special question prompt support
This module has features that support natural behavior in presence of question prompts. These are:

* Barge-in support - it is possible to request the start-of-input event to be sent so that question prompt playback may be stopped as soon as the caller starts speaking
* Start timers at then end of the prompt - generally all ASR timers should start only when the question prompt stops playing. It is possible to do that by setting startTimers to false and the after prompt finishes playing to call vg_asr_start_timers

## How to get the results
Results will always be returned as a FreeSWITCH event but it is also possible to get the results in a callback to the url specified in callback.uri

The FreeSWITCH event will be of custom type (Event-Name: CUSTOM)  and Event-Subclass will be "voicegain_asr_update". The relevant payload will be in the  "ASR-Response" field formatted as JSON. The JSON data payload will be as documented in the Web API  https://console.voicegain.ai/api-documentation#operation/asrRecognizeCallback and https://console.voicegain.ai/api-documentation#operation/asrTranscribeCallback
