# Aria — AI Music Showcase

A web-based music player for showcasing AI-generated tracks. Each track comes with rich metadata: headline, description, the prompt used, the AI tool, and a personal reflection on the piece.

Built as a single static HTML page — no build step, no framework, no backend. Deploys to Vercel (or any static host) in seconds.

## Features

- Play / pause / next / previous controls
- Click-to-seek progress bar and volume slider
- Live audio visualizer (Web Audio API)
- Playlist with click-to-play and auto-advance
- Editorial-style track detail panel (headline, description, prompt, tool, reflection)
- Custom cover image per track
- Keyboard shortcuts: `Space` = play/pause, `←` / `→` = prev/next
- Responsive (works on mobile)

## Project Structure

```
music-player/
├── index.html          ← The player
├── tracks.json         ← Track metadata (edit this to add tracks)
├── tracks/             ← Drop your audio files here
│   └── too-many-thoughts.mp3
├── covers/             ← Drop your cover images here
│   └── too-many-thoughts.jpg
├── README.md
└── .gitignore
```

## Adding a Track

Three steps:

### 1. Drop the audio file into `tracks/`

Supported formats: MP3, WAV, M4A, OGG (MP3 is safest across browsers).

```
tracks/my-new-song.mp3
```

### 2. Drop the cover image into `covers/`

Recommended: square JPG or PNG, at least 500×500 px.

```
covers/my-new-song.jpg
```

The cover is optional — if you skip it, a gradient placeholder is shown.

### 3. Add an entry to `tracks.json`

Open `tracks.json` and add a new object to the array:

```json
[
  {
    "audio": "tracks/my-new-song.mp3",
    "cover": "covers/my-new-song.jpg",
    "headline": "My New Song",
    "description": "A short paragraph describing the piece, the mood, the intent.",
    "prompt": "the exact prompt you fed to the AI tool",
    "tool": "Suno v4",
    "reflection": "Your thoughts on the process, what you learned, what worked, what didn't."
  }
]
```

### Field reference

| Field         | Required | What it is                                          |
|---------------|----------|-----------------------------------------------------|
| `audio`       | yes      | Path to the audio file, relative to the project root |
| `cover`       | no       | Path to the cover image. Falls back to gradient placeholder |
| `headline`    | yes      | The track title (shown big and italic)              |
| `description` | yes      | Short prose intro (1–3 sentences)                   |
| `prompt`      | yes      | The literal AI prompt used                          |
| `tool`        | yes      | Which tool generated it (e.g., `Suno v4`, `Udio`, `Stable Audio`) |
| `reflection`  | yes      | Your reflection / what you learned                  |

> **Watch out for JSON commas.** Every item except the last needs a trailing comma. If the page shows "Couldn't load tracks.json", you probably have a JSON syntax error — paste your file into [jsonlint.com](https://jsonlint.com) to check.

## Running Locally

You can't just open `index.html` directly in the browser — it uses `fetch()` to load `tracks.json`, which is blocked on `file://` URLs.

Use a tiny local server instead:

```bash
# If you have Python:
python3 -m http.server 8000

# Or with Node:
npx serve

# Or with PHP:
php -S localhost:8000
```

Then open `http://localhost:8000`.

## Deploying

### Push to GitHub

```bash
# Inside the project folder
git init
git add .
git commit -m "Initial commit"

# Create a new repo on github.com first, then:
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/music-player.git
git push -u origin main
```

### Deploy to Vercel

1. Go to [vercel.com](https://vercel.com) and sign in with GitHub.
2. Click **Add New → Project**.
3. Pick your `music-player` repo and click **Import**.
4. Leave all settings at default (it's a static site — no framework needed).
5. Click **Deploy**.

In ~30 seconds you'll get a live URL like `music-player-xyz.vercel.app`. Every `git push` to `main` automatically redeploys.

### Updating after deployment

Add tracks locally, commit, and push:

```bash
git add tracks/ covers/ tracks.json
git commit -m "Add new track: My New Song"
git push
```

Vercel rebuilds and your live site updates within a minute.

## Keyboard Shortcuts

| Key       | Action       |
|-----------|--------------|
| `Space`   | Play / pause |
| `→`       | Next track   |
| `←`       | Previous track (or restart current track if past 3s) |

## Troubleshooting

**"Couldn't load tracks.json"** — Either you're opening the file directly without a local server (see "Running Locally"), or your JSON has a syntax error.

**"Can't play [track] — format not supported"** — Your audio file isn't in a format the browser supports. Convert to MP3.

**Track loads but no audio plays** — Most browsers block audio until the user clicks something. Click the play button (don't expect autoplay on first load).

**Cover image doesn't show** — Check the path in `tracks.json` matches the actual filename (case-sensitive!) and that the file is in `covers/`.

## License

Use this however you want for your assignment, portfolio, or personal projects.
