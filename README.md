# trakt2feed

Generate an Atom feed of recent TV episodes watched on Trakt, using the **public user history API**. No OAuth tokens, refresh tokens, or Python are required.

## Configuration

Create a Trakt API application and obtain its client ID. Set `TRAKT_CLIENT_ID` in your shell, or add it as a GitHub Actions repository secret named `TRAKT_CLIENT_ID`.

The default Trakt username is `davorg`. Override it locally with `TRAKT_USERNAME` if needed. Edit `config.json` to change the feed title, author and published URL.

## Local use

Install dependencies with `cpanm --installdeps .`, then run:

```sh
perl bin/get-history > history.json
perl bin/mkfeed history.json > feed.xml
```

The fetcher retrieves the latest 20 episode viewing events. The generator produces an Atom feed with episode titles, Trakt links, viewing timestamps, and unique history-event IDs (so repeat viewings remain distinct).

## GitHub Pages

The workflow in `.github/workflows/make-feed.yml` runs every two hours and supports manual `workflow_dispatch`. It publishes `feed.xml` using the GitHub Pages artifact/deploy actions.

Configure GitHub Pages to use **GitHub Actions** as its build and deployment source. Add the `TRAKT_CLIENT_ID` repository secret before running the workflow.

After the feed is published, point the existing Dancer2 feed proxy's `tv` URI to its GitHub Pages URL.

The script requires publicly accessible Trakt history. Private history requires OAuth and is not supported by this implementation.
