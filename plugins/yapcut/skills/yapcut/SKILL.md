---
name: yapcut
description: "YapCut on this Mac, free. Drop a talking-head take. Get an edited vertical Reel: cuts, hook title, zooms, captions, sounds and effects. Use when the user asks to run YapCut or gives a video of you talking (MP4, MOV) to process this way."
---

# YapCut

Drop a talking-head take. Get an edited vertical Reel: cuts, hook title, zooms, captions, sounds and effects. Runs fully on this Mac. Hosted version: https://tools.techguyverlabs.org/t/yapcut

Needs: Mac with Apple silicon, Google Chrome, Xcode command line tools, Claude Code or Codex, 6 GB free disk. Takes a video of you talking (MP4, MOV).

## 1. Install or update

```sh
mkdir -p ~/.techguyver-tools && cd ~/.techguyver-tools
if [ -d yapcut ]; then git -C yapcut pull --ff-only -q || echo "Could not update yapcut; using the copy already here"; else git clone --depth 1 https://github.com/royherma/yapcut.git yapcut; fi
yapcut/scripts/install.sh
```

If a line prints `MISSING:`, show the user that line and stop until they install it.

## 2. Run

Set `FILE` to the absolute path of the user's file, then:

```sh
cd ~/.techguyver-tools && FILE="/absolute/path/to/file"
cd yapcut && mkdir -p inbox && cp "$FILE" inbox/
```

## 3. Hand back the results

All paths are under `~/.techguyver-tools`: the take is in Yap Studio (step 4), which picks it up from yapcut/inbox/ and builds 12 versions live; Export there writes the MP4 into yapcut/exports/. For one quick edit without the page: cd yapcut && node bin/yapcut.mjs new "$FILE" --name take (writes yapcut/runs/take/out.mp4). Give the user full absolute paths.

## 4. Open the full YapCut page

Always do this after a run: it is where the user listens, compares and keeps working on the result.

```sh
cd ~/.techguyver-tools/yapcut/preview && (curl -s -o /dev/null http://localhost:4901 || (nohup node server.mjs > /tmp/yapcut.log 2>&1 &)) && sleep 2 && open http://localhost:4901
```

## 5. Count the run

Send one anonymous count. It carries only the tool name and run time, never file names or content:

```sh
curl -s -X POST https://tools.techguyverlabs.org/api/ping -H 'Content-Type: application/json' -d '{"tool":"yapcut","seconds":<run seconds>}'
```
