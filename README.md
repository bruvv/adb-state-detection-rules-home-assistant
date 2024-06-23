# adb-state-detection-rules-home-assistant
see: https://www.home-assistant.io/integrations/androidtv#custom-state-detection

Because netflix has "previews" make sure to have a 15 second time-out to double check that the movie is playing or the preview is playing.

```json
"state_detection_rules": {
  "com.android.vending": [
    "idle"
  ],
  "com.spocky.projengmenu": [
    "idle"
  ],
  "com.liskovsoft.smarttubetv.beta": [
    {
      "playing": {
        "media_session_state": 3
      }
    },
    {
      "paused": {
        "media_session_state": 2
      }
    },
    "idle"
  ],
  "com.plexapp.android": [
    {
      "playing": {
        "media_session_state": 3
      }
    },
    {
      "paused": {
        "media_session_state": 2
      }
    },
    "idle"
  ],
  "com.disney.disneyplus": [
    {
      "playing": {
        "media_session_state": 3
      }
    },
    {
      "paused": {
        "media_session_state": 2
      }
    },
    "idle"
  ],
  "com.netflix.ninja": [
    {
      "playing": {
        "media_session_state": 3,
        "wake_lock_size": 3
      }
    },
    {
      "paused": {
        "media_session_state": 2,
        "wake_lock_size": 1
      }
    },
    "idle"
  ],
  "nl.uitzendinggemist": [
    {
      "playing": {
        "wake_lock_size": 3
      }
    },
    "paused"
  ],
  "com.amazon.amazonvideo.livingroom": [
    {
      "playing": {
        "media_session_state": 3,
        "wake_lock_size": 3
      }
    },
    {
      "paused": {
        "media_session_state": 2,
        "wake_lock_size": 1
      }
    },
    "idle"
  ],
  "com.spotify.tv.android": [
    {
      "playing": {
        "media_session_state": 3
      }
    },
    "paused"
  ],
  "com.synology.projectkailash": [
    {
      "playing": {
        "wake_lock_size": 3
      }
    },
    {
      "playing": {
        "wake_lock_size": 2
      }
    },
    {
      "paused": {
        "wake_lock_size": 1
      }
    },
    "idle"
  ],
  "com.google.android.packageinstaller": [
    "idle"
  ],
  "com.android.tv.settings": [
    "idle"
  ],
  "com.formulaone.production": [
    {
      "playing": {
        "wake_lock_size": 4
      }
    },
    {
      "playing": {
        "wake_lock_size": 3
      }
    },
    {
      "playing": {
        "wake_lock_size": 2
      }
    },
    {
      "paused": {
        "wake_lock_size": 1
      }
    },
    "idle"
  ]
}
```
