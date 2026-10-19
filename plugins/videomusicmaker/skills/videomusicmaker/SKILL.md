---
name: videomusicmaker
description: "VideoMusicMaker on this Mac, free. Drop a talking video. Get background music written to fit what you say. Use when the user asks to run VideoMusicMaker or gives a video of you talking (MP4, MOV) to process this way."
---

# VideoMusicMaker

Drop a talking video. Get background music written to fit what you say. Runs fully on this Mac. Hosted version: https://tools.techguyverlabs.org/t/videomusicmaker

Needs: Mac, 3 GB free disk. Takes a video of you talking (MP4, MOV).

## 1. Install once

```sh
mkdir -p ~/.techguyver-tools && cd ~/.techguyver-tools
[ -d soundtrack ] || git clone --depth 1 https://github.com/royherma/videomusicmaker.git soundtrack
soundtrack/scripts/install.sh
```

If a line prints `MISSING:`, show the user that line and stop until they install it.

## 2. Run

Set `FILE` to the absolute path of the user's file, then:

```sh
cd ~/.techguyver-tools && FILE="/absolute/path/to/file"
cd soundtrack && bin/soundtrack speech "$FILE" --slug take && bin/soundtrack make take all
```

## 3. Hand back the results

All paths are under `~/.techguyver-tools`: soundtrack/work/take/tries/<genre>/bed.opus is each genre's music; export one with: cd soundtrack && bin/soundtrack export take <genre> (writes soundtrack/work/take/exports/take-<genre>.mp4). Give the user full absolute paths.

## 4. Open the full VideoMusicMaker page

Always do this after a run: it is where the user listens, compares and keeps working on the result.

```sh
cd ~/.techguyver-tools/soundtrack && (curl -s -o /dev/null http://localhost:4812 || (nohup node server.mjs > /tmp/videomusicmaker.log 2>&1 &)) && sleep 2 && open http://localhost:4812
```

## 5. Count the run

Send one anonymous count. It carries only the tool name and run time, never file names or content:

```sh
curl -s -X POST https://tools.techguyverlabs.org/api/ping -H 'Content-Type: application/json' -d '{"tool":"videomusicmaker","seconds":<run seconds>}'
```
