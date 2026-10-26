---
layout: "post"
title: "Score the Part of the Page You Cannot Afford to Lose"
date: "2026-10-26"
tags: ["OCR", "Local models", "Benchmarks"]
redirect_from: ["/software/archive/score-the-part-of-the-page-you-cannot-lose/"]
---

[Home](https://neutze.de/) › [Software](../README.md) › [Reading my documents](README.md)

Tags: [OCR](../../tags/ocr/README.md) · [Local models](../../tags/local-models/README.md) · [Benchmarks](../../tags/benchmarks/README.md)

I keep my paperwork in a document archive at home, and the archive needs something that reads
scanned pages. For a while that reader was a small vision model with a long, careful prompt. It
read the letters well. It also left out everything above the subject line: the letterhead, the
sender, the date, the reference number. Those are the parts I search by.

No leaderboard would have warned me. They are led by small models like that one, and they do not
score the top and the bottom of a page.

This article is about the tool I built to choose a better reader, and why I am publishing it. It is
called **ocr-bakeoff**, it is open source, and it is at
[github.com/neutze/ocr-bakeoff](https://github.com/neutze/ocr-bakeoff). What it found is the
series *Reading My Own Archive*, coming to this section part by part. This piece is about the
instrument.

## The question a leaderboard cannot answer

The obvious move was to pick the next model on the leaderboard. I decided against it, since a
leaderboard answers a different question from mine.

Public document-reading benchmarks are built from books, papers and reports. In those, the running
header and the page footer are noise, and several of the leading models are trained, or prompted by
their makers, to leave them out. For a personal archive they are the most important text on the
page. In other words, a model can top a leaderboard because it is good at dropping what I need.

There is a second gap. A published score belongs to a model running inside its maker's pipeline, with
its own layout stage, page size, prompt and sampling. I serve the weights locally with `llama.cpp`.
That is a different system, and nobody has scored it.

So the call was to measure the thing my archive needs, on pages like mine, on the machine that would
run it. The headline number in ocr-bakeoff is **drops**: on how many pages does a reader lose most of the
words in the top or bottom of the page? The character error rate is still measured, but it ranks
last. Next to it sit the
things that break an archive quietly: numbers read wrong or invented, German and Turkish letters
lost, and output that loops, echoes the prompt or stops at the length limit.

## What it does

You point it at a folder of documents and it gives you a table.

```bash
uv run ocr-bakeoff ./testfiles
```

It sends the same page images to eleven vision models and five classic OCR engines, scores every
answer, and picks a winner by a rule written down in advance: disqualify an engine that degenerates or
drops a header or footer too often, rank the rest by drops, then numbers, then error rate. Engines
within two pages of the leader on every count tie with it, and a tie is reported as a group with its
fastest member, never as a single winner. I wrote that rule before the first run, because a rule
chosen after seeing the table is a rule chosen to crown a favorite.

Born-digital PDFs need no answer key: their own text layer is the reference, and each page is scored
twice, as rendered and as a simulated scan. Real scans have no text layer, so the tool can build a
reference from what several independent engines agree on, scoring each engine only against the
others.

Everything runs on the machine. Nothing leaves it unless you opt into using Google Drive's own OCR as
a second reference.

## What the benchmark got wrong first

The tool is worth publishing mostly because of what it got wrong, and what I added so that it would
not get it wrong again. Each of these becomes a part of the series; here they are as one line each.

- **The answer key held text nobody can see.** On the first run, every engine failed the same nine
  pages. The fault was in the key: it held microprint, letter-spaced headings stored as
  single letters, and text hidden under pictures. The tool now keeps only visible text in a
  born-digital reference.
- **The model in last place had been asked wrong.** One model moved from last to the leading group when
  its reasoning mode was switched off; a single output limit, inherited from my own pipeline, had cost
  every leading model several points.
- **A suspicion became a test.** A new model looped until its image came before the instruction. Rather
  than argue whether that was unfair to the others, I added twelve one-change variants from the makers'
  own cards. The ranking held.
- **The page size on the model card is a speed setting.** Two leading models read a page of about two
  megapixels as well as the full 300 dpi page, in about half the time.

Every one of these is now a lane, a rule or a check in the tool, with the reason next to it.

## Why it is public, when the documents never can be

Nobody else can run a benchmark on my archive. My documents
are tax letters, contracts and medical bills. They are never leaving the house, and I would not want
yours either.

So the tool brings the benchmark to the documents instead. `profile` reads a folder locally and
records only the *shape* of each PDF: paper format, scan or born-digital, header and footer as text or
as an image, columns, table rules, language. It prints counts, never a name or a word. `synth` then
writes look-alike documents with those shapes and invented content in German, English and Turkish,
each with an exact answer key, because the generator knows every word it drew. You can run the whole
comparison on look-alikes without opening a single real document, and confirm the leaders on a
handful of real ones afterwards with `pick`.

The alternative I did not take was to publish my results and let them stand as a recommendation. They
would have been a leaderboard of one archive, which is the thing this whole exercise argues against.
The useful thing to share is the question, asked in a way you can point at your own pages.

## What it costs

A full run of every engine over a few dozen documents takes hours on a laptop. The Mac with Apple
Silicon is the tested path; Linux works for everything except the Apple Vision engines. The consensus
reference for scans is honest about itself: on pages that do have a true key, it gives the same drop
verdict on 91 to 97 percent of engine-page pairs, but it flatters strong engines, because what a good
engine misses the others often miss too. It separates good readers from bad ones, not the leaders from
one another.

The look-alikes are cleaner than real paper. Real letterheads and real scanners are harder. Treat a
look-alike result as the first cut.

And one model is not in it any more. HunyuanOCR was a lane until I checked its license for this
release: it does not apply in the European Union, where I live. A tool that offers a model to everyone
has to be one everyone may run.

## What I would tell another team

The design my archive runs now came out of this benchmark: the PDF's own text where it can be trusted,
two fast readers from different families, and the slow model only on the pages where those two
disagree. That design is the first part of the series. What carries over to another team is one rule.

**Before you choose a model for your documents, score it on the part of the page you cannot afford to
lose.** For an archive that is the top and the bottom of a letter. For your documents it may be a table
column, a signature, or a stamp. The leaderboard will not tell you, because it was never asked.

ocr-bakeoff is at [github.com/neutze/ocr-bakeoff](https://github.com/neutze/ocr-bakeoff), MIT licensed.
`uv run ocr-bakeoff samples ./demo` makes a test folder to try it on in a minute.

---

**Fenced out, deliberately:** no document from the archive appears, by name, by text or by
description. Every figure is an aggregate from the evaluation paper or the tool's README. No career
fact appears.
