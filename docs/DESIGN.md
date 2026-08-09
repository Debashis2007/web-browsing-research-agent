# Design: Web Browsing Research Agent

**Project:** `web-browsing-research-agent`  
**Parent system design:** `07-agent-runtime-containment.md`

## 1. What this POC demonstrates

Egress allow/deny for browse tool; page text labeled untrusted before summarization.

## 2. Architecture (POC)

```text
POST /browse → deny link-local/RFC1918 → untrusted observation → summarize
```

## 3. Patterns used (and why)

| Pattern | Why used | Where in code |
|---------|----------|---------------|
| Egress deny list | Block SSRF/metadata endpoints. | `DENY_HOSTS` + private IP prefixes. |
| Untrusted observation channel | Prompt injection via pages. | `observation_channel=untrusted`. |
| Citations | Research agents must show sources. | `citations`. |

## 4. Key endpoints

`GET /health`, `POST /browse`

## 5. Tradeoffs / POC limits

Does not actually fetch URLs — returns simulated page text.

## 6. How to run

See the **Run (self-contained POC)** section in [`../README.md`](../README.md).

This folder is self-contained and can be published as its own GitHub repository.

## 7. Design walkthrough video

> **Watch on YouTube:** [Web Browsing Research Agent — System Design #Shorts](https://youtu.be/RJqtwjYqHQ4)
>
> Direct link: **https://youtu.be/RJqtwjYqHQ4**

Also available in-repo:
- GIF preview: [`video/design-overview.gif`](./video/design-overview.gif)
- MP4 download: [`video/design-overview.mp4`](./video/design-overview.mp4)
- Narration script: [`video/narration.txt`](./video/narration.txt)

