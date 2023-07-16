# Reolink Bash scripts examples #

All the examples below are created with the @jasonk **rl-api** bash script (for linux), **which it's an awesome script and many thanks for him**!

These examples are pretty useful when you can't define or configure some parameters (or features) through your Home Automation Solution.

**!! THESE EXAMPLES WORK ONLY WITH REOLINK CAMERAS WITH CGI/API SUPPORT !!** \
Please, take a look at the specifications of your camera on the Reolink official website

**!!IMPORTANT!! 
- these scripts work with standalone cameras - They aren't tested with NVR or binocular cameras**
- these scripts work on linux OS

## First operation :
- go to @jasonk [gist.github webpage](https://gist.github.com/jasonk/4772d1cd5154069cfc9eed07acb2057a) and download his bash script named rl-api.
- dependencies for the script : executables curl and jq.
- download it on your Home Automation Solution and make it as executable `chmod +x rl-api`

## Second operation : 
- copy the script rl-api to another one that correspond to the camera IP address (example, my IP finish by 60, so i rename it rl-api60).
- make it's new script as executable (for my example`chmod +x rl-api60`)
- edit the file rl-api (here rl-api60) and change at the beginning of the script your username, password and IP address of the camera (192.xxx.xxx.60).
- Do the same thing for the others cameras.

## Examples of features that you can control

### Summary
 1. [PRIVACY MASK : How to create and set differents privacy masks for each PTZ Preset (PTZ cameras AI and NON-AI)](README.md#1-privacy-mask--how-to-create-and-set-differents-privacy-masks-for-each-ptz-preset-ptz-cameras-ai-and-non-ai)
 2. [DETECTION ZONE : How to create and set differents detection zone (PTZ Cameras AI and NON-AI)](README.md#2-detection-zone--how-to-create-and-set-differents-detection-zone-ptz-cameras-ai-and-non-ai)
 3. [VIDEO CLIP : How to create and set different zones of the main image as clips zones files and how to display one of these video clips zones (ALL Cameras)](README.md#3-video-clip--how-to-create-and-set-different-zones-of-the-main-image-as-clips-zones-files-and-how-to-display-one-of-these-video-clips-zones-all-cameras)
 4. [SIREN ACTIVATION : manual mode or repeat mode](README.md#4-siren-activation--manual-mode-or-repeat-mode)
 5. [NTP : Force synchronization](README.md#5-ntp--force-synchronization)

----
### 1. **PRIVACY MASK : How to create and set differents privacy masks for each PTZ Preset (PTZ cameras AI and NON-AI)**
<details>
<summary>HOW-TO detailed</summary>

> __Warning__ \
**EDIT :** This method doesn't work with TrackMix Cameras (or Cameras that Privacy Mask options are : Wide-Angle Image AND Telephoto Image)
 
For those which had a PTZ camera with PTZ Presets, generally you need to redefine your privacy mask to suit with your image of the preset when you move to another one and it's boring.

Here the method how to create several privacy masks and how to apply a new privacy mask after moving to a preset (**without losing the privacy masks specific to the other presets**)

**Example** : Here, i have 3 presets for my PTZ camera (id1: -preset garage-, id2 : -preset entrance- and id:3 -preset road-) \

> __Warning__ \
**!!IMPORTANT!!** use the right script that your camera depends on

>
> - For the first one which your have defined a privacy mask (in my example **id:1** -garage-), launch the command bellow and save the result in a file called, for example, json_mask_id1 : \
>`./rl-api GetMask '{"channel":0}' | jq '.' -c > json_mask_id1`
>
>- Switch to the second preset -id:2- and redefined a new privacy mask, apply it, and and also launch the command to saved the result in a second file called json_mask_id2: \
>`./rl-api GetMask '{"channel":0}' | jq '.' -c > json_mask_id2`
>
>- Switch to the third preset -id:3- and do the same method like the second preset and save the result in a file called json_mask_id3 : \
>`./rl-api GetMask '{"channel":0}' | jq '.' -c > json_mask_id3`
>
>After that, when you trigger PTZ Preset by id with the command and in my case the id:2 entrance- \
>`./rl-api PtzCtrl '{"channel":0, "op":"ToPos", "id":2, "speed":32}`
>
>Launch, after that previous command, the command dedicated to apply the privacy mask file for id:2 : \
>`./rl-api SetMask $(cat json_mask_id2)`

> __Warning__ \
 **IMPORTANT :** 
   * all the files json_mask_id**X** are located in the same repository of the rl-api executable.
   * use the right script that your camera depends on

**NB :** If you need to activate/desactivate mask privacy ONLY, here is the command : 

>`./rl-api SetMask '{"channel":0, "enable":1}' ` -> to enable the privacy mask\
>`./rl-api SetMask '{"channel":0, "enable":0}' ` -> to disable the privacy mask
</details>

----

### 2. **DETECTION ZONE : How to create and set differents detection zone (PTZ Cameras AI and NON-AI)**
<details>
<summary>HOW-TO detailed</summary>

Like the method for the privacy masks in the post bellow, here the commands (depends on cameras types) to generate multiple detection zones on the preset you choose.

Here the method how to create several detection zones and how to apply a new detection zone after moving to a preset (**without losing the others detection zones**) :

**Example :** Here, i have 2 presets (id1: -preset garage-, id2 : -preset entrance-) \
**!!IMPORTANT!!** Like the privacy mask, use the right script that your camera depends on

## FOR CAMERAS NON AI (like E1 Zoom) ##
<details>
<summary>HOW-TO detailed</summary>
 
>- For the first one which your have defined a detection zone (in my example id:1 -garage-), launch the command bellow and save the result in a file called, for example, json_detectionzone_id1 : \
>`./rl-api GetMdAlarm '{"channel":0}' | jq '.[]|={channel,scope}' -c > json_detectionzone_id1`
>
>- Switch to the second preset -id:2- and redefined a new detection zone (here the entrance), apply it, and and also launch the command to saved the result in a second file called json_detectionzone_id2 : \
>`./rl-api GetMdAlarm '{"channel":0}' | jq '.[]|={channel,scope}' -c > json_detectionzone_id2`
>
>After that, when you trigger PTZ Preset by id with the command and in my case the id:2 entrance- \
>`./rl-api PtzCtrl '{"channel":0, "op":"ToPos", "id":2, "speed":32}`
>
>Launch, after that previous command, the command dedicated to apply the detectionzone file for `id:2` : \
>`./rl-api SetMdAlarm $(cat json_detectionzone_id2)`

**IMPORTANT** : all the files json_detectionzone_idX are located in the same repository of the rl-api executable. \
**!!IMPORTANT!!** use the right script that your camera depends on
</details>

## FOR CAMERAS AI (**without PetDetection**) : based on the same example
<details>
<summary>HOW-TO detailed</summary>
 
>- For the first one which your have defined a detection zone (in my example id:1 -garage-), launch the command bellow and save the result in a file called, for example, json_detectionzoneAI_id1 : \
>`./rl-api GetAiAlarm '{"channel":0,"ai_type":"people"}' | jq '{channel:.[].channel,md:.[].scope,people:.[].scope,vehicle:.[].scope}' -c > json_detectionzoneAI_id1`
>
>- Switch to the second preset -id:2- and redefined a new detection zone (here the entrance), apply it, and and also launch the command to saved the result in a second file called json_detectionzoneAI_id2 : \
>`./rl-api GetAiAlarm '{"channel":0,"ai_type":"people"}' | jq '{channel:.[].channel,md:.[].scope,people:.[].scope,vehicle:.[].scope}' -c > json_detectionzoneAI_id2`
>
>After that, when you trigger PTZ Preset by id with the command (taken for @pixeldoc2000) and in my case the id:2 entrance- \
>`./rl-api PtzCtrl '{"channel":0, "op":"ToPos", "id":2, "speed":32}`
>
>Launch, after that previous command, the command dedicated to apply the json_detectionzoneAI file for `id:2` : \
>`./rl-api SetAlarmArea $(cat json_detectionzoneAI_id2)`

**IMPORTANT** : all the files json_detectionzoneAI_id**X** are located in the same repository of the rl-api executable. \
**!!IMPORTANT!!** use the right script that your camera depends on
</details>

## FOR CAMERAS AI (**_with PetDetection_**) : based on the same example
<details>
<summary>HOW-TO detailed</summary>
 
>- For the first one which your have defined a detection zone (in my example id:1 -garage-), launch the command bellow and save the result in a file called, for example, json_detectionzoneAI_pet_id1 : \
>`./rl-api GetAiAlarm '{"channel":0,"ai_type":"people"}' | jq '{channel:.[].channel,md:.[].scope,people:.[].scope,vehicle:.[].scope,dog_cat:.[].scope}' -c > json_detectionzoneAI_pet_id1`
>
>- Switch to the second preset -id:2- and redefined a new detection zone, apply it, and also launch the command to saved the result in a second file called json_detectionzoneAI_pet_id2 : \
> `./rl-api GetAiAlarm '{"channel":0,"ai_type":"people"}' | jq '{channel:.[].channel,md:.[].scope,people:.[].scope,vehicle:.[].scope,dog_cat:.[].scope}' -c > json_detectionzoneAI_pet_id2`
>
>After that, when you trigger PTZ Preset by id with the command (taken for @pixeldoc2000) and in my case the id:2 -entrance- \
>`./rl-api PtzCtrl '{"channel":0, "op":"ToPos", "id":2, "speed":32}`
>
>Launch, after that previous command, the command dedicated to apply the json_detectionzoneAI_pet file for id:2 : \
>`./rl-api SetAlarmArea $(cat json_detectionzoneAI_pet_id2)`

**IMPORTANT** : all the files json_detectionzoneAI_pet_id**X** are located in the same repository of the rl-api executable. \
**!!IMPORTANT!!** use the right script that your camera depends on
</details>
</details>

----

### 3. **VIDEO CLIP : How to create and set different zones of the main image as clips zones files and how to display one of these video clips zones ~~(ALL Cameras)~~**
<details>
<summary>HOW-TO detailed</summary>
 
On Reolink cameras (not all), you have the feature Video clip that permit to have a live zoom of part of the main image.

In that section, you have the possibility to generate several video clip zones (to focus on some particular sectors of the main image). 

After defining them, you are able to choose the video clip zone you want to looking at, and finally how to display this video clip live zoom stream through http(s) and rtmp live stream URLs 

Here the method how to create several video clip zones and how to apply one of them for display it (**without losing the others video clip zones**) :
 
**!! VERY IMPORTANT !!** : To verify if your camera support videoclip, type that command command : 
 > `./rl-api GetCrop '{"channel":0}'`
 
If you get one of these return message below, your camera do not support videoclips :  
 > GetCrop ERROR: ability error (-26) \
 > GetCrop ERROR: unknown (-9)

**Example :** Here, i want to focus on two particular sectors of the main image (zone1 : Front door, zone2 : Letter box) \
**!! IMPORTANT !!** like the privacy mask, use the right script that your camera depends on

>- For the first one (front door) which your have defined a video clip zone, launch the command bellow and save the result in a file called, for example, json_clip_frontdoor : \
>`./rl-api GetCrop '{"channel":0}' | jq '.[]|=.*{"screenHeight":.mainHeight,"screenWidth":.mainWidth}|del(.Crop.minHeight,.Crop.minWidth)' -c > json_clip_frontdoor`
>
>- For the second one (letter box), modify the sector you want to see and apply it. After that, launch the command to saved the result in a second file called json_clip_letterbox : \
>`./rl-api GetCrop '{"channel":0}' | jq '.[]|=.*{"screenHeight":.mainHeight,"screenWidth":.mainWidth}|del(.Crop.minHeight,.Crop.minWidth)' -c > json_clip_letterbox`
>
>When there is an event (doorbell ringing > view frontdoor zoom), launch the command dedicated to apply the json_clip_frontdoor file for zooming to the frontdoor: \
>`./rl-api SetCrop $(cat json_clip_frontdoor)`

### HOW TO DISPLAY THE VIDEO CLIP : 
<details>
<summary>HOW-TO detailed</summary>
 
There are 2 methods possibles to see the video clip zoom live stream :
 - HTTP or HTTPS Url
> `http(s)://#IP#/flv?port=1935&app=bcs&stream=channel0_mobile.bcs&user=#username#&password=#password#` \
> (Remplace #IP#, #username#, #password# by your own values)
 - RTMP
> `rtmp://#IP#/bcs/channel0_mobile.bcs?channel=0&stream=0&user=#username#&password=#password#` \
> (Remplace #IP#, #username#, #password# by your own values)

**IMPORTANT** : 
 - RTMP or HTTP(HTTPS) must be open on your camera to have a display of the sector you zoom to. In my example, the front door
 - Verify if your home automation solution able to display one of these streams.
</details>
</details>

----

### 4. SIREN ACTIVATION : manual mode or repeat mode
<details>
<summary>HOW-TO detailed</summary>

 For those which have a recent cameras (AI) There are 2 modes for triggering the siren : the manual mode and the repeat mode. Here are the methods to use these, depends on what you want. \
 **!! IMPORTANT !!** : these commands do not work with non AI cameras because this command isn't included in their firmware.
 
 **SIREN in manual mode**\
 → turn the siren ON\
 `./rl-api AudioAlarmPlay '{"alarm_mode": "manul", "manual_switch": 0, "channel": 0 }'`

 → turn the siren OFF\
`./rl-api AudioAlarmPlay '{"alarm_mode": "manul", "manual_switch": 1, "channel": 0 }'`

 **SIREN in repeat mode** 
 
In the command below, change **#NBER_SECONDS#** by the time in seconds you want to hear the siren (example : 10 → 10 seconds)

`./rl-api AudioAlarmPlay '{"channel":0,"alarm_mode":"times","times":#NBER_SECONDS#}'`
</details>

----

### 5. NTP : Force synchronization
<details>
<summary>HOW-TO detailed</summary>

 → Force NTP synchronization\
 `./rl-api SetNtp $(./rl-api GetNtp '{"channel":0}' | jq '.Ntp.interval = 0' -c)`
