# Malta News — RSS feed mirror

This repository is a CDN-fronted mirror of the Malta News daily RSS feed.

## What it is

The Malta News pipeline (publisher at `~/.hermes/scripts/malta_news_publish.sh`)
publishes an RSS 2.0 feed (`feed.xml`) to the live origin
`https://maltanews.great-site.net/feed.xml`. The origin sits behind a
JS-challenge WAF that prevents plain-text feed readers and validators from
fetching it directly. This mirror solves that by serving the same feed.xml via
the jsDelivr CDN, which has no WAF on the served file path.

## How it gets updated

The publisher (`~/.hermes/scripts/malta_news_publish.sh`) commits and pushes
this repo's `main` branch on each successful publish:

```bash
git -C ~/.hermes/state/maltanews-feed-mirror/ add feed.xml
git -C ~/.hermes/state/maltanews-feed-mirror/ commit -m "..."
git -C ~/.hermes/state/maltanews-feed-mirror/ push origin main
```

The push is fail-soft — if it fails, the email + Discord + FTP-upload paths
still complete normally.

## How to consume

Public RSS URL: `<https://cdn.jsdelivr.net/gh/dejjem/maltanews-feed@main/feed.xml>`

Validation URL: paste the public RSS URL into the W3C feed validator
(`https://validator.w3.org/feed/`). The jsDelivr-served copy is well-formed
RSS 2.0 + Atom self-link, identical to the origin.
echo
cd "$MIRROR_DIR"
ls -la
echo
# Configure git identity so the commit succeeds (gh provides user but per-repo identity
# still needs to be set when committing locally)
git config user.email "dejjem@gmail.com"
git config user.name "Hermes Mark Micallef"
git add feed.xml README.md
git commit -m "Initial mirror: Malta News RSS feed (digest 2026-10-19)" 2>&1 | tail -5
echo
git push origin main 2>&1 | tail -10
