# BBIC Lab website

English academic website for **BBIC Lab**, with a design adapted from
[Yale NLP Lab](https://github.com/yale-nlp/yale-nlp.github.io), based on the
[Allan Lab template](https://github.com/mpa139/allanlab).

## Pages

- `/` — laboratory introduction, research interests, and paper-sharing sessions
- `/team/` — Principal Investigator and student profiles
- `/publications/` — publications from 2021 onward in Yale's text bibliography layout, with publication-type and year filters alongside title/author search
- `/opportunities/` — research opportunities and contact information
- `/yuqi/` — Yu Qi’s original personal website

The personal website was moved as a complete directory. Its HTML, CSS, JavaScript,
images, publication order, and original PerPLe / Jon Barron acknowledgments are
unchanged. Lab pages use their own styles and scripts under `assets/lab/`.

## Preview locally

No Node, Ruby, Jekyll, or package installation is required. From this directory:

```sh
python3 -m http.server 8000 --bind 127.0.0.1
```

Open `http://127.0.0.1:8000/` and `http://127.0.0.1:8000/yuqi/`.

## Edit lab content

- `site-data/lab.json` — lab name, homepage photo path (`home_photo`), research interests, featured video and cover-issue link (`research_highlight`), contact information, and paper-sharing sessions (`paper_sharing`)
- `assets/lab/paper-sharing/` — presentation files linked from the homepage; each session includes its date, presenter, paper title, presentation path/format, and original-paper URL in `paper_sharing`
- `site-data/publications.json` — publication records and links, with optional thumbnail paths
- `site-data/publications-sources.json` — scope, source coverage, year counts, and version/date reconciliation notes
- `site-data/students.json` — student profiles, displayed four per row on desktop
- `scripts/build_site.py` — page templates and shared navigation/footer
- `assets/lab/lab.css` — lab-specific styles
- `assets/lab/vendor/yale-team.css` — the original Yale Team card styles, scoped to Students
- `assets/lab/vendor/yale-publications.css` — Yale publication-list styles scoped to the Publications page
- `assets/lab/bbic-logo.png` — the supplied laboratory logo, shown in the navigation bar
- `assets/lab/lab-group.jpg` — the supplied group photo on the homepage
- `assets/lab/site.js` — mobile navigation and publication filtering

For a PPTX session, export a PDF to `assets/lab/paper-sharing/` and add its path
as `slides_preview` in that session's data. The homepage shows `Preview (PDF)`
in a new tab alongside `Download (PPTX)` for the original file. PDF presentations
use their existing file for preview. Original presentation files are preserved.
PDF previews are static. The JoSE preview uses the presentation's original poster
images for its four embedded videos; download the PPTX for its interactive media.

After changing data, templates, or lab styles, regenerate the four lab pages:

```sh
python3 scripts/build_site.py
python3 scripts/check_site.py
```

Commit the generated HTML along with source changes. The build script only writes
the four lab pages and never writes inside `yuqi/`. The check script verifies every
original personal-site file against a SHA-256 manifest, local page links, and all
available publication thumbnails. The publication list remains readable without JavaScript.
The editable lab stylesheets have content-based version identifiers in their
links, so rebuilding after a CSS change makes browsers request the updated styles.

The publication bibliography combines the original personal homepage records with
verified DBLP and publisher records, limited to 2021 onward. Exact-title preprint
duplicates use their published version where available. The 2025 Brain-in-the-loop
Supervision preprint is represented by its 2026 Nature Communications publication;
its previous metadata and preprint link remain in the data. Existing thumbnail
assets are retained, while this page uses Yale's text-only publication layout.
Google Scholar access was unavailable during this update, so complete agreement
with that profile has not been verified. Per-record `sources` and the source
summary file document the coverage.

Other initial content comes from the existing personal homepage.
Student names, ordering, and degree labels are supplied by the laboratory.
All student cards currently use the default placeholder photo; collaborators
listed on papers are not treated as laboratory members.

To add a student, append an object to `site-data/students.json` with their
confirmed details. Edit existing objects to update their profiles. All fields
are optional; for example:

```json
{
  "name": "Student name",
  "role": "PhD student",
  "photo": "assets/lab/students/student-name.jpg",
  "homepage": "https://example.com/",
  "interests": ["Research interest", "Another interest"]
}
```

Photo paths are relative to the repository root. Add or remove objects to adjust
the number of profiles; each empty object is an explicitly labeled placeholder.
The Students grid displays four profiles per row on desktop. On smaller screens
it uses Yale's original automatic columns: 150px minimum width with 45px gaps.
The original 150px avatars, typography, interest tags, and hover effect are retained.

## Fork and publication workflow

Development is isolated on `website/lab-homepage` in a local checkout of
`Ybekk/BBIC-Lab.github.io`. Review and preview this version before merging or
deploying anything to `BBIC-Lab/BBIC-Lab.github.io`.

All internal links and assets use directory-relative paths, so the same files work
both at a fork’s project URL (such as `/BBIC-Lab.github.io/`) and at the eventual
organization homepage `https://bbic-lab.github.io/`. No custom domain or upstream
Pages setting is changed by this implementation. `.nojekyll` keeps publication
independent of a Jekyll build.

See [design sources](docs/design-sources.md) for attribution and third-party terms.
