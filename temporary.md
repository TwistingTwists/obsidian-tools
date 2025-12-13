---
aliases:
---

```
two people alex and rahul are there 

first scene - alex speaks about his unorganised way of life . rahul is attentively listening

scene scene - rahul speaks about sling bag as  solution . alex is smiling and listening. 
```


```
scene - 1 - person alex is speaking aobut problems of not finding items 
He is speaking to romi. romi is listening.

scene - 2 - romi is speaking to alex. alex is listening about benefits of the sling bag

```

two people in scene - alex and romi - alex talking  to romi about how to organise 

second scene - both people visible - romi replying back - hey this bag is awesome - use tihis and other benefits of sling bag 

make sure that in scene 1 alex is speaking and in scene 2 romi is replying. this is important

```json
{"scene_type": "SPEAKING", "dialogue_prompt": "Hook line introducing the product", "lipsync_enabled": false, "duration_seconds": 6.0, "regenerate_first_frame": true}
```



```json
{"summary": "A young man named Leo vlogs on a sunny campus.\nHe walks towards the camera, smiling enthusiastically.\nHe proudly talks about showing off his Harry Potter fandom through his style.", "scene_order": 0, "template_id": null, "scene_prompt": "The scene is a continuous point-of-view shot from a smartphone on a selfie stick. A charismatic man in his mid-20s named Leo, with messy brown hair and a light beard, is vlogging while walking confidently through a modern university campus during the golden hour. The background shows blurred figures of other students and contemporary architecture under a warm, sunny sky. Leo is wearing a black oversized t-shirt with a subtle graphic. He looks directly into the camera with a bright, engaging smile. Ambient sounds include the distant chatter of students and the gentle rustle of leaves. Leo gestures with his free hand as he speaks with an energetic and proud voice. Leo says: \"Being a Potterhead doesn't mean you can't be stylish. I love how I can rep my fandom anywhere.\" His expression is one of pure, unadulterated joy and confidence in his style.", "custom_config": {"usp": null, "dialogue": "Being a Potterhead doesn't mean you can't be stylish. I love how I can rep my fandom anywhere.", "settings": null, "characters": [{"is_speaking": false, "voice_profile": "An upbeat, friendly male voice in his mid-20s with a standard American accent. He speaks clearly and conversationally, with an energetic pace and enthusiastic tone.", "character_name": "Leo"}], "scene_description": "POV from a selfie stick camera held by a man in his mid-20s, vlogging as he walks. He has brown hair, a light beard, and a wide, friendly smile. He's wearing a black oversized graphic t-shirt. The background is a bustling, modern university campus with green lawns and contemporary buildings, slightly blurred. Golden hour lighting, warm and inviting atmosphere. Cinematic, eye-level shot.", "first_frame_s3_key": "cmirs9amy0000lnq5njqv1lb2/c8ca6e0b-961f-4349-bd20-fc9b8c6ef3a5/e28a313a-9ac1-417b-b6b6-54a9d56ebbf8/hook_first_frame/3559.png", "reference_image_url": null, "first_frame_s3_bucket": "cco-public"}, "moderation_details": null, "customisation_suggestions": [{"short_title": "Confident female student vlogging on campus", "change_request": "Replace Leo with a charismatic young woman in her early 20s, with a modern haircut and confident smile. She is wearing the Men's Black Order Of The Phoenix Graphic Printed Oversized T-shirt, walking through the same university campus during golden hour, vlogging with the same energetic and proud demeanor.", "require_assets": false}, {"short_title": "Relaxed male student sharing fandom on campus", "change_request": "Replace Leo with a male character in his late teens or early 20s, with a more relaxed, slightly introverted but still confident vibe, perhaps with glasses or a beanie, wearing the Men's Black Order Of The Phoenix Graphic Printed Oversized T-shirt. He's vlogging through the university campus, sharing his passion for fandom and comfort.", "require_assets": false}, {"short_title": "Potterhead vlogging in a trendy urban street", "change_request": "Keep Leo (or a similar male character) but change the environment. Instead of a university campus, he is vlogging while walking confidently through a vibrant, trendy urban street or a bustling outdoor market, emphasizing the shirt's versatility for city outings. The background shows diverse people and modern storefronts.", "require_assets": false}, {"short_title": "Clearly showcasing Order of Phoenix graphic", "change_request": "Ensure the character (Leo or similar) is clearly wearing the Men's Black Order Of The Phoenix Graphic Printed Oversized T-shirt. The 'Order of the Phoenix' graphic should be distinctly visible and recognizable, perhaps with a slight adjustment in pose or camera angle to highlight the design on the shirt while maintaining the vlogging POV.", "require_assets": true}]}
```



---
## debugging  who is speaking 


```json 
{"summary": null, "scene_order": 0, "template_id": null, "scene_prompt": "The scene opens on a continuous, cinematic shot in a luxurious hotel restaurant. The ambient sound is a low murmur of conversation and the gentle clinking of cutlery. Eleanor, a sophisticated woman in her mid-30s with dark hair styled in a low bun and wearing a cream silk blouse, sits alone at a beautifully set table. Soft, warm light illuminates her and the Madeline 21 in Canvas Chocolate bag resting on the table next to her hand. The camera slowly pushes in on her as she gazes thoughtfully into the distance, her expression calm and contemplative. Her fingers gently trace the outline of the bag. The atmosphere is serene and elegant. Eleanor says in a soft, reflective internal monologue: \"I’ve learned that true elegance isn’t about having more, it’s about choosing what truly elevates you.\"", "custom_config": {"usp": null, "dialogue": "I’ve learned that true elegance isn’t about having more, it’s about choosing what truly elevates you.", "settings": null, "characters": [{"is_speaking": true, "voice_profile": "A soft, reflective female voice, approximately 35 years old, with a clear, standard American accent. Her tone is calm and contemplative, with a medium pitch and a thoughtful, measured pace. The voice is smooth and elegant.", "character_name": "Eleanor"}], "scene_description": "A sophisticated woman in her mid-30s with dark hair in a low bun, wearing a cream silk blouse, sits at a table in a fancy hotel restaurant. She is holding the Madeline 21 in Canvas Chocolate bag, which rests on the table beside her. Medium shot. The restaurant has a luxurious ambiance with soft, ambient lighting, creating an elegant and intimate atmosphere. Cinematic, production-grade photo with subtle depth of field. Exclusions: no text, no UI, no packaging.", "first_frame_s3_key": "cmj2po1sv00d1ry93379ig8ff/ed4104dd-b72a-4e05-b65f-604ef5670cdb/8296b1ec-af56-4771-bd0a-5beb0396cf4e/hook_first_frame/6797.png", "reference_image_url": null, "first_frame_s3_bucket": "cco-public"}, "moderation_details": null, "customisation_suggestions": null}
```

---


```json
{"s3_key": "cmirs9amy0000lnq5njqv1lb2/e7894efc-bfc7-4de7-b4f6-3b9f3550fb6d/b6edbce7-4e08-46ba-9242-18450239c14a/video/voice_change_8ec4f386.mp4", "s3_bucket": "cco-public", "exported_at": null, "has_watermark": null, "thumbnail_s3_key": null, "last_frame_s3_key": null, "thumbnail_s3_bucket": null, "last_frame_s3_bucket": null}
```


```json
{"s3_key": "cmirs9amy0000lnq5njqv1lb2/e7894efc-bfc7-4de7-b4f6-3b9f3550fb6d/b6edbce7-4e08-46ba-9242-18450239c14a/video/voice_change_8ec4f386.mp4", "s3_bucket": "cco-public", "exported_at": null, "has_watermark": null, "thumbnail_s3_key": null, "last_frame_s3_key": null, "thumbnail_s3_bucket": null, "last_frame_s3_bucket": null}
```


```md

[Fetching Title#z7aj](https://cco-public.s3.amazonaws.com/cmirs9amy0000lnq5njqv1lb2/e7894efc-bfc7-4de7-b4f6-3b9f3550fb6d/b6edbce7-4e08-46ba-9242-18450239c14a/video/voice_change_8ec4f386.mp4)




[Fetching Title#adrx](https://cco-public.s3.amazonaws.com/cmirs9amy0000lnq5njqv1lb2/e7894efc-bfc7-4de7-b4f6-3b9f3550fb6d/3101b1c2-dc2f-4253-bd87-a04950553082/video/voice_change_66488e8e.mp4)


[Fetching Title#s5jj](https://cco-public.s3.amazonaws.com/cmirs9amy0000lnq5njqv1lb2/e7894efc-bfc7-4de7-b4f6-3b9f3550fb6d/3101b1c2-dc2f-4253-bd87-a04950553082/video/hook.mp4)


```

## video one 
- [Notch](https://app.usenotch.ai/v2/video-ads/preview?idea=cmj166ici00fs6r3om6ivern5&workspaceId=cmiskf19n006638i7ppwbguyl&videoId=8672a3c1-f023-47e5-9816-1f1eed4cbc34) 

[Fetching Title#ejww](https://cco-public.s3.amazonaws.com/ws_demo_001/video_demo_001/scene_demo_001/video/voice_change_757c263b.mp4)
[Fetching Title#5s4c](https://cco-public.s3.amazonaws.com/ws_demo_001/video_demo_002/scene_demo_002/video/voice_change_3e2c69fe.mp4)


## video two 
- [Fetching Title#uaxe](http://cco-public.s3.amazonaws.com/cmj0xextb00k6k9be5qxldy4p/791f3605-bbc7-4cbe-aaa2-14acd0493a3a/ac46befb-2c7f-4e46-bfc7-d877572dbeb0/video/hook.mp4)
- [Fetching Title#lypv](https://cco-public.s3.amazonaws.com/cmj0xextb00k6k9be5qxldy4p/791f3605-bbc7-4cbe-aaa2-14acd0493a3a/5df8c3c0-84fc-4628-97aa-632178f3188c/video/hook.mp4)

[Fetching Title#blsb](https://cco-public.s3.amazonaws.com/ws_demo_001/video_demo_001/scene_demo_001/video/voice_change_57ff4f0b.mp4)
[Fetching Title#0fmq](https://cco-public.s3.amazonaws.com/ws_demo_001/video_demo_002/scene_demo_002/video/voice_change_f10828e1.mp4)


## video three 

[Title Unavailable \| Site Unreachable](https://app.usenotch.ai/v2/ad-concepts/review?idea=cmj16k6yl00gfkhthjhwpa3pa&workspaceId=cmj0vbv5200df14fz6zdhcfap)

[Fetching Title#yrgk](https://cco-public.s3.amazonaws.com/ws_demo_001/video_demo_002/scene_demo_002/video/voice_change_0da2a788.mp4)
[Fetching Title#z820](https://cco-public.s3.amazonaws.com/ws_demo_001/video_demo_001/scene_demo_001/video/voice_change_43736001.mp4)



----

## music results 
with  background noise removed

[part1](https://cco-public.s3.amazonaws.com/ws_demo_001/video_demo_002/scene_demo_002/video/voice_change_c07fdace.mp4)
[part2](https://cco-public.s3.amazonaws.com/ws_demo_001/video_demo_001/scene_demo_001/video/voice_change_93132a91.mp4)


without background noise removed 

[part_1](https://cco-public.s3.amazonaws.com/ws_demo_001/video_demo_001/scene_demo_001/video/voice_change_65ade40d.mp4)
[part_2](https://cco-public.s3.amazonaws.com/ws_demo_001/video_demo_001/scene_demo_001/video/voice_change_65ade40d.mp4)

-----


with background noise removed 
```json

{'voice_creations': [{'prompt_fingerprint': '1aebd42bd55fb45a6f05f899f2faa988', 'voice_id': 'itkUuCeluzmxnISkRimf', 'generated_voice_id': 'itkUuCeluzmxnISkRimf', 'voice_name': 'Voice_001', 'voice_description': 'oneoneoneoneoneoneoneoneon', 'chosen_preview': {'public_owner_id': '2e1625cb5873bb543befd3177d044933e239412bbc64fedf5ed02517c3b3c8f9', 'voice_id': 'itkUuCeluzmxnISkRimf', 'date_unix': 1714018360, 'name': 'Graham - Young Teacher', 'accent': 'american', 'gender': 'male', 'age': 'young', 'descriptive': 'casual', 'use_case': 'conversational', 'category': <LibraryVoiceResponseModelCategory.PROFESSIONAL: 'professional'>, 'language': 'en', 'locale': 'en-US', 'description': 'Graham - 20 year old white male voice. Perfect for casual conversation.', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3', 'usage_character_count_1y': 34308020, 'usage_character_count_7d': 251504, 'play_api_usage_character_count_1y': 0, 'cloned_by_count': 1899, 'rate': 1.0, 'fiat_rate': None, 'free_users_allowed': True, 'live_moderation_enabled': True, 'featured': False, 'verified_languages': [{'language': 'en', 'model_id': 'eleven_turbo_v2_5', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_v2_flash', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_flash_v2', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_turbo_v2', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_multilingual_v2', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_v2_5_flash', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_flash_v2_5', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}], 'notice_period': None, 'instagram_username': None, 'twitter_username': None, 'youtube_username': None, 'tiktok_username': None, 'image_url': '', 'is_added_by_user': False}, 'create_response': {'voice_id': 'itkUuCeluzmxnISkRimf', 'source': 'similar_voice_lookup'}, 'design_response': {}}], 'video_results': [{'scene_id': 'scene_demo_001', 'video_id': 'video_demo_001', 'workspace_id': 'ws_demo_001', 'source_bucket': 'cco-public', 'source_key': 'cmhp3fipi000j10xfjr8nirz0/81ab354f-d3be-42e8-9801-0d442d756394/04c1cd91-9f63-4a50-a1fa-d0007ff39353/video/hook.mp4', 'prompt_fingerprint': '1aebd42bd55fb45a6f05f899f2faa988', 'target_voice_id': 'itkUuCeluzmxnISkRimf', 'output_bucket': 'cco-public', 'output_key': 'ws_demo_001/video_demo_001/scene_demo_001/video/voice_change_93132a91.mp4'}, {'scene_id': 'scene_demo_002', 'video_id': 'video_demo_002', 'workspace_id': 'ws_demo_001', 'source_bucket': 'cco-public', 'source_key': 'cmhp3fipi000j10xfjr8nirz0/81ab354f-d3be-42e8-9801-0d442d756394/0169978b-06b3-4752-8e55-38074f6e61ae/video/hook.mp4', 'prompt_fingerprint': '1aebd42bd55fb45a6f05f899f2faa988', 'target_voice_id': 'itkUuCeluzmxnISkRimf', 'output_bucket': 'cco-public', 'output_key': 'ws_demo_001/video_demo_002/scene_demo_002/video/voice_change_c07fdace.mp4'}]}

```



without background noise removed 
```json



{'voice_creations': [{'prompt_fingerprint': 'f5fc3f59294b985231f33bd01a205ed0', 'voice_id': 'itkUuCeluzmxnISkRimf', 'generated_voice_id': 'itkUuCeluzmxnISkRimf', 'voice_name': 'Voice_001', 'voice_description': '', 'chosen_preview': {'public_owner_id': '2e1625cb5873bb543befd3177d044933e239412bbc64fedf5ed02517c3b3c8f9', 'voice_id': 'itkUuCeluzmxnISkRimf', 'date_unix': 1714018360, 'name': 'Graham - Young Teacher', 'accent': 'american', 'gender': 'male', 'age': 'young', 'descriptive': 'casual', 'use_case': 'conversational', 'category': <LibraryVoiceResponseModelCategory.PROFESSIONAL: 'professional'>, 'language': 'en', 'locale': 'en-US', 'description': 'Graham - 20 year old white male voice. Perfect for casual conversation.', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3', 'usage_character_count_1y': 34308020, 'usage_character_count_7d': 251504, 'play_api_usage_character_count_1y': 0, 'cloned_by_count': 1899, 'rate': 1.0, 'fiat_rate': None, 'free_users_allowed': True, 'live_moderation_enabled': True, 'featured': False, 'verified_languages': [{'language': 'en', 'model_id': 'eleven_turbo_v2_5', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_v2_flash', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_flash_v2', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_turbo_v2', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_multilingual_v2', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_v2_5_flash', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_flash_v2_5', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}], 'notice_period': None, 'instagram_username': None, 'twitter_username': None, 'youtube_username': None, 'tiktok_username': None, 'image_url': '', 'is_added_by_user': False}, 'create_response': {'voice_id': 'itkUuCeluzmxnISkRimf', 'source': 'similar_voice_lookup'}, 'design_response': {}}], 'video_results': [{'scene_id': 'scene_demo_001', 'video_id': 'video_demo_001', 'workspace_id': 'ws_demo_001', 'source_bucket': 'cco-public', 'source_key': 'cmhp3fipi000j10xfjr8nirz0/81ab354f-d3be-42e8-9801-0d442d756394/04c1cd91-9f63-4a50-a1fa-d0007ff39353/video/hook.mp4', 'prompt_fingerprint': 'f5fc3f59294b985231f33bd01a205ed0', 'target_voice_id': 'itkUuCeluzmxnISkRimf', 'output_bucket': 'cco-public', 'output_key': 'ws_demo_001/video_demo_001/scene_demo_001/video/voice_change_be5bf940.mp4'}, {'scene_id': 'scene_demo_002', 'video_id': 'video_demo_002', 'workspace_id': 'ws_demo_001', 'source_bucket': 'cco-public', 'source_key': 'cmhp3fipi000j10xfjr8nirz0/81ab354f-d3be-42e8-9801-0d442d756394/0169978b-06b3-4752-8e55-38074f6e61ae/video/hook.mp4', 'prompt_fingerprint': 'f5fc3f59294b985231f33bd01a205ed0', 'target_voice_id': 'itkUuCeluzmxnISkRimf', 'output_bucket': 'cco-public', 'output_key': 'ws_demo_001/video_demo_002/scene_demo_002/video/voice_change_1cf9b1fc.mp4'}]}

```


without remove background noise 

```json 

{'voice_creations': [{'prompt_fingerprint': 'f5fc3f59294b985231f33bd01a205ed0', 'voice_id': 'itkUuCeluzmxnISkRimf', 'generated_voice_id': 'itkUuCeluzmxnISkRimf', 'voice_name': 'Voice_001', 'voice_description': '', 'chosen_preview': {'public_owner_id': '2e1625cb5873bb543befd3177d044933e239412bbc64fedf5ed02517c3b3c8f9', 'voice_id': 'itkUuCeluzmxnISkRimf', 'date_unix': 1714018360, 'name': 'Graham - Young Teacher', 'accent': 'american', 'gender': 'male', 'age': 'young', 'descriptive': 'casual', 'use_case': 'conversational', 'category': <LibraryVoiceResponseModelCategory.PROFESSIONAL: 'professional'>, 'language': 'en', 'locale': 'en-US', 'description': 'Graham - 20 year old white male voice. Perfect for casual conversation.', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3', 'usage_character_count_1y': 34308020, 'usage_character_count_7d': 251504, 'play_api_usage_character_count_1y': 0, 'cloned_by_count': 1899, 'rate': 1.0, 'fiat_rate': None, 'free_users_allowed': True, 'live_moderation_enabled': True, 'featured': False, 'verified_languages': [{'language': 'en', 'model_id': 'eleven_turbo_v2_5', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_v2_flash', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_flash_v2', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_turbo_v2', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_multilingual_v2', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_v2_5_flash', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}, {'language': 'en', 'model_id': 'eleven_flash_v2_5', 'accent': 'american', 'locale': 'en-US', 'preview_url': 'https://storage.googleapis.com/eleven-public-prod/M0SVaDFrG9PZ1XNq61lXO6YhVL83/voices/itkUuCeluzmxnISkRimf/ceb155ae-ac9f-4af6-b992-a799727d8c5c.mp3'}], 'notice_period': None, 'instagram_username': None, 'twitter_username': None, 'youtube_username': None, 'tiktok_username': None, 'image_url': '', 'is_added_by_user': False}, 'create_response': {'voice_id': 'itkUuCeluzmxnISkRimf', 'source': 'similar_voice_lookup'}, 'design_response': {}}], 'video_results': [{'scene_id': 'scene_demo_001', 'video_id': 'video_demo_001', 'workspace_id': 'ws_demo_001', 'source_bucket': 'cco-public', 'source_key': 'cmhp3fipi000j10xfjr8nirz0/81ab354f-d3be-42e8-9801-0d442d756394/04c1cd91-9f63-4a50-a1fa-d0007ff39353/video/hook.mp4', 'prompt_fingerprint': 'f5fc3f59294b985231f33bd01a205ed0', 'target_voice_id': 'itkUuCeluzmxnISkRimf', 'output_bucket': 'cco-public', 'output_key': 'ws_demo_001/video_demo_001/scene_demo_001/video/voice_change_65ade40d.mp4'}, {'scene_id': 'scene_demo_002', 'video_id': 'video_demo_002', 'workspace_id': 'ws_demo_001', 'source_bucket': 'cco-public', 'source_key': 'cmhp3fipi000j10xfjr8nirz0/81ab354f-d3be-42e8-9801-0d442d756394/0169978b-06b3-4752-8e55-38074f6e61ae/video/hook.mp4', 'prompt_fingerprint': 'f5fc3f59294b985231f33bd01a205ed0', 'target_voice_id': 'itkUuCeluzmxnISkRimf', 'output_bucket': 'cco-public', 'output_key': 'ws_demo_001/video_demo_002/scene_demo_002/video/voice_change_bfe2939f.mp4'}]}


```