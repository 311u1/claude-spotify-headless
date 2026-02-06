---
name: spotify
description: Control headless Spotify playback
user-invocable: true
allowed-tools: Bash, WebSearch
---

# Spotify Control

Headless Spotify via go-librespot + NowPlayingBridge.

**When this skill is invoked, always start your response by printing this ASCII art exactly as shown:**

```
                SPOTIFY JUKEBOX :)

                    @#+++++++++++++++++++++++++++++++++++++%
                    @#%                                  @#%
                    @#%                                  @#@
                 @#--=++++++++++++++++++++++++++++++++++++=--#@
                 #--=%%%%#%%##%##%#%##%%%####%%##%##%%###%%+--#
                 #---%%%%%%%@%%%%%%%%%%%%%%%@%%%%%%@%%%%%%%=--#
                 #-#**%++*+%%-------*+-**-=#-------+%###%%#%%-#
                 #-%+::=+:-:@------+%#-##-#%*------#=:-:::=:+=#
                 %############################################%
       @@@@@@@@@@#---------------%------------#=--------------#@@@@@@@@@@
     @@@@@@@@@@@@#---=##+++##=---@************%=---*#*++*#*---#@@@@@@@@@@@@
    @@@@       @@#--#:::+*+::-%--%-*:-::-:+:*-#=-**::=*+-::#*-#@@       @@@@
    @@@@         #-%::@@@@  %::%-%-*::-=:*-:*-#=*-:#@@@= +::=+#         @@@@
    =..-@@       #=-:+@@@%   =:+*%-*:*:::-:%#-#=@::@@@@:  @::##       @@-..=@
    @......=*@   #-=::@@@@  .-:#+%-#@:::::+=*-#=%::@@@@#  #::##   @*=......@@
   @:#+=%..%.@   #-#:::@@* %:::*-@@#--------#@@=-*::@@% *=::%-#   @.@..%=**:@
   @........:@   #--+@:::::::@+--##############---#-::::::=#--#   @:........@
   @-....*+.+@   #-----+***+-----@:+-++-%-#-%-%=----=***+=----#   @+.+*....:@
    @#.#: :@     %---------------@:+-++:%:*:%:%=--------------#     @- :#.#@
                  @%#%%%%%%%%%%##%%%###%%###%%%##%%%#%%%%%%#%@
                            @@@@                 @@@@
                            @@@@                @@@@@
                            @@@@                @@@@
                             @@@@               @@@@
                             @@@@              @@@@
                         @@@@ @@@@            @@@@  @@@
                        #:---*@ @@@@@       @@@@@@%-::-+@
                      @@=:--:::*+..++#@   %+%..:@-:::::-@@
                      @.*:::::::::::::%@ @=::::::::::::+:*
                       @-:%=........=*%@ @*#........-#=.%@
                         @@##%@@%%@@%@    @@@@%%@@@%#%@
```

Then check status and proceed with the user's request.

## Service Management

```bash
spotify-ctl status  # Check if running
spotify-ctl start   # Start service
spotify-ctl stop    # Stop service
```

**Always check status before playing. Start if not running.**

## Playback (API at localhost:3678)

**Play by URI** (search web for Spotify URI first):
```bash
curl -s -X POST http://localhost:3678/player/play -H 'Content-Type: application/json' -d '{"uri": "spotify:album:ID"}'
```

**Controls**:
```bash
curl -s -X POST http://localhost:3678/player/playpause
curl -s -X POST http://localhost:3678/player/next
curl -s -X POST http://localhost:3678/player/prev
```

**Volume** (0-100):
```bash
curl -s -X POST http://localhost:3678/player/volume -H 'Content-Type: application/json' -d '{"volume": 50}'
```

**Shuffle**:
```bash
curl -s -X POST http://localhost:3678/player/shuffle_context -H 'Content-Type: application/json' -d '{"shuffle_context": true}'
```

**Queue a track**:
```bash
curl -s -X POST http://localhost:3678/player/add_to_queue -H 'Content-Type: application/json' -d '{"uri": "spotify:track:ID"}'
```

## Finding Spotify URIs

1. Web search: "Artist Album spotify"
2. Extract from open.spotify.com URL → `spotify:album:ID` or `spotify:track:ID` or `spotify:playlist:ID`

## Workflow

1. `spotify-ctl status` — check if running
2. `spotify-ctl start` — start if needed
3. Web search for Spotify URI
4. Enable shuffle if playlist
5. Play URI
