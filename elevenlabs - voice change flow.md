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



---

A slow, subtle push-in shot begins on Alex, a 45-year-old Caucasian man with salt-and-pepper hair, wearing a grey button-down shirt with rolled-up sleeves. He stands alone on a minimalist balcony with a glass railing, overlooking a vast city skyline during the golden hour. The setting sun paints the sky in hues of orange and purple, and city lights begin to glitter below. The atmosphere is calm and contemplative, with the gentle, ambient hum of the distant city being the only sound. Alex leans against the railing, his expression weary and thoughtful as he gazes outward. His internal monologue, spoken in a soft, calm, mid-pitch male voice, reflects his day. Alex says: \"The emails, the deadlines, the endless noise. Some days just feel like a constant rush from one thing to another.

[Fetching Title#ljtx](https://cco-public.s3.amazonaws.com/cmi2v6i8e015sg9uerkdfdz9u/6de2aec7-859f-4248-a41c-80bb57274e2e/7a78d4d9-7de9-4b84-9e95-4363df22f16e/video/hook.mp4)

---

The shot continues its slow push-in as Alex turns from the railing towards a small table where a glass bottle of Coca-Cola sits. He picks it up, opens it with a satisfying pop and fizz, and pours the sparkling beverage into a clear glass. The camera follows the motion, focusing on the bubbles and the rich color of the drink. He lifts the glass, and the shot transitions to a close-up on his face as he takes a long, deliberate sip. His eyes close for a moment, and the tension in his face melts away, replaced by a look of pure, unadulterated satisfaction. A small, genuine smile graces his lips. The ambient city hum continues softly in the background. His internal monologue, in the same soft, mid-pitch male voice, conveys his relief. Alex says: \"But then there's this. That first sip of Coca-Cola. The perfect pause. Suddenly, everything just feels right again.

[Fetching Title#gqe4](https://cco-public.s3.us-west-1.amazonaws.com/cmi2v6i8e015sg9uerkdfdz9u/6de2aec7-859f-4248-a41c-80bb57274e2e/4bdbfd36-dcfd-4284-b086-f6a4d1038fa8/video/hook.mp4)




--- 
future - two speaker tempalte support 
- find out speaker  one, speaker two segments for speaker diarization 