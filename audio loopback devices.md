This is actually **easier** on Linux than macOS because the "BlackHole" driver functionality is built directly into the kernel (via PulseAudio or PipeWire). You don't need to install 3rd party drivers.

On Linux, we use `module-null-sink` to create the virtual cable and `module-loopback` to ensure you can still hear the audio yourself.

Here is the bash script. Save it as `stream-audio` and make it executable (`chmod +x stream-audio`).

### The Script

```bash
#!/bin/bash

# The name we will give our virtual device
VIRTUAL_SINK_NAME="StreamMode"
VIRTUAL_SINK_DESC="StreamMode (Audio+Mic)"

# File to store the IDs of the modules we load (so we can remove them later)
STATE_FILE="/tmp/stream_mode_state"

function audio_toggle() {
    if [ "$1" == "on" ]; then
        if [ -f "$STATE_FILE" ]; then
            echo "⚠️  StreamMode seems to be already active."
            exit 1
        fi

        echo "🎙️  Setting up StreamMode..."

        # 1. Identify your real speakers (Current Default)
        REAL_SPEAKERS=$(pactl get-default-sink)
        echo "   -> Routing real audio to: $REAL_SPEAKERS"

        # 2. Create the Virtual Sink (The "BlackHole")
        # We capture the module ID to unload it cleanly later
        SINK_ID=$(pactl load-module module-null-sink \
            sink_name="$VIRTUAL_SINK_NAME" \
            sink_properties=device.description="$VIRTUAL_SINK_DESC")

        # 3. Create the Loopback (So YOU can still hear the audio)
        # This takes audio from StreamMode and sends it to your Real Speakers
        LOOP_ID=$(pactl load-module module-loopback \
            source="$VIRTUAL_SINK_NAME.monitor" \
            sink="$REAL_SPEAKERS" \
            latency_msec=1)

        # 4. Save state
        echo "$SINK_ID" > "$STATE_FILE"
        echo "$LOOP_ID" >> "$STATE_FILE"

        # 5. Set System Audio to output to StreamMode
        pactl set-default-sink "$VIRTUAL_SINK_NAME"

        echo "✅ StreamMode Active!"
        echo "👉 In Zoom/Meet/Discord: Select Input/Mic as 'Monitor of StreamMode'"
        
    elif [ "$1" == "off" ]; then
        if [ ! -f "$STATE_FILE" ]; then
            echo "⚠️  StreamMode is not active."
            exit 1
        fi

        echo "🎧 Tearing down StreamMode..."

        # 1. Read the module IDs from file and unload them
        # We read in reverse order to unload loopback first, then sink
        tac "$STATE_FILE" | while read module_id; do
            pactl unload-module "$module_id" 2>/dev/null
        done

        # 2. Cleanup
        rm "$STATE_FILE"
        
        # PulseAudio automatically reverts to the hardware speakers when the default sink is destroyed.
        echo "✅ Audio restored to normal."

    else
        echo "Usage: stream-audio [on|off]"
    fi
}

audio_toggle "$1"
```

### How to use it

1.  **Start Mode:**
    ```bash
    ./stream-audio on
    ```
2.  **Open your Video Call (Zoom/Meet/Discord):**
    *   **Microphone Input:** Change this to **"Monitor of StreamMode"**.
    *   *Note: In some Linux apps, this might just be called "Monitor of Null Output".*
3.  **Stop Mode:**
    ```bash
    ./stream-audio off
    ```

### Troubleshooting (The `pavucontrol` trick)
If you are on Ubuntu, you **must** install `pavucontrol` (PulseAudio Volume Control). It provides the visual interface for what the script is doing.

```bash
sudo apt install pavucontrol
```

Open `pavucontrol` while the script is running:
1.  Go to the **"Recording"** tab.
2.  You will see your meeting app (e.g., Chrome/Zoom) recording audio.
3.  Ensure its source is set to **"Monitor of StreamMode"**.