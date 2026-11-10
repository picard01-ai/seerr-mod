# Seerr mod: "Recently Released" streaming sliders

Adds two slider types to Seerr's **Customize Discover → Create Slider** list,
each with a **See more** page:

| Slider type | What it shows |
|---|---|
| **Recently Released Movies (Streaming Services)** | Movies **added to** the services you pick (with a subscription) in the last **30 days**, most popular first. Data from the [Streaming Availability API](https://www.movieofthenight.com/about/api). Without an API key it falls back to TMDB digital/TV release dates (last 90 days). |
| **Recently Released Series (Streaming Services)** | Series with **any episode aired in the last 60 days** on the services you pick, so **new seasons of older shows count**, most popular first. News, talk and soap genres are left out. Data from TMDB. |

## Container settings

| Environment variable | Purpose |
|---|---|
| `STREAMING_AVAILABILITY_API_KEY` | Your Streaming Availability API key. Required for the movie slider to use real "added to service" dates. |
| `STREAMING_AVAILABILITY_RAPIDAPI` | Set to `true` only if your key came from rapidapi.com (default: key from movieofthenight.com). |
| `STREAMING_AVAILABILITY_REFRESH_HOURS` | How long results are reused before asking the API again (default `12`). |
| `STREAMING_AVAILABILITY_MAX_PAGES` | Pages of 25 changes fetched per refresh (default `8`). Lower it if you hit your plan's request limit. |

Each refresh uses one request for the service list (once a day) plus up to
`MAX_PAGES` requests per distinct set of services, so a single slider costs
roughly 2 × 8 = 16 requests a day at the defaults.

## How the image is built

`.github/workflows/build.yml` takes the latest official Seerr release, applies
`mod/recent-streaming.patch`, and publishes
`ghcr.io/<your-github-name>/seerr-mod:latest`. It reruns every Monday, so new
Seerr releases are picked up automatically. If a future Seerr release changes
the code so much that the patch no longer applies, the build fails and your
NAS keeps running the last good image.

## Using it on the Synology

Run `ghcr.io/<your-github-name>/seerr-mod:latest` with the same config folder
and port as the official image. Switching back to the official image
(`ghcr.io/seerr-team/seerr:latest`) undoes the mod; sliders created with the
mod are then just ignored.
