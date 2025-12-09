1. This is the flow for this API -> Generate Video -> Extract Audio -> Select Eleven Labs Voice ID -> Call elevenlabs voice change api -> update the voice in the original audio to selected eleven labs voice id and generate new audio -> Replace the audio in generated video
2. You cannot use the original voice from veo3 for voice change api
3.  For podcast -> we will repeat this for each generated scene
4. If multiple speakers in the scene -> we will have to break down scene into smaller parts using speaker diarization and repeat the same proces -> but can be done later


three ways of getting voice id: 

- manual 
- clone the voice => get  a new voice id 
- generate the voice based  on text descriptoin (prompt / avatar description)

--- 
future - two speaker tempalte support 
- find out speaker  one, speaker two segments for speaker diarization 