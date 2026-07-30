# gbro-collage-broll
[CN](https://github.com/ihsanbagus/gbro-collage-broll/blob/main/README.md) | [ID](https://github.com/ihsanbagus/gbro-collage-broll/blob/main/README_ID.md)

<p align="center">
  <img src="assets/demo-purple.gif" width="180" alt="深紫底：多人协作压出科幻胶片">
  <img src="assets/demo-yellow.gif" width="180" alt="芥末黄底：错误被印刷机批量放大">
  <img src="assets/demo-red.gif" width="180" alt="红底：导演之手摆放棋盘走位">
  <img src="assets/demo-teal.gif" width="180" alt="青绿底：剪刀裁开镜头轨道">
</p>

Turn a ~5s voiceover line into a sharp visual idea, then generate a premium editorial **halftone paper-collage assemble-from-empty animation** as B-roll — powered by Gemini Omni Flash first/last-frame video generation.

## Output Style

- Strong, flat solid-color paper fields + black-and-white halftone photo clippings + colored cardstock accents
- Elements slide in one by one from an empty frame, snap into place, and assemble (stop-motion feel) — not a fade-in or slow zoom
- Delivers by default as 9:16, 5-second, 720×1280, 24fps, silent MP4, ready to drop straight under a voiceover

## Workflow: Three-Gate Approval

The core of this skill isn't a prompt template — it's a mandatory three-stage approval process, so your attention goes toward aesthetic judgment instead of burning generation costs:

1. **Gate 1 · Metaphor Confirmation** — Outputs only the visual metaphor plan (core meaning / key objects / base color / assembly order), with no image or video generated
2. **Gate 2 · Static Frame Confirmation** — Once confirmed, generates the colored collage still frame + contact sheet, then waits for your confirmation again
3. **Gate 3 · Video Generation** — Once the still frame is approved, automatically uses `gemini-omni-flash-preview` to produce the first/last-frame assembly animation, with full QA (per-second frame extraction, empty-frame verification on the first frame, comparison against the last frame)

Batch mode supports partial approval: only confirmed entries move on to the next stage.

## Requirements

On first trigger, the skill automatically runs `scripts/check_setup.sh` for a self-check and provides setup guidance for any missing pieces. Requires:

| Dependency | Notes |
| --- | --- |
| Codex environment | Gate 2 still-frame generation relies on the built-in `image_gen` tool |
| `GEMINI_API_KEY` | Create one at [Google AI Studio](https://aistudio.google.com/apikey); video generation is billed by usage |
| Python >= 3.10 | Used for the video generation scripts |
| `google-genai >= 2.10.0` | The skill will guide you through creating a shared venv at `~/hyperframes-projects/.omni-venv/` |
| ffmpeg / ffprobe | For first/last-frame processing, audio stripping, and contact sheet generation |

The video generation scripts (`scripts/generate_video.py` + `scripts/upload_file.py`) are bundled with the skill — no additional skills need to be installed.

## Installation

Place the entire directory into your agent skills directory (e.g. `~/.agents/skills/` or `~/.claude/skills/`):

```
git clone https://github.com/ihsanbagus/gbro-collage-broll.git ~/.agents/skills/gbro-collage-broll
```

## Usage

Tell your agent:

```
collage b-roll: a lot of people think AI is here to think for you, but it's really more like a mirror — it reflects the gaps in the question you asked.
```

Trigger phrases: `collage b-roll`, `纸拼贴 b-roll`, `半调拼贴`, `拼贴风格配画面`, `gbro-collage-broll`.

Then confirm step by step through Gate 1 → Gate 2 → Gate 3. You can also feed multiple lines at once in batch mode — each line produces one metaphor and one finished clip.

## Directory Structure

```
gbro-collage-broll/
├── SKILL.md                        # Main skill document (three-gate protocol + prompt templates + QA standards)
├── agents/openai.yaml              # Codex interface configuration
├── evals/evals.json                # Behavioral evals for the four gates
└── scripts/
    ├── check_setup.sh              # Environment self-check on first use
    ├── generate_video.py           # Batch video generation with Gemini Omni Flash
    ├── upload_file.py              # Files API upload helper
    └── generate_veo_first_last.py  # Legacy Veo pipeline (kept for compatibility only, not used by default)
```

## FAQ

**Why enforce two rounds of manual confirmation?** A wrong metaphor or static frame that goes straight into video generation wastes real API money. Editing text at Gate 1 is free, and regenerating one image at Gate 2 is far cheaper than re-running an entire video.

**A sliver of paper shows at the edge of the final clip's first frame?** A slight amount is acceptable; if you need a strictly empty opening frame, use an animation tool with an editable timeline to touch up the beginning.

**Can I switch the video model?** By default it's fixed to `gemini-omni-flash-preview`; it only switches when another model is explicitly specified.

## License

MIT License
