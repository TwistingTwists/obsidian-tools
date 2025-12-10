1. This is the flow for this API -> Generate Video -> Extract Audio -> Select Eleven Labs Voice ID -> Call elevenlabs voice change api -> update the voice in the original audio to selected eleven labs voice id and generate new audio -> Replace the audio in generated video
2. You cannot use the original voice from veo3 for voice change api
3.  For podcast -> we will repeat this for each generated scene
4. If multiple speakers in the scene -> we will have to break down scene into smaller parts using speaker diarization and repeat the same proces -> but can be done later


---

2025-12-09
### ways of getting voice id: 

- manual 
- search api - list voices - 
- clone the voice => get  a new voice id 
- [ ] generate the voice based  on text description (prompt / avatar description)
	- [ ] DOING THIS.


DOING_THIS
Method: veo3 video in -- video out
- { avatar_description -> create a voice, veo3_videos: list_of_s3url } 

[Instant Voice Cloning \| ElevenLabs Documentation](https://elevenlabs.io/docs/creative-platform/voices/voice-cloning/instant-voice-cloning?nis=6&ch=1)
[Design a voice \| ElevenLabs Documentation](https://elevenlabs.io/docs/api-reference/text-to-voice/design)


- elevenlabs voice 
- remove bg 
- add music


[Fetching Title#p60h](https://cco-public.s3.us-west-1.amazonaws.com/cmi2v6i8e015sg9uerkdfdz9u/6de2aec7-859f-4248-a41c-80bb57274e2e/4bdbfd36-dcfd-4284-b086-f6a4d1038fa8/video/hook.mp4)



--- 
future - two speaker tempalte support 
- find out speaker  one, speaker two segments for speaker diarization 