# Reolink Bash scripts examples #

All the examples below are created with the @jasonk **rl-api** bash script (for linux), **which it's an awesome script and many thanks for him**!

These examples are pretty useful when you can't define or configure some parmeters (or features) through your Home Automation Solution.

## First operation : 
- go to @jasonk [gist.github webpage](https://gist.github.com/jasonk/4772d1cd5154069cfc9eed07acb2057a) and download his bash script named rl-api.
- download it on your Home Automation Solution and make it as executable `chmod +x rl-api`

## Examples of features that you can control

----

### 1. **PRIVACY MASK : How to set differents privacy masks for each PTZ Preset (PTZ cameras AI and NON-AI)**

For those which had a PTZ camera with PTZ Presets, generally you need to redefine your privacy mask to suit with your image of the preset when you move to another one and it's boring.

Here the method how you can apply a new privacy mask after moving to a preset (**without losing the privacy masks specific to the other presets**)

**Example** : Here, i have 3 presets for my PTZ camera (id1: -preset garage-, id2 : -preset entrance- and id:3 -preset road-)
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

**IMPORTANT :** all the files json_mask_id**X** are located in the same repository of the rl-api.sh executable. 

----

### 2. **DETECTION ZONE : How to set differents detection zone for the PTZ Cameras (AI and NON-AI)**

Like the method for the privacy masks in the post bellow, here the commands (depends on cameras types) to generate multiple detection zones on the preset you choose.

Here the method how you can apply a new detection zone after moving to a preset (without losing the others detection zones) :

**Example :** Here, i have 2 presets (id1: -preset garage-, id2 : -preset entrance-)

## FOR CAMERAS NON AI (like E1 Zoom) ##

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

**IMPORTANT** : all the files json_detectionzone_idX are located in the same repository of the rl-api executable.


## FOR CAMERAS AI (**without PetDetection**) : based on the same example

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

**IMPORTANT** : all the files json_detectionzoneAI_id**X** are located in the same repository of the rl-api executable.


## FOR CAMERAS AI (**_with PetDetection_**) : based on the same example

>- For the first one which your have defined a detection zone (in my example id:1 -garage-), launch the command bellow and save the result in a file called, for example, json_detectionzoneAI_pet_id1 : \
>`./rl-api GetAiAlarm '{"channel":0,"ai_type":"people"}' | jq '{channel:.[].channel,md:.[].scope,people:.[].scope,vehicle:.[].scope,dog_cat:.[].scope}' -c > json_detectionzoneAI_pet_id1`
>
>- Switch to the second preset -id:2- and redefined a new detection zone, apply it, and and also launch the command to saved the result in a second file called json_detectionzoneAI_pet_id2 : \
> `./rl-api GetAiAlarm '{"channel":0,"ai_type":"people"}' | jq '{channel:.[].channel,md:.[].scope,people:.[].scope,vehicle:.[].scope,dog_cat:.[].scope}' -c > json_detectionzoneAI_pet_id2`
>
>After that, when you trigger PTZ Preset by id with the command (taken for @pixeldoc2000) and in my case the id:2 -entrance- \
>`./rl-api PtzCtrl '{"channel":0, "op":"ToPos", "id":2, "speed":32}`
>
>Launch, after that previous command, the command dedicated to apply the json_detectionzoneAI_pet file for id:2 : \
>`./rl-api SetAlarmArea $(cat json_detectionzoneAI_pet_id2)`

**IMPORTANT** : all the files json_detectionzoneAI_pet_id**X** are located in the same repository of the rl-api executable.







