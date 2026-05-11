---
name: hermes-tweet
description: Use the native Hermes Tweet plugin for X/Twitter automation through Xquik. Load this when a Hermes Agent workflow needs tweet search, reply reads, account lookups, trend monitoring, or approval-gated actions such as post tweets, post replies, DMs, follows, monitors, and webhooks.
version: 1.0.0
author: Xquik
license: MIT
metadata:
  hermes:
    tags: [hermes-agent, hermes-plugin, twitter, x, tweet-search, xquik]
    related_skills: [hermes-agent]
---

# Hermes Tweet

Hermes Tweet adds a native Hermes Agent toolset for X/Twitter automation through
Xquik. Use it when the task needs structured X search, account reads, tweet
reply reads, trend checks, monitoring, or explicitly approved X account actions.

Repo: https://github.com/Xquik-dev/hermes-tweet

Guide: https://docs.xquik.com/guides/hermes-tweet

Package: https://pypi.org/project/hermes-tweet/

## Use Cases

- Search tweets and summarize X conversations.
- Read tweet replies before drafting a response.
- Look up users and account context.
- Monitor tweets, keywords, trends, and social signals.
- Prepare post tweets, post replies, DMs, follows, monitor updates, and webhook
  changes behind a human approval step.

## Install

Install the plugin directly through Hermes:

```bash
hermes plugins install Xquik-dev/hermes-tweet --enable
```

Or install the published Python package into the Hermes Python environment:

```bash
uv pip install --python ~/.hermes/hermes-agent/venv/bin/python hermes-tweet
hermes plugins enable hermes-tweet
```

Verify the toolset:

```bash
hermes tools list
```

## Configure

Create an Xquik API key, then expose it to Hermes:

```bash
export XQUIK_API_KEY="xq_..."
export HERMES_TWEET_ENABLE_ACTIONS="false"
```

Keep `HERMES_TWEET_ENABLE_ACTIONS=false` for read-only research, monitoring,
and cron workflows. Enable actions only when the workflow has an explicit human
approval step.

Do not paste API keys, X passwords, cookies, or login material into prompts or
tool arguments.

## Tools

| Tool | Purpose |
|------|---------|
| `tweet_explore` | Search the bundled endpoint catalog. No API call. |
| `tweet_read` | Call read-only catalog endpoints. |
| `tweet_action` | Call write-like or private endpoints. Disabled by default. |

Use `tweet_explore` first to find the endpoint path. Use `tweet_read` for tweet
search, replies, user lookups, trend reads, monitors, and audits. Use
`tweet_action` only after the user approves the exact final action.

## Verification

No-action check:

```bash
hermes -z "Use tweet_explore to find tweet search endpoints. Do not call tweet_action." --toolsets hermes-tweet
```

Authenticated read check:

```bash
hermes -z "Use tweet_explore, then read /api/v1/account. Do not call tweet_action." --toolsets hermes-tweet
```

Expected behavior:

- `tweet_explore` can run without an API key.
- `tweet_read` requires `XQUIK_API_KEY`.
- `tweet_action` stays hidden or disabled unless
  `HERMES_TWEET_ENABLE_ACTIONS=true`.

## Agent Workflow

1. Decide whether the request is read-only or action-taking.
2. Use `tweet_explore` to identify the right endpoint path.
3. Use `tweet_read` for searches, replies, user lookup, trends, and monitoring.
4. Summarize the exact search terms, endpoint path, timestamps, and permission
   or rate-limit errors.
5. For post tweets, post replies, DMs, follows, monitor changes, or webhook
   changes, draft the exact action and ask for approval.
6. Call `tweet_action` only after explicit approval.

## Example Prompts

```text
Use tweet_explore to find tweet search. Search X for recent posts about Hermes
Agent plugins and summarize recurring questions. Do not call tweet_action.
```

```text
Use tweet_explore to find the tweet reply read endpoint. Read replies for this
tweet URL and draft a response plan. Do not post anything.
```

```text
Use tweet_read to inspect the thread. Draft a reply for approval. Only call
tweet_action after I approve the exact final text.
```

## Pitfalls

- Long-running Hermes sessions may need reload or restart after `.env` changes.
- Missing API keys should expose only safe discovery behavior.
- Action endpoints are intentionally unavailable unless actions are explicitly
  enabled.
- Do not use browser automation or cookie scraping as a fallback for this skill.
