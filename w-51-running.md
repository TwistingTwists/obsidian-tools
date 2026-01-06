2025-12-31
Things I did last week (and todo)
- [ ] analysis of generated videos (read vinay docs on this?) - [Discord](https://discord.com/channels/945373173015265300/997567596054462495/1455072735179964458)
- [ ] elevenlabs api credits issue and PR for fixing the VideoScene Status 
- [ ] end cards issue mentioned by Rtiesh - [Discord](https://discord.com/channels/945373173015265300/997567596054462495/1455080874705162355)
- [x] first day of oncall summary in notion + [Discord](https://discord.com/channels/945373173015265300/1454425740467507293)
- [ ] 

- [ ] video id - 028fa75e-d691-40be-b64c-03ac8c8e7484 - failing first frame generation because of image saftey
	- [ ] how can I make a list of these videos in last 10 days?
- [ ] [[2026-01-05]]


2025-12-15

### bugs
[Notch](https://app.usenotch.ai/v2/video-ads/preview?videoId=c5903974-22c4-4888-af78-13de15699567&workspaceId=cm1l4oy2x019dkquee9xztrfn)
why so many scenes in this? two background music??

### bugs- backgrdoun music failed - UI fix 
```
## update on the nano-banan-pro for resizing 
1. integrated with the resize card -> [example from staging](https://staging.usemadmen.ai/v2/ad-concepts/review?idea=cmjb5e0qz0002hb3qr9kfma1g&workspaceId=cmghpdqgz0002u5xvrx7vc9u1)

2. testing end card -- added end card -> [got resized using nano banana pro](https://staging.usemadmen.ai/v2/video-ads/preview?videoId=0fcdbbf6-7f4b-44f3-8f89-926e8fecdc2d&workspaceId=cmayb5crn0000ghpt9ni6fa7o)  
```


### end-card 
- [x] test the UI integration 
- [x] UI - remove the overlay cards
- [x] generate simplified ad idea as fallback


### things to explore for location constraint on the voice -- basiaclly, we want to have the realisitic voice 
1. try v3 
2. emailed 11labs for clarficiation on voice_settings -> to see what kind of control can we have. they have sparse docs no this. 
context: [Discord](https://discord.com/channels/945373173015265300/1444935031222177812/1451486909703520369)  [notch-link](https://app.usemadmen.ai/v2/video-ads/preview?videoId=26c4805b-00ec-4e44-b9e8-558de6ed1c1b&workspaceId=cmjcbzf5200ykh4f5ri2f9ulz "https://app.usemadmen.ai/v2/video-ads/preview?videoId=26c4805b-00ec-4e44-b9e8-558de6ed1c1b&workspaceId=cmjcbzf5200ykh4f5ri2f9ulz")

## nano banana pro image

- create variant trigger -> resize (see zen table)
- on the end card ()
 
## Asset selection - person inspired flow

- [ ] find good assets related to the product
	- [ ] good = person + product (optional) in image
		- [ ] person - on white background -- AVATAR ❌
		- [ ] person in lifestyle settings -> use as a FIRST_FRAME ✅
	- [ ] How to get a good asset?
		- [ ] Start with the product ? 
		- [ ] woman -> search for it - list top 10
		- [ ] 
	- [ ] Aim: find good first frame for both scenes
- [ ] after good asset
	- [ ] use the AVATAR -> generate idea ->  gen storyboard -> frames ❌
	- [ ] use the avatar (in a lifestsyle setting) AS THE FIRST_FRAME
		- [ ] 






### Python design questions 
- how to pass the kwargs from higher level methods to the lower level methods? 
	- resize_image -> asnyc_image_eidt -> gemini api 





## BG AUDIO 

REGENRATION
### handle these cases for voice generation 
dialgoue change, scene change
- [ ] store voice_id and use that for regeneration
- [ ] single scene change 
	- [ ] bg music - ❌
	- [ ] voice change - with scene.config.AIHookConfig.voice_id - ✅
- [ ] dialgoue change
	- [ ] bg music - ❌
	- [ ] voice change - with scene.config.AIHookConfig.voice_id - ✅


- [x] Do not delete voice anymore
- [ ] # todo: abhi-bg-music: make this method async

- [ ] sync the background music on cco  - sync with semyon on frontend
	- [ ] make sure backend video scene is created BEFORE api call returns the scenes from backend - video generation prototype task 
- [ ] testing of voice change 
	- [x] get from elevenlabs 
	- [x] get from db 
	- [x] if elevenl1abs fails -> get from db whatever is the best match - do similarity testing to 0.3 and then try

- [x] background music tracing langfuse 
- [ ] generate_complete_video_ad - wait for entire video 
	- [ ] generating 
	- [ ] generated 
	- [ ] failure = on job - on failure - set job generatedAdVideo 
	- [ ] frontend logic to use the 


- [x] elevenlabs - required in config.py 
- [x] logger -- extra key for dumping json fields 


- [ ] dialogue change -> use audio of original scene 
	- [ ] voice generation should happen 
	- [ ] voice selection can be skipped - use voice_id added to db 

If video  has mutliple scenes and you change ONE scene. 


#### ignore for now - Discuss with Aman
- [ ] edge cases - if the speaker2 is not speaking -> use 
- [ ] retry generation of the scene to be handled


### ideation 
- generation of good story 
	- data driven 
	- other commercials driven



### Zen Mode - QIP (quality improvement programme)

| discord msg                                                                                        | failure                                                                                                            | fix?                 |
| -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | -------------------- |
| [Discord](https://discord.com/channels/945373173015265300/1397998114258026656/1449435614163238922) | exception": "[{'loc': ['body', 'image_url'], 'msg': 'The aspect ratio of the image should be between 0.4 and 2.5.' |                      |
| size variant in the card                                                                           | frontend -> do not generate a new prompt for that and -- method is already replaced                                | frontend needs a fix |
|                                                                                                    |                                                                                                                    |                      |

### canva template (editability) fields 

- data fields 
- layout fields as data 

## Bugs in hydrate template - ai ugc dashboard 

```
tiq.worker.WorkerThread] [ERROR] Failed to process message HydrateCollectionFirstFrames(collection_id='490ba155-91aa-4ee7-a88c-691ad76fd5de', workspace_id='cmirs9amy0000lnq5njqv1lb2', product_id='cc1ae474-8865-422e-8cc4-91fd40138e30', product_name=None, product_description=None, product_usps=None, runtime_assets={'avatars': {}, 'backgrounds': {}, 'dependencies': {'product_1': {'s3Key': 'assets/cmirs9amy0000lnq5njqv1lb2/oneone.png', 's3Bucket': 'cco-public'}}}, title=None, aspect_ratio='9_16') with unhandled exception.
Traceback (most recent call last):
  File "/home/abhishek/Downloads/experiments/vertexcover/ad-insights-data-analysis/db/generated_ad_video.py", line 356, in validate_scene_config
    return AIHookConfig(**config)
           ^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abhishek/anaconda3/envs/ad-analysis-env-nb/lib/python3.12/site-packages/pydantic/main.py", line 214, in __init__
    validated_self = self.__pydantic_validator__.validate_python(data, self_instance=self)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
pydantic_core._pydantic_core.ValidationError: 21 validation errors for AIHookConfig
sceneTemplateId
  Extra inputs are not permitted [type=extra_forbidden, input_value='f5ce2643-c601-405a-8bc1-cae59fef3cc1', input_type=str]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
sceneTemplateReferenceKey
  Extra inputs are not permitted [type=extra_forbidden, input_value='scene_1', input_type=str]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
sceneIndex
  Extra inputs are not permitted [type=extra_forbidden, input_value=0, input_type=int]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
avatarIds
  Extra inputs are not permitted [type=extra_forbidden, input_value=['avatar_raw'], input_type=list]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
backgroundIds
  Extra inputs are not permitted [type=extra_forbidden, input_value=['bg_raw'], input_type=list]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
dependencyKeys
  Extra inputs are not permitted [type=extra_forbidden, input_value=['product_1'], input_type=list]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
promptTemplate
  Extra inputs are not permitted [type=extra_forbidden, input_value='Make sure the avatar fol...s from start to finish.', input_type=str]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
dialogue
  Extra inputs are not permitted [type=extra_forbidden, input_value="You keep stuffing your p...'t ruin your whole fit.", input_type=str]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
resolvedAssets
  Extra inputs are not permitted [type=extra_forbidden, input_value={'avatars': [{'id': 'avat...ype': 'product_image'}]}, input_type=dict]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
sceneType
  Extra inputs are not permitted [type=extra_forbidden, input_value='SPEAKING', input_type=str]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
durationSeconds
  Extra inputs are not permitted [type=extra_forbidden, input_value=6.0, input_type=float]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
lipsyncEnabled
  Extra inputs are not permitted [type=extra_forbidden, input_value=False, input_type=bool]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
regenerateFirstFrame
  Extra inputs are not permitted [type=extra_forbidden, input_value=True, input_type=bool]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
firstFrameS3Key
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
firstFrameS3Bucket
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
overrides
  Extra inputs are not permitted [type=extra_forbidden, input_value={}, input_type=dict]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
audioS3Key
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
audioS3Bucket
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
audioUrl
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
elevenLabsVoiceId
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
useVoiceChange
  Extra inputs are not permitted [type=extra_forbidden, input_value=True, input_type=bool]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "/home/abhishek/anaconda3/envs/ad-analysis-env-nb/lib/python3.12/site-packages/dramatiq/worker.py", line 487, in process_message
    res = actor(*message.args, **message.kwargs)
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abhishek/Downloads/experiments/vertexcover/ad-insights-data-analysis/task_runner/task_utils.py", line 166, in __call__
    return self.fn(*args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abhishek/anaconda3/envs/ad-analysis-env-nb/lib/python3.12/site-packages/dramatiq/asyncio.py", line 67, in wrapper
    return event_loop_thread.run_coroutine(async_fn(*args, **kwargs))
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abhishek/anaconda3/envs/ad-analysis-env-nb/lib/python3.12/site-packages/dramatiq/asyncio.py", line 147, in run_coroutine
    return future.result(timeout=self.interrupt_check_ival)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abhishek/anaconda3/envs/ad-analysis-env-nb/lib/python3.12/concurrent/futures/_base.py", line 456, in result
    return self.__get_result()
           ^^^^^^^^^^^^^^^^^^^
  File "/home/abhishek/anaconda3/envs/ad-analysis-env-nb/lib/python3.12/concurrent/futures/_base.py", line 401, in __get_result
    raise self._exception
  File "/home/abhishek/anaconda3/envs/ad-analysis-env-nb/lib/python3.12/site-packages/dramatiq/asyncio.py", line 137, in wrapped_coro
    return await coro
           ^^^^^^^^^^
  File "/home/abhishek/Downloads/experiments/vertexcover/ad-insights-data-analysis/task_runner/hydrate_collection_first_frames_task.py", line 122, in hydrate_collection_first_frames_task
    response = await hydrate_collection_with_first_frames(request)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abhishek/Downloads/experiments/vertexcover/ad-insights-data-analysis/api_service/services/template_hydration.py", line 1869, in hydrate_collection_with_first_frames
    scene = await create_scene(
            ^^^^^^^^^^^^^^^^^^^
  File "/home/abhishek/Downloads/experiments/vertexcover/ad-insights-data-analysis/db/generated_ad_video.py", line 586, in create_scene
    config = validate_scene_config(config, content_type)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abhishek/Downloads/experiments/vertexcover/ad-insights-data-analysis/db/generated_ad_video.py", line 364, in validate_scene_config
    raise ValueError(
ValueError: Invalid config for content type Hook: 21 validation errors for AIHookConfig
sceneTemplateId
  Extra inputs are not permitted [type=extra_forbidden, input_value='f5ce2643-c601-405a-8bc1-cae59fef3cc1', input_type=str]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
sceneTemplateReferenceKey
  Extra inputs are not permitted [type=extra_forbidden, input_value='scene_1', input_type=str]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
sceneIndex
  Extra inputs are not permitted [type=extra_forbidden, input_value=0, input_type=int]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
avatarIds
  Extra inputs are not permitted [type=extra_forbidden, input_value=['avatar_raw'], input_type=list]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
backgroundIds
  Extra inputs are not permitted [type=extra_forbidden, input_value=['bg_raw'], input_type=list]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
dependencyKeys
  Extra inputs are not permitted [type=extra_forbidden, input_value=['product_1'], input_type=list]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
promptTemplate
  Extra inputs are not permitted [type=extra_forbidden, input_value='Make sure the avatar fol...s from start to finish.', input_type=str]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
dialogue
  Extra inputs are not permitted [type=extra_forbidden, input_value="You keep stuffing your p...'t ruin your whole fit.", input_type=str]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
resolvedAssets
  Extra inputs are not permitted [type=extra_forbidden, input_value={'avatars': [{'id': 'avat...ype': 'product_image'}]}, input_type=dict]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
sceneType
  Extra inputs are not permitted [type=extra_forbidden, input_value='SPEAKING', input_type=str]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
durationSeconds
  Extra inputs are not permitted [type=extra_forbidden, input_value=6.0, input_type=float]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
lipsyncEnabled
  Extra inputs are not permitted [type=extra_forbidden, input_value=False, input_type=bool]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
regenerateFirstFrame
  Extra inputs are not permitted [type=extra_forbidden, input_value=True, input_type=bool]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
firstFrameS3Key
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
firstFrameS3Bucket
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
overrides
  Extra inputs are not permitted [type=extra_forbidden, input_value={}, input_type=dict]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
audioS3Key
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
audioS3Bucket
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
audioUrl
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
elevenLabsVoiceId
  Extra inputs are not permitted [type=extra_forbidden, input_value=None, input_type=NoneType]
    For further information visit https://errors.pydantic.dev/2.10/v/extra_forbidden
useVoiceChange
  Extra inputs are not permitted [type=extra_forbidden, input_value=True, input_type=bool]

```