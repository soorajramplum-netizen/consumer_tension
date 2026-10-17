# consumer_tension 💬🔍

**The video is the pitch. The comments are the truth.** A free, local tool that collects YouTube comments, on their own or paired with the video's transcript, so you can find what consumers actually think and understand *why* they think it.

<!-- Add a screenshot: save it as docs/screenshot.png in this repository, then delete this line and the one below.
![Screenshot of consumer_tension](docs/screenshot.png) -->

---

## Why I built this

I've always believed the most honest consumer research on the internet is sitting in plain sight: the YouTube comment section.

Nobody in a comment section is being polite to a moderator or picking from a list of survey answers. People say exactly what's on their mind: what frustrates them, what they don't trust, what they wish existed, what made them laugh, and what made them switch brands. That unprompted friction between what people want and what they're getting is **consumer tension**, and it's the raw material of every good insight.

But I kept running into the same problem. **Comments read on their own are misleading.** A wave of anger, a running joke, or a cluster of the same question only makes sense once you know what was said in the video that triggered it. Read in isolation, you see *what* people feel but miss *why*, and the why is the insight.

So I built **consumer_tension**. It does two things:

- **Pairs comments with the transcript.** For each video, you get one file: everything that was said, followed by every comment and reply. Reading them together connects each reaction to its cause.
- **Collects comments at scale.** It pulls every comment from as many videos as you like into a single CSV, so you can spot patterns across a whole brand, category, or competitor set.

---

## Two ways to use it

| Mode | Use it when you want to… | Output |
| --- | --- | --- |
| **Transcripts + comments** | Understand *why* an audience reacted the way it did to a specific video | One Markdown file per video: the full transcript, then every comment with replies nested underneath |
| **Comments only** | Find patterns across many videos: a category, a set of competitors, a product launch | One CSV with every comment from every video |

The app also includes a plain **Transcripts** mode, from my sister project [lazy_podcast_listener](https://github.com/YOUR-USERNAME/lazy_podcast_listener), for when you only need what was said.

### Where it's useful

- **Brand and category research:** what do people love, distrust, or complain about across reviews in a category?
- **Competitor analysis:** how do audiences react to competitors' launches, ads, and creators?
- **Product feedback:** which features, prices, or claims trigger the strongest reactions?
- **Content and creative testing:** which moments in a video spark agreement, pushback, or questions?
- **Influencer and creator research:** how does an audience actually respond when a creator talks about a brand?

---

## What it does

- **Bulk input.** Paste links or upload a CSV of videos. Links can be in any column; standard links, `youtu.be` links, Shorts, live streams, and bare video IDs all work, and duplicates are removed.
- **Checks before collecting.** A review step shows what's available upfront, e.g. *"24 of 25 videos can be checked for comments, about 48,000 in total,"* so you know the size of the job before it starts.
- **Every comment, or just the top ones.** Collect all comments, or the top 100 to 5,000. Sort by top or newest. Include or skip replies.
- **Context labels.** Pinned comments, comments by the creator, and comments hearted by the creator are marked, since these often signal what the brand or creator wants to amplify.
- **Spreadsheet-ready CSV.** Opens directly in Excel or Google Sheets, with emoji and non-English text (Tamil, Hindi, and more) intact. Cells are protected from being misread as formulas.
- **Live progress.** Large comment sections show progress as they're collected, e.g. *"Collecting comments… 1,240 of about 5,300."*
- **Transcripts in any language.** Choose preferred caption languages, fall back to whatever a video has, or machine-translate. Optional [Whisper](https://github.com/SYSTRAN/faster-whisper) transcribes videos that have no captions, locally.
- **Private and free.** Runs entirely on your own computer. No accounts, API keys, or subscriptions.

---

## Getting started

### Windows

1. Install [Python](https://www.python.org/downloads/) 3.9 or newer. On the installer's first screen, tick **"Add python.exe to PATH."**
2. Download this repository: click the green **Code** button, then **Download ZIP**.
3. Right-click the ZIP, choose **Extract All**, and open the extracted folder.
4. Double-click **`start.bat`**. If Windows shows a security prompt, click **More info**, then **Run anyway**.

The first launch installs what it needs, which takes a minute or two, and then the app opens in your browser. After that, double-click `start.bat` any time and it starts in seconds. Keep the black window open while you use the app; close it when you're done.

### Mac / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python app.py
```

The app opens at `http://127.0.0.1:8765`.

### Optional: Whisper for videos without captions

Only needed in **Transcripts + comments** mode, for videos that have no captions.

- **Windows:** double-click `install-whisper.bat`, then restart the app with `start.bat`.
- **Mac / Linux:** run `pip install -r requirements-whisper.txt` and install [Deno](https://deno.com/) (`brew install deno`).

---

## How to use it

1. Choose **Transcripts + comments** or **Comments only**.
2. **Add videos:** upload a CSV or paste YouTube links. A CSV is checked automatically.
3. **Review** what's available, and set how many comments to collect, the sort order, and whether to include replies.
4. Click **Generate** (or **Collect comments**), then **Download ZIP** or **Download CSV**.

Popular videos can have tens of thousands of comments. Collecting all of them can take several minutes per video, so for a first pass across many videos, "Top 500" is often enough.

---

## Output formats

### Transcripts + comments (Markdown, one file per video)

```markdown
# We Tested Every Budget Smartphone

- **Channel:** Example Tech Reviews
- **Video:** https://www.youtube.com/watch?v=VIDEO_ID
- **Transcript:** English (auto-generated)
- **Retrieved:** 2026-10-17

---

## Transcript

[00:00](https://www.youtube.com/watch?v=VIDEO_ID&t=0s) Today we're testing six phones under 15,000 rupees...

[04:12](https://www.youtube.com/watch?v=VIDEO_ID&t=252s) The camera is great in daylight, but at night...

---

## Comments

1,204 comments and 312 replies, top comments first.

- **@buyer_one** (2,310 likes, 3 weeks ago): Bought this after your last video. Night camera is exactly as bad as you said.
  - **@buyer_two** (41 likes, 2 weeks ago): Same. Why does no brand fix low light at this price?
- **@channel_name** (creator, pinned, 980 likes, 3 weeks ago): Prices and links are in the description.
```

### Comments only (CSV, all videos in one file)

| Column | Description |
| --- | --- |
| `video_title`, `video_url`, `video_id` | Which video the comment belongs to |
| `comment_number` | `1`, `2`, `3`… for comments; `3.1`, `3.2`… for replies to comment 3 |
| `type` | `comment` or `reply` |
| `reply_to` | The author being replied to |
| `author` | Commenter's handle |
| `comment` | Full comment text |
| `likes` | Like count, a rough measure of how many people agree |
| `posted` | Relative time as shown by YouTube, e.g. "2 years ago" |
| `pinned`, `by_creator`, `hearted_by_creator` | `yes` / `no` |
| `comment_id` | YouTube's comment ID |

---

## From comments to insight

These are the prompts I use with an AI assistant. Upload the Markdown file or the CSV along with the prompt.

### Find the tension in one video (Transcripts + comments)

> Read the transcript first, then the comments. Identify the five strongest consumer tensions in the audience's reaction: places where what people want conflicts with what they're getting. For each tension:
> 1. Name it in one line.
> 2. Point to the moment in the transcript that triggered it, with the timestamp.
> 3. Quote two or three representative comments, with their like counts.
> 4. Describe the underlying need, fear, or frustration.

### Map a whole category (Comments only)

> This CSV contains YouTube comments from several videos in the same category. Group the comments into themes. For each theme, give its approximate share of comments, total likes as a measure of agreement, three representative quotes, and the consumer tension behind it. Then list the unmet needs that no brand seems to be addressing.

### Compare competitors (Comments only)

> Each video in this CSV is about a different brand. Compare how audiences talk about each brand: what they praise, what they distrust, and what makes them consider switching. Present it as a table, then summarise the biggest opportunity for a new entrant.

### Reading the signals

- **Likes are votes.** A comment with 2,000 likes speaks for far more people than one with 2. Sort or weight by likes.
- **Replies are where tension lives.** Arguments in reply threads often reveal the real trade-offs people are weighing.
- **Pinned and hearted comments show the creator's agenda,** not necessarily the audience's.
- **Repeated questions are unmet information needs:** what the video, and often the brand, failed to explain.

---

## A note on what comments can and can't tell you

Comments are rich, honest, and unprompted, but they aren't a representative sample. They over-represent people who feel strongly, include some spam and bots, and reflect the audience of a specific channel. Treat them as a source of hypotheses and language, and validate important decisions with other research.

---

## Troubleshooting

- **"YouTube is temporarily limiting requests":** too many requests in a short time. Wait 5 to 15 minutes, then click **Retry failed** or **Check failed again**. The app already retries automatically.
- **Comment counts don't match YouTube:** the number YouTube displays usually includes comments that were removed or held for review, so collected totals are often a little lower.
- **Everything suddenly fails:** YouTube may have changed something. Delete `.venv\installed-v2.txt` and run `start.bat` again to update the libraries. If that doesn't help, delete the whole `.venv` folder for a fresh setup.
- **"Port 8765 is busy":** the app is already running in another window. Close it first.

---

## Project structure

```
start.bat                  Windows: double-click to set up and launch
install-whisper.bat        Windows: optional Whisper setup
app.py                     Local web server
core.py                    Checks and fetches YouTube captions
comments_engine.py         Checks and collects comments and replies
whisper_engine.py          Optional local transcription
public/index.html          The interface
public/sample.csv          Example input
requirements.txt           Required packages
requirements-whisper.txt   Optional Whisper packages
```

## Built with

- [yt-dlp](https://github.com/yt-dlp/yt-dlp): collecting comments and downloading audio
- [youtube-transcript-api](https://github.com/jdepoix/youtube-transcript-api): fetching captions
- [faster-whisper](https://github.com/SYSTRAN/faster-whisper): optional offline transcription
- [PapaParse](https://www.papaparse.com/) and [JSZip](https://stuk.github.io/jszip/): CSV reading and ZIP creation in the browser

## Limitations

- Comment timestamps are approximate ("2 years ago"), because that's all YouTube provides.
- Videos with comments turned off, and private, members-only, or age-restricted videos, can't be collected.
- Transcripts need captions (manual or auto-generated), unless Whisper is installed.
- The tool relies on YouTube's public pages, which change occasionally. Updating the libraries usually fixes things.

## Responsible use

Comments are written by real people, and the collected data includes their usernames. Use it for research, anonymise or aggregate it before sharing findings publicly, and don't use it to target or contact individuals. Please respect [YouTube's Terms of Service](https://www.youtube.com/t/terms).

## License

Released under the [MIT License](LICENSE).
