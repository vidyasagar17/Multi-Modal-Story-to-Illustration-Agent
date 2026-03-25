# Multi-Modal Story-to-Illustration Agent

An AI agent pipeline that converts plain text stories into illustrated visual narratives. It reads a story, splits it into scenes, condenses each scene into concise narration, and generates a matching illustration for every page.

## How It Works

The pipeline has three stages per page:

1. **Text Condensation** — Takes a raw story segment and distills it into 4 clear narrative sentences using GPT-4o-mini, preserving key plot beats and character actions.

2. **Image Prompt Generation (Verbal Sampling)** — Generates 3 candidate image prompts, each with a different creative focus (character emotion, environment/lighting, composition/framing). A fourth LLM call merges the strongest visual details from all candidates into one final prompt. This produces richer, more specific prompts than a single-pass approach.

3. **Image Generation** — Sends the merged prompt to DALL-E 2 to produce a 512×512 illustration for the scene.

An orchestrating `CodeAgent` (from [smolagents](https://github.com/huggingface/smolagents)) manages the full loop: splitting the story, calling each tool in sequence, and assembling the final markdown output with text and images.

## Project Structure

```
├── image_generation_cleaned.ipynb   # Main notebook
├── alice_in_wonderland.txt          # Input story (bring your own)
├── generated_images/                # Output images (created at runtime)
├── image_story_output.md            # Output markdown (created at runtime)
├── requirements.txt                 # Python dependencies
└── README.md
```

## Setup

```bash
pip install smolagents litellm openai requests
```

Set your API key in the notebook or as an environment variable:

```bash
export OPENAI_API_KEY="sk-..."
```

Then run the notebook cells in order.

## Requirements

- Python 3.9+
- An OpenAI API key (GPT-4o-mini + DALL-E 2)

## What is Verbal Sampling?

Instead of generating a single image prompt and hoping it's good enough, verbal sampling generates multiple candidates with deliberately different creative lenses, then uses an LLM to select and merge the best concrete visual details from each. This exploits the variance across samples to produce a final prompt that is more vivid and specific than any single generation.
