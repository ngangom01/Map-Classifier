**Project overview**

The Item Classifier PWA helps review geographic candidate locations and assign land-cover classes. It loads 360 records from candidates.csv, displays each location on a satellite map, and provides a link to open it in Google Maps.

The app saves classification progress in the browser and lets users navigate records, delete unsure entries, and export the results to CSV. The CSV parser supports quoted map URLs, and the app shows an error and retry option if data fails to load.

The package includes a service worker for caching and a GitHub Actions workflow for GitHub Pages. The app changes are prepared, but deployment has not been verified. The supplied screenshot shows the deploy job timing out; its detailed log is needed to diagnose that issue.
