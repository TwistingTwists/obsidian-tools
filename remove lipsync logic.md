
lipsync -> template update to use_voice+change true by default => and then do that. 

---

⛬  Current logic (lines 583-629):

   python
     if should_apply_voice_change(config):     # checks useVoiceChange=True + voiceId
         # do voice change (extract audio from video → ElevenLabs STS → replace)
     elif should_apply_lipsync(config):        # checks SPEAKING + voiceId + audioUrl (NOT useVoiceChange)
         # do lipsync (sync video lips to TTS audio)

   Current `should_apply_lipsync`:

   python
     def should_apply_lipsync(config):
         scene_type = config.get("sceneType", "")
         voice_id = config.get("elevenLabsVoiceId")
         audio_url = config.get("audioUrl")
         # Does NOT check useVoiceChange
         return scene_type == "SPEAKING" and voice_id and audio_url

   Issue: Lipsync doesn't explicitly check useVoiceChange=False. It relies on the elif to not run when
   voice change applies.

   Should I update `should_apply_lipsync` to explicitly require `useVoiceChange=False`?

   python
     def should_apply_lipsync(config):
         scene_type = config.get("sceneType", "")
         voice_id = config.get("elevenLabsVoiceId")
         audio_url = config.get("audioUrl")
         use_voice_change = bool(config.get("useVoiceChange"))

         # Lipsync only when NOT using voice change
         if use_voice_change:
             return False
         return scene_type == "SPEAKING" and voice_id and audio_url

   Or do you mean something different - that lipsync should check useVoiceChange=True (meaning the flag
   controls lipsync, not voice extraction)?

