# Use Case: Web-Browsing Research Agent

**YouTube walkthrough:** [Web Browsing Research Agent — System Design #Shorts](https://youtu.be/RJqtwjYqHQ4)

**Design doc:** [docs/DESIGN.md](./docs/DESIGN.md) — architecture, patterns, and why.


**Parent system design:** [07 — Agent Runtime with Hard Containment](../07-agent-runtime-containment.md)

## Users & problem

An agent browses the web to research and cite sources. Page content is an injection surface; egress must not reach internal networks.

## Requirements & SLOs

| Requirement | Target |
|-------------|--------|
| Egress | Allowlisted proxy; deny RFC1918/link-local |
| Untrusted data | Page text labeled untrusted |
| Citations | Keep URLs/titles |
| Caps | Max pages/steps/cost |

## Design (from parent)

```
Model browse(url) → policy + proxy fetch
  → sanitize/truncate HTML → observation (untrusted)
  → model summarizes with citations
  → never elevate privileges from page instructions
```

## Specializations

| Concern | Browse agent choice |
|---------|---------------------|
| Browser | Ephemeral profile; isolated jar |
| Downloads | Quarantine; don’t exec |
| Paywalls/login | Only with user OAuth connector—not random creds |
| Safety | Moderate fetched text ([06](../06-safety-moderation-pipeline.md)) |

## Failure modes

- Prompt injection “ignore policies” → runtime permissions unchanged.
- Metadata IP access → proxy egress rules.
- Infinite browse loops → max_steps + max_wall_time.




## Design walkthrough (opens on GitHub)

> **Watch on YouTube:** [Web Browsing Research Agent — System Design #Shorts](https://youtu.be/RJqtwjYqHQ4)


![Design overview](docs/video/design-overview.gif)

Full narrated video (download): [docs/video/design-overview.mp4](docs/video/design-overview.mp4)

## Run (self-contained POC)

This folder is a **standalone** project (safe to split into its own GitHub repo).

```bash
cd web-browsing-research-agent
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
PYTHONPATH=. python -m uvicorn app.main:app --reload --port 8000
```

```bash
curl -s http://127.0.0.1:8000/health | jq
```

curl -s -X POST http://127.0.0.1:8000/browse -H 'Content-Type: application/json' -d '{"url":"https://example.com"}' | jq
