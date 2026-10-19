---
name: song-dna
description: "Song DNA on this Mac, free. Drop a MIDI. Get its DNA and new variations that sound like the same song. Use when the user asks to run Song DNA or gives a MIDI file to process this way."
---

# Song DNA

Drop a MIDI. Get its DNA and new variations that sound like the same song. Runs fully on this Mac. Hosted version: https://tools.techguyverlabs.org/t/song-dna

Needs: Mac with Apple silicon, 6 GB free disk (it shares Song Exploder's setup). Takes a MIDI file.

## 1. Install once

```sh
mkdir -p ~/.techguyver-tools && cd ~/.techguyver-tools
[ -d song-reverse ] || git clone --depth 1 https://github.com/royherma/song-exploder.git song-reverse
[ -d midi-dna ] || git clone --depth 1 https://github.com/royherma/song-dna.git midi-dna
song-reverse/scripts/install.sh
curl -sL -o GeneralUser-GS.sf2 https://raw.githubusercontent.com/mrbumpy409/GeneralUser-GS/97049183643d5fc5a9322a69c5b09efb667c6c3a/GeneralUser-GS.sf2
```

If a line prints `MISSING:`, show the user that line and stop until they install it.

## 2. Run

Set `FILE` to the absolute path of the user's file, then:

```sh
cd ~/.techguyver-tools && FILE="/absolute/path/to/file"
cd midi-dna && MIDI_DNA_SF2=../GeneralUser-GS.sf2 ../song-reverse/.venv/bin/python bin/mididna.py new "$FILE" --count 8
```

## 3. Hand back the results

All paths are under `~/.techguyver-tools`: midi-dna/work/<midi-name>/v/ has each variation as MP3 and MIDI; the DNA is midi-dna/work/<midi-name>/dna.json and the scores are in variations.json. Give the user full absolute paths.

## 4. Open the full Song DNA page

Always do this after a run: it is where the user listens, compares and keeps working on the result.

```sh
cd ~/.techguyver-tools/midi-dna && (curl -s -o /dev/null http://localhost:4912 || (nohup node server.mjs > /tmp/song-dna.log 2>&1 &)) && sleep 2 && open http://localhost:4912
```

## 5. Count the run

Send one anonymous count. It carries only the tool name and run time, never file names or content:

```sh
curl -s -X POST https://tools.techguyverlabs.org/api/ping -H 'Content-Type: application/json' -d '{"tool":"song-dna","seconds":<run seconds>}'
```
