---
name: song-exploder
description: "Song Exploder on this Mac, free. Drop a song. Get every stem, the drum parts, tempo, key, chords and a MIDI file. Use when the user asks to run Song Exploder or gives a song (MP3, WAV, M4A, FLAC) to process this way."
---

# Song Exploder

Drop a song. Get every stem, the drum parts, tempo, key, chords and a MIDI file. Runs fully on this Mac. Hosted version: https://tools.techguyverlabs.org/t/song-exploder

Needs: Mac with Apple silicon, 6 GB free disk. Takes a song (MP3, WAV, M4A, FLAC).

## 1. Install once

```sh
mkdir -p ~/.techguyver-tools && cd ~/.techguyver-tools
[ -d song-reverse ] || git clone --depth 1 https://github.com/royherma/song-exploder.git song-reverse
song-reverse/scripts/install.sh
```

If a line prints `MISSING:`, show the user that line and stop until they install it.

## 2. Run

Set `FILE` to the absolute path of the user's file, then:

```sh
cd ~/.techguyver-tools && FILE="/absolute/path/to/file"
cd song-reverse && .venv/bin/python bin/reverse.py "$FILE"
```

## 3. Hand back the results

All paths are under `~/.techguyver-tools`: song-reverse/songs/<song-name>/web/ has every stem and drum part as MP3; the MIDI is song-reverse/songs/<song-name>/runs/<newest>/song.mid; tempo, key and chords are in song-reverse/songs/<song-name>/song.json. Give the user full absolute paths.

## 4. Open the full Song Exploder page

Always do this after a run: it is where the user listens, compares and keeps working on the result.

```sh
cd ~/.techguyver-tools/song-reverse && (curl -s -o /dev/null http://localhost:4843 || (nohup node server.mjs > /tmp/song-exploder.log 2>&1 &)) && sleep 2 && open http://localhost:4843
```

## 5. Count the run

Send one anonymous count. It carries only the tool name and run time, never file names or content:

```sh
curl -s -X POST https://tools.techguyverlabs.org/api/ping -H 'Content-Type: application/json' -d '{"tool":"song-exploder","seconds":<run seconds>}'
```
