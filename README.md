# ADB State Detection Rules for Home Assistant

Extend Home Assistant’s Android TV integration with custom ADB‑based rules to accurately detect **idle**, **playing** and **paused** states for all your favorite apps.

---

## 🔧 Prerequisites

- Home Assistant 2022.5 or newer  
- Android TV integration installed and connected via ADB  
- Your Android TV device reachable from HA (over network or USB)

---

## 🚀 Installation

1. In HA, go to **Settings → Devices & Services → Android TV**.  
2. Click **Configure**, then open the **Custom State Detection** YAML editor.  
3. Paste the snippet below under your device entry:

    ```yaml
    state_detection_rules:
      com.android.vending:
        - idle
      com.spocky.projengmenu:
        - idle

      com.liskovsoft.smarttubetv.beta:
        - playing:
            media_session_state: 3
        - paused:
            media_session_state: 2
        - idle

      com.plexapp.android:
        - playing:
            media_session_state: 3
        - paused:
            media_session_state: 2
        - idle

      com.disney.disneyplus:
        - playing:
            media_session_state: 3
        - paused:
            media_session_state: 2
        - idle

      com.netflix.ninja:
        - playing:
            media_session_state: 3
            wake_lock_size: 3
        - paused:
            media_session_state: 2
            wake_lock_size: 1
        - idle

      nl.uitzendinggemist:
        - playing:
            wake_lock_size: 3
        - paused

      com.amazon.amazonvideo.livingroom:
        - playing:
            media_session_state: 3
            wake_lock_size: 3
        - paused:
            media_session_state: 2
            wake_lock_size: 1
        - idle

      com.spotify.tv.android:
        - playing:
            media_session_state: 3
        - paused

      com.synology.projectkailash:
        - playing:
            wake_lock_size: 3
        - playing:
            wake_lock_size: 2
        - paused:
            wake_lock_size: 1
        - idle

      com.google.android.packageinstaller:
        - idle
      com.android.tv.settings:
        - idle

      com.formulaone.production:
        - playing:
            wake_lock_size: 4
        - playing:
            wake_lock_size: 3
        - playing:
            wake_lock_size: 2
        - paused:
            wake_lock_size: 1
        - idle
    ```

4. **Important:** Netflix often shows 5 – 15 s previews that can fool your detection rules. In the UI, set **Timeout** to **15 seconds** so HA double‑checks before switching to “playing.”  
5. Save and exit; HA will automatically reload the Android TV integration.

---

## 🔍 Determining `wake_lock_size`

The `adb_response` attribute is only populated when you explicitly call the `GET_PROPERTIES` ADB command. Follow these steps to fetch your device’s current `wake_lock_size`:

1. **Invoke `GET_PROPERTIES`**  
   - In HA go to **Developer Tools → Services**.  
   - Select the service **androidtv.adb_command**.  
   - Under **Service Data**, enter:
     ```yaml
     entity_id: media_player.YOUR_DEVICE_ENTITY_ID
     command: "GET_PROPERTIES"
     ```
   - Click **Call Service**.

2. **View the `adb_response` JSON**  
   - Go to **Developer Tools → States**, expand your `media_player.YOUR_DEVICE_ENTITY_ID`, and look under **Attributes → adb_response**.  
   - You’ll see something like:
     ```json
     {
       "screen_on": true,
       "awake": true,
       "wake_lock_size": 3,
       "media_session_state": 3,
       …  
     }
     ```  
     That `"wake_lock_size": <number>` is the value you need.

3. **Extract the value via a template (optional)**  
   - In **Developer Tools → Template**, paste and Render:
     ```jinja
     {{ state_attr('media_player.YOUR_DEVICE_ENTITY_ID', 'adb_response')['wake_lock_size'] }}
     ```
   - This will directly show the integer reported.

4. **Plug it into your `state_detection_rules`**  
   Use that integer under `wake_lock_size` for your playing/paused rules as in the examples above.

---

## 📖 More Info

For full details on custom state detection, see the official docs:  
https://www.home-assistant.io/integrations/androidtv#custom-state-detection  
