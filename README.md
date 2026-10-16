# SFX — real recordings only

The `AudioEngine` ships **silent by design**. Every slot stays a no-op until
`loadPack({ name: url })` is called with a real recording. There are no
synthesized fallbacks anywhere in the client — silence beats an unapproved
synth, per the project quality bar.

## Slots

| Slot          | When it plays                                  |
| ------------- | ---------------------------------------------- |
| `gloveImpact` | A strike lands (jab / cross / hook / kick)     |
| `swoosh`      | A dodge, miss, or whiffed swing                |
| `crowd`       | Round start, big momentum swings, knockbacks   |
| `bell`        | Round open and round close                     |
| `victory`     | Your side wins a round you entered             |
| `loss`        | Your side loses a round you entered            |
| `ambience`    | Looped arena crowd bed while a fight is live   |

## How to add real recordings

1. Record (or license) short clips, one per slot. MP3 or WAV, 44.1kHz.
2. Drop them in this `sfx/` folder, e.g. `sfx/glove-impact-1.mp3`.
   (`sfx/` is Vite's public dir — its contents are copied to `dist/` on build.)
3. Describe them in `sfx/manifest.json`:

```json
{
  "pack": "arena-pack-1",
  "files": {
    "gloveImpact": "glove-impact-1.mp3",
    "swoosh": "swoosh-1.mp3",
    "crowd": "crowd-cheer-1.mp3",
    "bell": "round-bell-1.mp3",
    "victory": "victory-1.mp3",
    "loss": "loss-1.mp3",
    "ambience": "arena-bed-1.mp3"
  }
}
```

4. Rebuild. On boot the client fetches `manifest.json` and arms each slot:

```ts
await audio.loadManifest(manifest); // same call the client makes on boot
```

The engine loads each URL, decodes it, and arms the slot. A missing or
undecodable file keeps that slot silent — playback is a guarded no-op, never
an exception, never a synth tone.

## Quality bar

- Hits must sound like real glove-on-body contact: layered crack, thump, and
  body. Never thin, hollow, or chime-like.
- Crowd must be real crowd recordings, never synthesized cheers.
- The user can mute everything from the header sound toggle (persisted).
