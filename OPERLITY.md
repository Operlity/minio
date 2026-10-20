# Operlity build of MinIO

This fork publishes the MinIO server image that Operlity Express installs as its object storage.

- **Image:** `ghcr.io/operlity/minio` (public).
- **Built from:** this repository's source, with `Dockerfile.operlity` and `.github/workflows/operlity-image.yml`.
  No upstream binaries are downloaded.
- **Base:** Alpine. It runs as the non-root user `minio` (UID 1000), owns `/data`, and keeps busybox `wget` for
  health checks.
- **Default command:** `server /data --console-address :9001` (S3 API on 9000, console on 9001).
- **Licence:** GNU AGPL v3 (see `LICENSE`). The source for every published image is this public repository at the
  commit named in the image's tags and build summary.

## Publishing a version

1. Push a tag named `operlity-<version>`, for example `operlity-2026.10.20.1`.
2. The workflow builds `linux/amd64` and `linux/arm64`, smoke-tests the result and pushes these tags:
   - `ghcr.io/operlity/minio:2026.10.20.1`
   - `ghcr.io/operlity/minio:sha-<12-char commit>`
3. The run summary shows the digest. Pin it in Operlity Express (PlatformManager,
   `PlatformCoreCatalogue`) as `ghcr.io/operlity/minio:<version>@<digest>`.

A manual run (Actions, then "Operlity image", then Run workflow) publishes only the `sha-<commit>` tag.

After the first publish, make the GHCR package public: open the package settings on GitHub, then
"Change visibility", then Public. Express pulls it anonymously.

## Upstream workflows

The upstream MinIO workflows (and their helper files) were removed from `.github/workflows`. They test against
MinIO's own infrastructure and are not needed to build this image. Only "Operlity image" remains.
`.github/dependabot.yml` keeps its action pins (commit SHAs) current.
