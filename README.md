# Item Classifier PWA

A lightweight browser app for reviewing geographic candidate locations and assigning land-cover classes. It displays one candidate at a time on a satellite map, stores classification progress in the browser, and exports the updated data as CSV.

## Features

- Loads the 360 candidate records in `candidates.csv`.
- Displays each location in a Leaflet map with Esri World Imagery tiles and a marker at the row's latitude and longitude.
- Keeps the source Google Maps URL from the CSV available through **Open in Google Maps**.
- Supports the classifier's class buttons, back and next navigation, and deleting an unsure row.
- Groups records into batches of 50 and provides previous and next batch controls.
- Stores only class codes (`6`, `4`, `1`, `2`, `0`) or an empty value for unclassifiable items.
- Shows the number classified and the number remaining, including blank-option classifications.
- Lets you update each item's WorldCover value from the standard WorldCover class dropdown; edits are saved locally and synced to Drive when connected.
- Saves progress in browser local storage and exports the current records, including the map URL, to CSV.
- Connects to Google Drive to save progress manually or automatically when moving between batches, and can restore a matching saved dataset.
- Provides **Delete Drive save** to remove the app's saved progress file from Google Drive after confirmation. This does not clear browser progress or change the CSV.
- Provides **Refresh app / clear cache** to unregister this app's service worker and clear its Cache Storage entries while preserving local progress and the Google Drive save.
- Parses quoted CSV fields so commas in map URLs do not split a record incorrectly.
- Shows a loading error and retry option if the CSV cannot be loaded.
- Includes a service worker for caching the app files and a GitHub Actions workflow for GitHub Pages deployment.

## Data format

`candidates.csv` uses these columns:

```text
lon,lat,set,worldcover,class,map_url
```

The app uses `lat` and `lon` to place the map marker. The `worldcover` value can be changed from the selector while classifying, and `map_url` is retained for opening the original location in Google Maps and for CSV export. To start a fresh dataset, replace `candidates.csv` and deploy it; saved local or Drive progress only restores when its dataset fingerprint matches the current CSV.

## Deploy with GitHub Pages

1. Put the package contents in the root of the repository and push them to the `main` branch.
2. In **Settings → Pages → Build and deployment**, set **Source** to **GitHub Actions**.
3. Open the **Actions** tab and check the `Deploy static site to GitHub Pages` workflow.

The included workflow uploads the repository root as the static site. The app uses relative file paths, so it can also be hosted under a project path such as `https://OWNER.github.io/REPOSITORY/`.

## Requirements and notes

- Serve the app from GitHub Pages, another HTTPS host, or localhost. Browsers restrict service workers and local file fetching for pages opened directly using `file://`.
- An internet connection is required for Leaflet from unpkg and for Esri satellite imagery tiles. The classifier data and interface are static files.
- Progress is stored in the current browser and syncs between devices only after using Google Drive save and restore.
- To use Drive sync, enable the Google Drive API in the Google Cloud project for the supplied OAuth client ID. Add the deployed site origin (for example, `https://ngangom01.github.io`) to the OAuth client's authorized JavaScript origins and configure the OAuth consent screen. Drive sync uses the `drive.file` scope and stores `item-classifier-progress.json` in the signed-in user's Drive.
- Drive access tokens are held in memory for the current page session. Use **Connect Google Drive** again after reloading the app.
- The package contains a GitHub Pages workflow, but it has not been deployed or verified against a GitHub repository. The deployment timeout shown in the supplied screenshot requires the detailed Actions deploy-job log to diagnose further.
- The service worker checks for updated same-origin files before using cached copies and removes older app caches during activation, which helps Chrome load new deployments.
