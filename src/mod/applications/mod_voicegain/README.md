Coming soon ...

# Installation
Contents of this directory needs to be added to freeswitch code checked out from:
https://github.com/signalwire/freeswitch/
specifically into this mod applications directory
https://github.com/signalwire/freeswitch/tree/master/src/mod/applications

It needs to be compiled into FreeSWITCH.

modules.conf.xml needs to be modified to include
```
<load module="mod_voicegain"/>
```
Configuration wise mod_voicegain has the following settings in the mod_voicegain.props file:

* JWT token - used to authenticate with Voicegain platform
* default mode: either transcribe or recognize
* default settings to be used in ASR api requests
  * for large vocabulary transcription
    * langModel - language model
    * hints
    * content - specifies what is returned in the incremental and/or final response
      * incremental
      * full
  * for grammar-based recognition
    * grammar(s) - one or more grammars
    * incompleteTimeout
    * continuousRecognition parameters - continuous recognition is described in this article.
      * enable
      * stopOn
      * noCallbackFor
  * shared config between transcription and recognition
    * acousticModel - allows to switch between acoustic models used
    * maxAlternatives
    * confidenceThreshold
    * noInputTimeout
    * completeTimeout
    * callback - optional way to get the results
      * uri
    * startTimers - whether timers should be started immediately or only when vg_asr_start_timers is called
    * startOfInput - enable start-of-input event
 
