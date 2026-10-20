# Stremio Hentai Hub

Stremio addon with four providers: HentaiGasm, HentaiSmile, HentaiCity, and HentaiMama. Hanime and Hentai.TV are omitted because their catalogs were returning no items.

## Changes in v2.5.4

- HentaiSmile stream discovery is now bound to the selected catalog video's numeric ID across both `/videos/<id>/` and `/embed/<id>/` pages, including when an embed page leads to another player. It extracts player configuration scoped to the matching ID rather than crawling every URL on the page, which could mix in recommendation/ad videos.
- When a player script contains multiple video objects, fallback extraction scans only the object containing the selected ID and its source properties.
- HentaiSmile catalog titles are checked against the URL slug to avoid borrowing the neighboring card's title; placeholder titles fall back to the slug.
- HentaiSmile stream names show the source hostname and duplicate entries from the same host are collapsed.
- Hentaigasm's known-working resolver path is preserved. Existing HentaiCity and HentaiMama resolver logic is unchanged in this patch.
- Hanime and Hentai.TV remain removed.

## Deploy

1. Replace `index.js`, `package.json`, `test.js`, and `README.md` in the repository with the files from the patch ZIP.
2. Push the changes and let the host reinstall dependencies and redeploy.
3. Install the deployment's public HTTPS URL ending in `/manifest.json` in Stremio. A `127.0.0.1` URL in server logs is internal to the server.
4. Check deployment logs if a catalog or stream still fails.

## Tests

Run after dependencies install:

```sh
npm test
npm start
```

Tests cover provider manifest, catalog pagination, route normalization, HentaiMama player parsing, and HentaiSmile player-ID matching, stream labeling, and title matching. The local environment does not have full npm dependencies, so parser tests that require real Cheerio could not be run there. Dependency-independent tests and mocked resolver checks were run. Live playback must still be verified on the deployed host because the build environment could not fetch provider video endpoints.

## Environment variables

`PORT`, `USER_AGENT`, `HTTP_TIMEOUT_MS`, `CACHE_CATALOG_SECONDS`, `CACHE_DETAIL_SECONDS`, `CACHE_STREAM_SECONDS`, `EMBED_DEPTH`, `EMBED_PAGE_LIMIT`, `HENTAIMAMA_BASE`, `HENTAIGASM_BASE`, `HENTAISMILE_BASE`, and `HENTAICITY_BASE`.
