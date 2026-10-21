# Docker Mirror

This repository publishes immutable copies of the third-party container images
used by Lithos Computer automation. Consumers pull the public copies from
GitHub Container Registry without authentication.

The mirror copies existing OCI manifests and layers. It does not rebuild or
modify upstream images. Each source is pinned by digest in `images.json`, and
the workflow fails if the destination digest differs from the source digest.

## Published images

| Upstream | Mirror |
| --- | --- |
| `docker.io/library/buildpack-deps` | `ghcr.io/lithoscomputer/docker-mirror/buildpack-deps` |
| `docker.io/library/node` | `ghcr.io/lithoscomputer/docker-mirror/node` |
| `docker.io/openapitools/openapi-generator-cli` | `ghcr.io/lithoscomputer/docker-mirror/openapi-generator-cli` |
| `docker.io/library/postgres` | `ghcr.io/lithoscomputer/docker-mirror/postgres` |
| `docker.io/library/rust` | `ghcr.io/lithoscomputer/docker-mirror/rust` |
| `docker.io/library/debian` | `ghcr.io/lithoscomputer/docker-mirror/debian` |

The upstream projects retain ownership of their images, software, metadata,
and licenses. This repository provides a distribution mirror only.

## Update an image

1. Select an exact upstream digest that has been public for at least 24 hours.
2. Update its entry in `images.json` and set `notBefore` to the earliest time
   the workflow may copy it.
3. Merge the change to `main`.
4. Confirm that the mirror workflow passes its digest and anonymous-access
   checks.

Moving upstream tags never update this mirror automatically. Consumers should
use the mirrored digest, not a tag.

## Certificate trust gate

Before publishing, inspect each Linux platform's system CA bundle without
executing source-image code. All twelve checked-in eMudhra certificate chains
must fail verification. Missing bundles or inconclusive checks stop publication.
The fixtures come from https://emudhra.we-have-a-problem.com/chains.zip.

This gate does not repair an old source image. Select and verify fixed upstream
images before publication can resume. Java and other runtimes with private trust
stores need separate validation; passing the native CA gate alone does not
establish full runtime coverage.
