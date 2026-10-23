# ScottVanDeWater.com

Plain HTML and one shared stylesheet. No build step, content JSON, dependencies,
or page-specific CSS. Open `docs/index.html` directly in a browser to review locally.

## Repository structure

- `docs/`: the complete public site, including HTML, `styles.css`, images, assets,
  `robots.txt`, `sitemap.xml`, `CNAME`, and `.nojekyll`.
- `Chart/`, `compose.yaml`, and `nginx.conf`: local preview deployment tools.
- `README.md` and `AGENTS.md`: repository documentation and editing instructions.

Publish only `docs/`. Its contents are served at the site's root URL, so links
remain relative and public URLs do not gain a `/docs/` prefix. `.nojekyll` keeps
GitHub Pages from processing these directly editable files with Jekyll.

## Review deployment

The Kubernetes preview is at `https://scott-site.ldev.shadowtechllc.net/` on the
standard HTTPS port. `Chart/` follows the local API chart's public ingress and
`lets-encrypt` certificate issuer, with one nginx Deployment and Service in a
dedicated `scott-site` namespace.

Install or update it:

```sh
helm upgrade --install scott-site ./Chart --namespace scott-site --create-namespace --kube-context microk8s --wait --timeout 2m
```

Only `docs/` is mounted as nginx's public document root from the local `bfml` node.
The nginx configuration is mounted separately. Content edits appear
on refresh without rebuilding an image. `Chart/values.yaml` specifies the node,
source path, image, hostname, ingress class, and certificate issuer. This is a
local development deployment, so the source directory must exist on that node.

After changing `nginx.conf`, restart the pod to reload its mounted configuration:

```sh
kubectl --context microk8s --namespace scott-site rollout restart deployment/scott-site
```

Remove the Kubernetes deployment with:

```sh
helm uninstall scott-site --namespace scott-site --kube-context microk8s
```

Both preview options disable response caching and send a `noindex, nofollow`
header. Hidden files and repository metadata are blocked. Otherwise nginx serves
existing files and returns 404 for missing paths. Neither preview deploys to the
public site.

### Docker alternative

Run `docker compose up -d` to serve `docs/` through nginx over HTTP on host
port 8443. Edits appear on refresh; no image build is needed. The site and nginx
configuration are mounted read-only. Stop it with `docker compose down`.

- Local: `http://localhost:8443/`
- External: `http://scott-site.ldev.shadowtechllc.net:10120/`

This alternative requires the router to forward TCP port 10120 to this host's
port 8443. Compose does not configure that router rule and uses no TLS.

## Editing and publishing

Edit each HTML file directly in `docs/`. Every page uses `styles.css`, the same header/footer
markup, and relative links. Keep the header/footer markup consistent when editing;
only the current navigation link's `aria-current` differs. Assets and bundled font
licenses are in `docs/assets/`. No public email or résumé is included.

The live site at `https://scottvandewater.com/` is hosted on GitHub Pages. The
repository remote is `sevandewater/web`. In the repository's Settings → Pages,
choose **Deploy from a branch**, select the intended publishing branch, and set
the folder to **/docs**. The remote default branch is `master`; confirm the
publishing branch before saving. The publishing-source settings have not been
verified or changed from this checkout. Commit and push `docs/` before selecting
it in GitHub's settings. There is no deployment workflow or build step in this
repository. `docs/CNAME` records the custom domain; preserve the existing domain
setting in GitHub Pages.

Use relative `.html` links for navigation and local-file review. There are no
extensionless or former-directory aliases. Docker Compose and Helm are local
preview tools; GitHub Pages does not run their configuration.

Development and deployment files stay outside `docs/` and are excluded from the
published site when Pages is configured to publish **/docs**. Local nginx previews
also serve only `docs/`.

The homepage includes Person structured data. Each page has its own title,
description, canonical URL, and social metadata. `sitemap.xml` lists only completed,
indexable pages, including Selected Work and its published case studies. Add new
case-study URLs when their content is ready. The 404 page is noindex.

Add future work as plain HTML using the same styles and article markup. Candidate
topics remain cloud infrastructure modernization, agentic engineering workflows,
observability/engineering feedback, and business-critical platform architecture.
Write only verified contributions and outcomes; review confidentiality first.

`docs/privacy.html` describes Google Analytics, GitHub Pages hosting, and locally served
fonts. Analytics currently loads automatically. Account-side analytics retention,
advertising/data-sharing settings, and any needed consent handling remain to be
reviewed. Update the notice when those practices change.
