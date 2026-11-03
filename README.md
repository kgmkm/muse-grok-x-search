# muse-grok-x-search

> 日本語版はこちら: [README.ja.md](README.ja.md)

A dedicated skill that adds **X (formerly Twitter) search** to Muse.

Out of the box, Muse's social search covers Instagram, Facebook and Threads — but not X. This skill calls the CLI of xAI's coding agent **Grok Build** from Muse, and uses its built-in `x_search` tool to search public posts on X.

## Features

- **Real reactions from X** — Fetches trending posts, replies and quotes with dates, accounts and URLs
- **Read-only by design** — The search prompt explicitly forbids write actions (posting, replying, liking, reposting, following); the skill is for research only
- **No separate API contract needed** — Runs within a Grok Build subscription (SuperGrok / X Premium family plans). *Eligible plans may change, so check the official information when you set it up*
- **Tested in practice** — Verified end to end in the author's environment (a Muse VM, 2026-11-01), from device-code sign-in with an X Premium account to a completed search. One search took about 26 seconds

## Requirements

- Muse (in an environment with shell access)
- The [Grok Build](https://x.ai) CLI (installed with the official installer script)
- An account that can use Grok Build (a SuperGrok / X Premium family plan, or an `XAI_API_KEY`)

## Installation

Just give Muse this repository's URL and ask it to install the skill.

To install manually, place `SKILL.md` and `skill.yaml` in Muse's custom skill folder (`~/workspace/custom-skills/grok_x_search/`).

On first use you need to sign in to Grok Build once. Muse starts a device-code sign-in; open the URL it shows on your own device and approve it with your own X account. Your credentials are stored only inside your own environment.

## Usage

Once installed, just ask Muse in plain words:

- "Check the reactions on X to ○○"
- "Collect the quotes and replies to the post I published last week"
- "How was the ○○ announcement received on X?"

### Use cases

- Checking reactions to your own posts, products or articles
- Gathering early feedback and bug reports right after a release
- Checking first reactions to news and announcements
- Tracking what specific accounts are posting

## Notes

- Searches consume your Grok Build subscription allowance. Scoped searches at a low frequency are recommended
- Grok Build is in beta, so its specifications and eligible plans may change (this document is based on testing as of 2026-11-01)
- Search results are summaries produced by Grok. For important decisions, always check the original posts via their URLs

## Rules of use

- Use it with your own account and your own subscription. Never share your credentials with anyone
- This skill is for research only. Do not use it for write actions such as posting, replying, liking or following
- When quoting other people's posts, keep quotes minimal and credit the source (account and URL)

## Terms review and publication decision

Before publishing this skill, we reviewed X / xAI's terms and the history of similar tools (such as the former Twitter engine in SearXNG) that disappeared. Based on that review, we decided on 2026-11-02 that publishing is acceptable. The research and the reasoning behind the decision are documented in [docs/terms-research.md](docs/terms-research.md) (in Japanese).

## Side quest: SearXNG / RustXNG

We also tested whether a self-hosted metasearch engine could strengthen Muse's search in the same environment. In short: for Japanese local searches such as weekend events, Muse's built-in search was the most practical, and neither SearXNG nor RustXNG has an X search engine. See [docs/findings.md](docs/findings.md) for details.

## License

MIT License ([LICENSE](LICENSE))
