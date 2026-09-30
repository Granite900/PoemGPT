# Temperature, Dropout, and the Limits of Machine Creativity

Is turning up sampling temperature, or training with dropout, a "creativity dial"? This repo holds the code, data, and figures for a small study that tests it on a poetry language model.

I trained three small GPT-style models (dropout 0, 0.1, 0.2) from scratch on public-domain poetry, generated endings for three famous poems at three temperatures (0.6, 0.9, 1.2), and scored all 27 endings on the three parts of the classic definition of creativity: **novelty**, **surprise**, and **value**.

**Paper:** 
Medium - https://medium.com/@grantpaxt/temperature-dropout-and-the-limits-of-machine-creativity-ba0a6b2b973c
Substack - https://substack.com/@grant724106/note/p-218104224?utm_source=notes-share-action&r=96gcsu


## Findings in brief

- **Temperature raises novelty and surprise.** The share of four-word runs not found in the training corpus rose from about 87% to about 100%, and surprise (perplexity) rose more than tenfold.
- **It costs value.** Twelve readers were asked to pick the human-written ending, but every ending was machine-generated. The share of picks fell from 64% to 25% to 11% as temperature rose.
- **Dropout made no detectable difference** on any of the three measures.

Neither setting turned out to be a creativity dial.

## How the endings were made

```
seed poem opening (3 poems)
   -> 3 models (dropout 0, 0.1, 0.2)
      -> 3 temperatures each (0.6, 0.9, 1.2)
         -> 1 ending per poem  =  27 endings
```

| Setting | Value |
|---|---|
| Architecture | Decoder-only transformer, 4 layers, 4 heads, 256-dimensional embeddings |
| Parameters | About 5.3 million |
| Context / vocabulary | 256 tokens / 8,000 tokens (byte-level BPE) |
| Training text | About 40 million characters of public-domain poetry from Project Gutenberg, converted to ASCII |
| Random seeds | Data split 1337, generation 20260829 |

## Measures

- **Novelty:** the share of a continuation's four-word runs that never appear in the source text (100% minus overlap).
- **Surprise:** the perplexity the trained models assign to a continuation.
- **Value:** how often readers took an ending for human writing in the survey.

## Running it

The notebook was run on Google Colab with an L4 GPU. Open it and run the sections in order: corpus (§2), tokenizer (§3), model (§4), training (§6-8), then generation and scoring (§9, §9b).

Requirements: Python 3, `torch`, `tokenizers`, `unidecode`.

Checkpoints store a hash of the corpus and split, and the notebook refuses to load one that doesn't match, so rebuilding the corpus means retraining the models.

## Links

Links are found on the paper.

## Data notes

- Project Gutenberg texts are public domain in the US. Check your local rules before redistributing the corpus.
- The survey covers 12 readers and 3 poems, so treat the results as a first look, not a final verdict.

## Citation

Grant LeBron. *Temperature, Dropout, and the Limits of Machine Creativity.* 2026.
