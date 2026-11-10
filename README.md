# Seerr mod: "Recently Released" streaming sliders

Adds two slider types to Seerr's **Customize Discover → Create Slider** list:

| Slider type | What it shows |
|---|---|
| **Recently Released Movies (Streaming Services)** | Movies whose streaming/digital release was in the last **90 days** and that are included with a subscription on the services you pick, most popular first. |
| **Recently Released Series (Streaming Services)** | Series with **any episode aired in the last 60 days** on the services you pick, so **new seasons of older shows count**, most popular first. News, talk and soap genres are left out. |

The date window rolls automatically every time the page loads.
To change 90 / 60 days or the excluded genres, edit the constants at the top of
`src/utils/recentStreaming.ts` inside `mod/recent-streaming.patch`.

## How the image is built

`.github/workflows/build.yml` takes the latest official Seerr release, applies
`mod/recent-streaming.patch`, and publishes
`ghcr.io/<your-github-name>/seerr-mod:latest`. It reruns every Monday, so new
Seerr releases are picked up automatically. If a future Seerr release changes
the code so much that the patch no longer applies, the build fails and your
NAS keeps running the last good image.

## Using it on the Synology

Replace the image of the existing Seerr container with
`ghcr.io/<your-github-name>/seerr-mod:latest`, keeping the same config folder
and port 5055. Switching back to the official image
(`ghcr.io/seerr-team/seerr:latest`) undoes the mod; sliders created with the
mod are then just ignored.
